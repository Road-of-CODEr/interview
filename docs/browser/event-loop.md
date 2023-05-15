# Event Loop란 무엇일까요?

- 이벤트 루프는 태스크가 들어오길 기다렸다가 태스크가 들어오면 이를 처리하고, 처리할 태스크가 없는 경우엔 잠드는, 끊임없이 돌아가는 자바스크립트 내부의
루프입니다.
- 이벤트 루프를 통해 자바스크립트는 싱글 스레드로 동작하면서도 비동기 처리를 할 수 있습니다.

![JS Engine workflow](https://res.cloudinary.com/practicaldev/image/fetch/s--0TQg9sD0--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/ek7ji4zrimozpp2yzk0a.png)

## Event Loop의 구조는 어떻게 이루어져 있나요?

- 이벤트 루프(Event Loop): 콜 스택과 이벤트 큐를 계속해서 모니터링하는 메커니즘입니다. 콜 스택이 비어 있을 때, 이벤트 큐에서 가장 오래된 콜백 함수를 콜 스택으로 이동시키고 실행합니다. 이 과정은 애플리케이션이 종료될 때까지 계속 반복됩니다.

- 이벤트 큐(Event/Callback Queue): 비동기 작업이 완료되면, 그에 대응하는 콜백 함수들이 대기하는 큐입니다. 마이크로 태스크 큐(Micro Task Queue)와 매크로 태스크 큐(Macro Task Queue)로 구분됩니다.

### Micro Task Queue, Macro Task Queue에 대해 설명해주세요

- Macro Task Queue: `Web APIs`(setTimeout, setInterval, setImmediate, requestAnimationFrame 등)의 콜백 함수가 대기하는 큐입니다. Macro Task Queue에 들어있는 콜백 함수는 콜 스택이 비어 있을 때 콜 스택으로 이동되어 실행됩니다.

- Micro Task Queue: `Promise`의 콜백 함수, `MutationObserver`의 콜백 함수가 대기하는 큐입니다. Micro Task Queue에 들어있는 콜백 함수는 콜 스택이 비어 있을 때 즉시 콜 스택으로 이동되어 실행됩니다.

- 우선순위
  - Micro Task Queue에 들어있는 콜백 함수는 Macro Task Queue에 들어있는 콜백 함수보다 우선순위가 높습니다.
  - 1. 콜 스택이 비어 있을 때, Macro Task Queue에 들어있는 콜백 함수가 콜 스택으로 이동되어 실행됩니다.
  - 2. Macro task가 실행된 후, 이벤트 루프는 Micro Task Queue의 모든 태스크들을 처리합니다.
  - 3. Micro Task Queue의 모든 태스크들이 실행된 후, 렌더링 엔진이 업데이트를 처리합니다.
  - 4. 1-3 단계를 반복합니다.

### setTimeout(fn, 0) 코드가 실행되는 과정을 이벤트 루프 구조를 통해 설명해주세요

- setTimeout(fn, 0) 코드가 실행되면, setTimeout 함수는 콜 스택에 쌓입니다.
- setTimeout 함수는 웹 API에 의해 처리되며, 웹 API는 콜 스택에서 setTimeout 함수를 제거합니다.
- setTimeout 함수는 지정된 시간이 지나면 콜백(fn) 함수를 `Macro Task Queue`에 추가합니다.
- 콜 스택이 비어 있을 때, Macro Task Queue에 있는 콜백 함수가 콜 스택으로 이동되어 실행됩니다.

## Event Loop와 callback 함수는 어떤 관계가 있을까요?

- 이벤트 루프는 콜 스택이 비어 있을 때, 이벤트 큐에서 가장 오래된 콜백 함수를 콜 스택으로 이동시키고 실행합니다.

### callback 함수의 개념은 무엇일까요?

- 콜백 함수는 다른 함수의 인자로 넘겨져 다른 함수의 내부에서 호출되는 함수입니다.

### See also

- https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif
