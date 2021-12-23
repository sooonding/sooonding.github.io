---
# []는 카테고리 명 title을 적지 않으면 파일명이 올라간다.
title: 'SENS를 이용한 SMS 예약 메시지 전송하기'
#excerpt는 description
excerpt:
# 대괄호 안에 들어가는 카테고리 : 목차
categories:
  - 'javaScript'
tags:
  - [javaScript]
# toc는 우측 테이블
toc: true
# 우측 테이블 내용이 따라 오는것
toc_sticky: true
# 우측 테이블 제목
toc_label: 'NAVER SENS 예약 메시지 전송하기'

toc_icon: 'cookie-bite'
# 처음 작성날짜
date: 2021-12-23
# 수정날짜
last_modified_at: 2021-12-23
---

금일 업무를 진행하면서 naver SENS api를 이용하여 예약메시지 전송하는 기능을 하며 잊지 않기 위해 기록을 남깁니다.

### 항상 api를 사용하기 위해 공식문서를 이용!

문서에서 요청 Body를 먼저 살펴보면

```js
{
    "type":"(SMS | LMS | MMS)",
    "contentType":"(COMM | AD)",
    "countryCode":"string",
    "from":"string",
    "subject":"string",
    "content":"string",
    "messages":[
        {
            "to":"string",
            "subject":"string",
            "content":"string"
        }
    ],
    "files":[
        {
            "name":"string",
            "body":"string"
        }
    ],
    "reserveTime": "yyyy-MM-dd HH:mm",
    "reserveTimeZone": "string",
    "scheduleCode": "string"
}
```

위에서 우리가 필요한 것은 reserveTimeZone에 대한 값을 문자열 행태로 값을 주어야 합니다.

그럼 해당 값을 주기위해 레이아웃을 구성합니다.

### 1. 레이아웃 구성

view를 만드는 부분은 Pug를 이용하여 구성하였습니다.

#### **구성한 로직 view**

![view](https://images.velog.io/images/sooonding/post/8462d195-01d6-49b3-945a-737778861c96/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-12-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.44.45.png)

**layout**

```js
  li#date
    label.id(for='Schedule') Schedule
    input(type='radio' name='Schedule' value='now')
    label(for='now') Send now
    input(type='radio' name="Schedule" value='later')
    label.hovering(for='later') Schedule for later
    div.scheduleBox.hideScheduleBox
      input#day(type='date' name='day' value='')
      span -
      input#time(type='time' name='time' value='')
      div.comment 예약시간이 현재시간의 10분이내 일 경우 10분뒤에 예약 메시지가 발송됩니다.
```

**기능관련 로직**

```js
$(document).ready(function () {
  $("input:radio[name='Schedule']").on('click', function () {
    const value = $("input:radio[name='Schedule']:checked").val();
    if (value === 'now') {
      $('.scheduleBox').addClass('hideScheduleBox');
      $('.scheduleBox').val('');
    }
    if (value === 'later') {
      $('.scheduleBox').removeClass('hideScheduleBox');
    }
  });
  /* 예약 메시지 관련 로직 */
  $('.hovering').hover(
    function () {
      $('.comment').show('slow');
    },
    function () {
      $('.comment').hide('slow');
    }
  );
});
```

**스타일**

```js
.scheduleBox {
  display: inline-block;
  margin-left: 2em;
  position: relative;
}

.hideScheduleBox {
  display: none;
}

#day,
#time {
  background: #f7f7f7;
  border-style: none;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #cbd6e2;
  appearance: none;
  background-color: #f5f8fa;
}

.scheduleBox span {
  padding: 0px 10px;
}

.comment {
  position: absolute;
  top: -57px;
  font-size: 11px;
  left: -162px;
  background-color: #ccc;
  color: black;
  border: solid 1px grey;
  padding: 10px;
  padding-left: 0;
  padding-right: 0px;
  padding: 6px;
  opacity: 0.6;
  border-radius: 4px;
}

```

해당 코드는 제이쿼리와 자바스크립트를 이용하였으며, `name이 Schedule`인 element에 클릭 이벤트를 주었으며, 토글화를 위해 해당 라디오 밸류에 따라 날짜를 선택하는 `input` 노출에 대한 로직을 추가하였습니다.

해당 포스트에서는 view로 데이터를 보내는 코드보다는 naver SENS api를 사용한 로직 기능이 더 중요하기에 view에 대한 코드 설명은 이렇게 끝내겠습니다.

### 2. SENS api 사용하기

#### **SENS를 api를 이용한 코드**

**해당 기능에 관련된 코드만 축소하여 작성하였습니다.**

```js
const moment = require('moment');
require('moment-timezone');
moment.tz.setDefault('Asia/Seoul');
const today = moment();


const sendSMS = async (name, email, fromPhoneNumber, smsID, phoneNumber, message, imgName, msgtype, imgFile, title, inputTime, inputDate) => {
  const url = `https://sens.apigw.ntruss.com/sms/v2/services/${smsID}/messages`;

  const method = 'POST';

  let reserveDate = ``;

  if (inputDate && inputTime) {
    reserveDate = `${inputDate} ${inputTime}`;
  } else {
    reserveDate = '';
  }

  // NOTE 날짜에 대한 조건 분기처리 로직
  const timeZone = (inputTime = null, reserveDate = null) => {
    if (!inputTime && !reserveDate) {
      reserveDate = null;
    } else {
      let t = inputTime.slice(3, 5);
      let o = today.format('HH:mm').slice(3.5);
      let diff = t - o;
      if (t === o) {
        reserveDate = null;
      } else if (inputTime !== today.format('HH:mm') && diff < 11) {
        reserveDate = moment(reserveDate).add(11, 'm').format('yyyy-MM-DD HH:mm');
      } else {
        reserveDate = reserveDate;
      }
    }

    return reserveDate;
  };

  // NOTE reserveTime을 보내기 위한 로직
  const sendObject = {
    type: smsType,
    countryCode: '82',
    subject: title ? title : '제목없음',
    from: fromPhoneNumber,
    content: `${message}`,
    messages: [
      {
        to: `${phoneNumber}`,
      },
    ],
    files: fileYn(smsType, imgName),
    reserveTime: timeZone(inputTime, reserveDate),
  };

  const sendString = JSON.stringify(sendObject);

  try {
    const response = await axios.post(url, sendString, {
      headers: headers,
      timeout: 5000,
    });

    responseData = response.data;
  } catch (err) {
    responseData = {
      statusCode: -1,
      statusName: err.toString(),
    };
  }

  return responseData;
};
```

sendSMS라는 함수의 인자로 `inputTime,inputDate`를 받아옵니다. 이 키들은 view에서 input으로 작성할 때 받아온 값들이며 `reserveTime`키에 밸류는 `timeZone`이라는 함수로 호출한 출력값이 들어 가게 작성하였습니다.

`timeZone` 함수로 따로 뺀 이유는 첫째 `inputTime,inputDate`를 명세에 맞게 조합하여 값으로 출력 하였는데, 이상하게도 <span style="color:red">현재 시간 이후 10분 이내의 값들을 반환하여 api 통신을 하였을때 400 에러가 나는 문제가 발생하였으며 </span> 둘째 `reserveTime`이라는 값은 빈문자열을 반환해도 에러가 나서 아예 객체의 키를 없애기 위해 함수로 따로 분리시켰습니다.

`timeZone` 함수를 보면 인자로 input에서 받아온 `inputTime`과 time과date를 조합한 `reserveDate`를 넘겨줍니다.
사용자는 즉시전송과 예약전송으로 나뉘기 때문에 "바로전송"이면 두 인자의 값이 없기에 값이 없는 조건을 태워 `reserveTime`를 없애기 위해 값을 `null` 처리 해줍니다.

"예약전송"이라면 <span style="color:red">현재 시간 이후의 10분 사이의 값이 에러가 나니</span> +10분을 더해주기 위한 조건을 만들었습니다.

해당 조건은 입력된 시간 값(inputTime)과 `moment`라이브러리를 이용하여 현재의 "시간"값의 대한 차이의 값을 구해줍니다.
입력값과 현재 값이 같다면 그것은 즉시발송이기 때문에 즉시발송의 조건과 같이 값을 `null` 처리합니다.

10분 이내의 값들은 해당 시간들의 차이가 11보다 작다라는 기준하에 moment의 `add` 메서드를 이용하여 11분을 더해줘 값을 담습니다.


### 배운점

외부 api를 통해 예약 메시지를 보내는 경험은 처음이었는데 생각보다 도큐먼트 명세가 잘 나와 있어서 값에 대한 전달은 어렵지 않았습니다.
하지만 현재시간 이후의 10분 이내의 시간들을 반환했을 때 `400`에러가 나는 점은 추후에 다시 해결하고 포스팅을 해야 될 거 같습니다.







