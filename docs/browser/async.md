# 브라우저의 비동기 작업

## 브라우저에서 비동기로 처리하는 것들은 어떤 것이 있는가?

<details>
<summary>정답 보기</summary>

### Web APIs

- 타이머
  - setTimeout, setInterval, setImmediate
- HTTP 요청(XMLHttpRequest)
  - async 속성이 걸려있는 스크립트는 파일의 다운로드가 비동기로 된뒤 즉시 실행
  - defer 속성은 async와 같으나 파일 실행이 HTML 파싱이 완료된 다음에 실행
- DOM
  - 이벤트 핸들러
- console에 window 입력시 확인 가능한 브라우저 내장 객체
- Callback Queue
  - Task Queue, Microtask Queue, Animation Frames

> 크롬 웹 브라우저는 V8 JS 엔진이 탑제되어있다. V8엔진은 하나의 Heap, Stack만 가지고 있기 때문에 한번에 하나의 일 밖에 하지 못하는 싱글 스레드의 특징을 가진다.
이는 비동기적 업무 수행이 불가능함을 의미한다. V8엔진이 하지못하는 비동기는 브라우저의 Web APIs, Callback Queue, Event Loop를 통해 가능하다.
Web APIs의 비동기 메서드를 활용해서 그 결과를 Callback Queue에 쌓으면 Event Loop가 V8 엔진의 Stack이 비어진 시점에 Callback Queue에 존재하는 업무를 Stack에 쌓는다.
![web-browser](https://velog.velcdn.com/images/wlwl99/post/e40f3421-ae1f-4c49-9d4b-10ab22c79278/image.png)

</details>
