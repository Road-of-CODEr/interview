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

브라우저에서의 이벤트 루프 구현과 Node.js의 구현은 미묘하게 다르다.

### 브라우저

![js architecture](https://res.cloudinary.com/practicaldev/image/fetch/s--I8K4E512--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/tg7893fgvd0q8im1fy3s.png)


- Heap: 함수에서 정의한 모든 개체 참조 및 변수를 저장
- Call Stack: 코드에서 사용하는 모든 함수는 마지막 함수가 맨 위에 있고 첫 번째 함수가 맨 아래에 있도록 LIFO 방식으로 여기에 쌓임
- Web API: 이 API는 V8 엔진을 통해 추가 기능을 제공하는 브라우저에서 제공됨. 이러한 API를 사용하는 함수는 이 컨테이너로 푸시되고 웹 API 응답이 완료되면 이 컨테이너에서 pop.
- Queue: 대기열은 엔진이 더 이상 실행되지 않도록 비동기 코드 응답을 계산하는 데 사용.
   - Macro Task Queue: 이 큐는 DOM 이벤트, Ajax 호출 및 setTimeout과 같은 비동기 기능을 실행하며 작업 큐보다 우선순위가 낮음.
   - Micro Task Queue: 이 대기열은 Promise, MutationObserver를 사용하고 Message Queue보다 우선 순위가 높은 비동기 함수를 실행.
- Event Loop: Call Stack과 Queue를 계속해서 모니터링하는 메커니즘. Call Stack이 비어 있을 때, Queue에서 가장 오래된 콜백 함수를 Call Stack으로 이동시키고 실행.


1. JS엔진의 Call Stack에서 실행된 비동기 함수가 Web API를 호출하고, Web API는 콜백 함수를 Queue(Micro Task Queue, Macro Task Queue)에 넣는다.
     - Promise then() 의 콜백은 Micro Task Queue에 담긴다.
     - setTimeout의 콜백은 Macro Task Queue에 담긴다.
4. Event Loop가 Call Stack과 Queue의 상태를 체크하며, Call Stack이 빈 상태이면 Queue의 콜백을 Call Stack에 넣는다.
5. 이때 Event Loop는 우선적으로 Micro Task Queue를 확인해 콜백을 Call Stack에 먼저 넣는다
6. Micro Task Queue가 비어있다면 Macro Task Queue를 확인해 콜백을 Call Stack에 넣는다


이러한 하나의 flow를 틱(tick) 이라고 한다.
Micro Task Queue의 경우 하나의 틱에서 큐가 비워질 때까지 모든 콜백을 실한다.
Macro Task Queue의 경우 하나의 틱에서 하나의 콜백만 스텍에 옮겨지므로 여분의 콜백이 남을 수 있다.

- 하나더>
rAF의 경우 Miacro Task Queue에 들어간 promise 콜백이 모두 비워질때까지 실행되지 않으므로, 화면이 멈춘다.
rAF의 경우 Macro Task Queue에 들어간 setTimeout 콜백이 하나 비워지고 실행가능하므로, 화면이 멈추지 않는다.


### Node
![Node phase](https://res.cloudinary.com/practicaldev/image/fetch/s--awEoNCT8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mi9i85g0g9kjrap88d6s.png)

Node Js의 이벤트 루프에는 실행할 콜백의 FIFO Queue에 있는 여러 단계(Phase)가 있다. 
이벤트 루프가 주어진 단계에 진입하면 Queue가 소진되고 최대 콜백 수가 실행될 때까지 해당 단계 대기열에서 콜백을 작동한 후 다음 단계로 이동한다.
이러한 하나의 flow를 브라우저와 동일하게 틱(tick) 이라고 한다.
</details>

## setTimeout(fn, 0) 코드가 실행되는 과정을 이벤트 루프 구조를 통해 설명해 보라

<details>
<summary>정답 보기</summary>

- setTimeout(fn, 0) 코드가 실행되면, setTimeout 함수는 콜 스택에 쌓인다.
- setTimeout 함수는 웹 API에 의해 처리되며, 웹 API는 콜 스택에서 setTimeout 함수를 제거한다.
- setTimeout 함수는 지정된 시간이 지나면 콜백(fn) 함수를 `Macro Task Queue`에 추가한다.
- 콜 스택이 비어 있을 때, Macro Task Queue에 있는 콜백 함수가 콜 스택으로 이동되어 실행된다.

</details>


## setTimeout, setImmediate를 이벤트 루프와 엮어 동작 방식을 설명해 보라

<details>
<summary>정답 보기</summary>

브라우저는 setImmediate가 존재하지 않는다.

setTimeout 콜백의 경우 Timer Phase의 Queue에 등록된다.
setImmediate 콜백의 경우 Check Phase의 Queue에 등록된다.
setTimeout, setImmediate의 경우 현재 Phase에 따라 실행되는 순서가 달라진다.

- 하나더> setInterval은 내부적으로 setTimeout과 [상호 호환](https://developer.mozilla.org/ko/docs/Web/API/setInterval#%EB%B0%98%ED%99%98_%EA%B0%92)이 된다. id값을 공유한다.

</details>


## References

- [https://dev.to/jasmin/difference-between-the-event-loop-in-browser-and-node-js-1113](https://dev.to/jasmin/difference-between-the-event-loop-in-browser-and-node-js-1113)
