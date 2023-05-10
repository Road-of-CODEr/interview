# EventLoop

## EventLoop가 무엇인가요?

<details>
<summary>정답 보기</summary>

JS는 단일 스레드 기반의 언어로, 한 번에 하나의 작업만 처리할수 있기 때문에 비동기 작업으로 블록킹 작업을 피한다.

이러한 비동기 작업을 할수 있는 기반이 이벤트 루프에 해당한다.

</details>

## EventLoop의 구조를 설명해 보라

<details>
<summary>정답 보기</summary>

![js architecture](https://res.cloudinary.com/practicaldev/image/fetch/s--I8K4E512--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tg7893fgvd0q8im1fy3s.png)

![nodejs](https://res.cloudinary.com/practicaldev/image/fetch/s--GfdywPiJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/d44bn34eds8o2wwjhlqq.png)

브라우저에서의 이벤트 루프 구현과 Node.js의 구현은 미묘하게 다르다.

### Browser

1. Heap - 함수에서 정의한 모든 개체 참조 및 변수를 저장
2. Call Stack - 코드에서 사용하는 모든 함수는 마지막 함수가 맨 위에 있고 첫 번째 함수가 맨 아래에 있도록 LIFO 방식으로 여기에 쌓임
3. Web API - 이 API는 V8 엔진을 통해 추가 기능을 제공하는 브라우저에서 제공됨. 이러한 API를 사용하는 함수는 이 컨테이너로 푸시되고 웹 API 응답이 완료되면 이 컨테이너에서 pop.
4. Queue - 대기열은 엔진이 더 이상 실행되지 않도록 비동기 코드 응답을 계산하는 데 사용.
   - Macro Task Queue - 이 큐는 DOM 이벤트, Ajax 호출 및 setTimeout과 같은 비동기 기능을 실행하며 작업 큐보다 우선순위가 낮음.
   - Micro Task Queue - 이 대기열은 Promise를 사용하고 Message Queue보다 우선 순위가 높은 비동기 함수를 실행.

### Node.js

1. Event Queue - 스레드 풀이 완료되면 콜백 함수가 실행되어 이벤트 큐로 전송. 호출 스택이 비어 있으면 이벤트가 이벤트 대기열을 통과하고 콜백을 호출 스택으로 보냄.
2. Thread Pool - 스레드 풀은 이벤트 루프에 대해 너무 무거운 작업을 위임하는 4개의 스레드로 구성. I/O 작업, 연결 열기 및 닫기, setTimeout이 이러한 작업의 예.

![Node phase](https://res.cloudinary.com/practicaldev/image/fetch/s--awEoNCT8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mi9i85g0g9kjrap88d6s.png)

- Node Js의 이벤트 루프에는 실행할 콜백의 FIFO 대기열이 있는 여러 단계가 있다. 이벤트 루프가 주어진 단계에 진입하면 대기열이 소진되고 최대 콜백 수가 실행될 때까지 해당 단계 대기열에서 콜백을 작동한 후 다음 단계로 이동.

### Browser vs Node.js

브라우저에는 setImmediate() 함수가 없음.

</details>

## setTimeout, setImmediate를 이벤트 루프와 엮어 동작 방식을 설명해 보라

<details>
<summary>정답 보기</summary>

setTimeout은 timers, setImmediate는 check에 들어간다.

- 하나더> setInterval은 내부적으로 setTimeout과 [상호 호환](https://developer.mozilla.org/ko/docs/Web/API/setInterval#%EB%B0%98%ED%99%98_%EA%B0%92)이 된다. id값을 공유한다.

</details>

## References

- [https://dev.to/jasmin/difference-between-the-event-loop-in-browser-and-node-js-1113](https://dev.to/jasmin/difference-between-the-event-loop-in-browser-and-node-js-1113)
