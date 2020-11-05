## AJAX

### 핵심 개념

**JavaScript Coding Style Guide**

- 스타일 가이드에 맞춰 코드를 작성하는 것은 기본을 지키는 것이다.
- 스타일 가이드를 따르지 않는 코드는 매우 어색하며 본질에 접근하는 것을 방해한다.

**What we do?**

우리는 기존에 작성한 '좋아요' 로직을 Ajax 요청으로 변경 할 것이다.

1. 페이지의 새로고침 없이 Client에서 Server에 Axios를 활용해 비동기 요청을 보낸다.
2. Server는 JSON으로 데이터를 응답하고 이 데이터를 활용해 DOM을 조작한다.

**AJAX(Asynchoronous JavaScript And XML)**

서버와 통신하기 위해 XMLHttpRequest 객체를 사용하는 것

1. Google map, Google Search, Gmail 등에 사용된 기술을 설명하기 위해 등장한 개념
   - 구글 지도, 검색 등에서 스크롤을 하거나 입력하는 모든 행위는 '요청'이다.
   - 새로운 기술이 아닌 기존에 존재하던 기술을 설명하기 위한 용어
2. 다음과 같은 특징을 지닌다.
   - reload 하지 않고 요청 작업을 비동기적으로 수행할 수 있다.(사용자 경험 향상)
   - 페이지의 전체가 아닌 일부만을 업데이트 할 수 있다.
   - HTML, JSON, XML 그리고 일반 텍스트 등을 교환할 수 있다.

**XHR(XMLHttpRequest)**

브라우저 내장 객체

1. 서버와 상호작용하기 위해서 사용한다. 전체 페이지의 새로 고침없이 데이터를 받아올 수 있다.
2. 사용자가 하는 것을 방해하지 않고(== 새로고침 발생하지 않음) 페이지의 일부를 업데이트 할 수 있다.
3. 주로 AJAX 프로그래밍에 사용한다.

**How JavaScript works?**

1. Asynchronous
   - 기다려주지 않음
   - 왜 기다려주지 않을까?
2. Single Thread
   - 혼자 일하기 때문에 기다릴 수가 없다.
3. Event Loop
   - Call Stack
     - 함수의 호출을 기록하는 stack 자료구조
     - 한번에 하나의 작업만 처리 할 수 있으며 함수의 처리는 Stack이 다시 비워질 때 까지 계속 이어진다.
   - Web API
     - 브라우저에서 제공하는 API
     - setTimeout, setInerval, XHR
   - Task Queue
     - Callback Function이 대기하는 Queue 자료 구조
     - 전송 순서대로 작업을 처리하기 위해 Queue 자료 구조를 사용함
   - Event Loop
     - Call Stack이 비었으면 Task Queue의 함수를 Call Stack으로 보낸다.