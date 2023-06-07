# rAF(Request Animation Frame) 은 무엇일까요?

<details>
<summary>정답 보기</summary>

- window.requestAnimationFrame()은 브라우저에게 수행하기를 원하는 애니메이션을 알리고 다음 리페인트가 진행되기 전에 해당 애니메이션을 업데이트하는 함수를 호출하게 합니다. 이 메소드는 리페인트 이전에 실행할 콜백을 인자로 받습니다.
- 브라우저에서 애니메이션을 효율적으로 업데이트 할 수 있는 API 입니다. 좀더 부드러운 애니메이션 표현이 가능해집니다.
- Web API. 1초에 60회(60fps)의 주기를 가지고, 다음 리페인트(렌더링) 전에 호출되는 함수입니다.

```javascript
function animate() {
  // 애니메이션 로직 작성
  // ...

  // 다음 애니메이션 프레임을 예약
  requestAnimationFrame(animate);
}

// 첫 애니메이션 프레임을 예약
requestAnimationFrame(animate);

// cancel animation frame
const id = requestAnimationFrame(animate);
cancelAnimationFrame(id);
```

</details>

## 어떤 상황에서 사용하는 Web API일까요?

<details>
<summary>정답 보기</summary>

- 애니메이션 효과를 만들거나 타이머를 만드는 등 시각적 업데이트를 수행할 때 사용할 수 있습니다.

- 브라우저는 리플로우, 리페인트 과정을 거쳐서 리렌더링을 하게된다. 아직 이전 리렌더링이 끝나지도 않았는데 다음 애니메이션 로직을 실행하도록 명령을 내린다면 애니메이션이 의도한대로 부드럽게 움직이지 않는다.

  - requestAnimationFrame은 위와 같은 문제를 해결해준다. 리페인트 과정이 끝난 후 적용할 애니메이션을 requestAnimationFrame의 콜백으로 넣어주면 된다.

</details>

### rAF(RequestAnimationFrame)이 Event Loop 구조내에서 실행되는 과정을 설명해주세요

<details>
<summary>정답 보기</summary>

1. requestAnimationFrame이 호출되면, 이 함수에 전달된 콜백 함수는 브라우저 웹 API에 의해 관리되는 애니메이션 프레임 준비 큐에 추가됩니다.

2. 화면 갱신 주기가 오면, '애니메이션 프레임 준비 큐'의 콜백 함수들은 브라우저에 의해 '콜백 큐'로 이동됩니다.

3. 이벤트 루프에 의해 콜스택이 비어있을때 콜백 큐의 콜백 함수가 콜스택으로 이동되어 실행됩니다.

만약 또 다른 rAF 호출이 콜백 내에서 이루어진다면, 이 과정은 계속 반복됩니다.

</details>

### setInterval과 rAF(RequestAnimationFrame)은 어떤 차이가 있을까요?

<details>
<summary>정답 보기</summary>

1프레임(16ms)마다 호출이 보장 되는지 여부가 가장 큰 차이점입니다.

`setInterval`을 사용한 경우엔 프레임과 무관하게 실행되기 때문에 이벤트루프가 이벤트 큐의 setInterval callback을 가져와서 처리하는데 지연이 생길 수 있고, 그로인해 렌더링이 늦어져 화면이 끊기는 현상이 발생할 수 있습니다. 반대로 1프레임에 여러번 실행될 수도 있다.

`RequestAnimationFrame`은 브라우저가 화면 주사율에 맞춰 리페인트 이전에 콜백을 실행시킴으로써 렌더링이 끊기는 현상을 방지할 수 있습니다.
또한 rAF는 콜백 함수가 실행되는 주기를 브라우저에게 맡기기 때문에, 브라우저가 비활성화되어 있거나 탭이 백그라운드에 있을 때는 콜백 함수가 실행되지 않습니다

다만 실행 시간이 1프레임(보통 16ms)가 넘는 경우엔 렌더링이 끊기는 현상이 발생할 수 있습니다.

</details>

#### 참고한 문서

- https://developer.mozilla.org/ko/docs/Web/API/window/requestAnimationFrame
- https://inpa.tistory.com/entry/%F0%9F%8C%90-requestAnimationFrame-%EA%B0%80%EC%9D%B4%EB%93%9C#%EB%94%94%EC%8A%A4%ED%94%8C%EB%A0%88%EC%9D%B4_%EC%A3%BC%EC%82%AC%EC%9C%A8%EC%97%90_%EB%A7%9E%EA%B2%8C_%ED%98%B8%EC%B6%9C_%ED%9A%9F%EC%88%98
