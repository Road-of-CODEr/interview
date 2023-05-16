# requestAnimationFrame 메서드

window.requestAnimationFrame()은 브라우저에게 수행하기를 원하는 애니메이션을 알리고 다음 리페인트가 진행되기 전에 해당 애니메이션을 업데이트하는 함수를 호출하게 한다. 이 메소드는 리페인트 이전에 실행할 콜백을 인자로 받는다.

```js
window.requestAnimationFrame(callback);
```

## requestAnimationFrame의 원리

1. requestAnimationFrame은 매개변수로 리페인트 이전에 실행할 콜백 함수를 인자로 받는다.
2. 브라우저는 리플로우, 리페인트 과정을 거쳐서 리렌더링을 하게된다. 아직 이전 리렌더링이 끝나지도 않았는데 다음 애니메이션 로직을 실행하도록 명령을 내린다면 애니메이션이 의도한대로 부드럽게 움직이지 않는다.
3. requestAnimationFrame은 위와 같은 문제를 해결해준다.
4. 리페인트 과정이 끝난 후 적용할 애니메이션을 requestAnimationFrame의 콜백으로 넣어주면 된다.

```js
const canvas = document.querySelector(".canvas");
const context = canvas.getContext("2d");
function draw() {
  context.arc(10, 150, 10, 0, Math.PI * 2, false);
  context.fill();
  console.log("그렸다!");

  requestAnimationFrame(draw);
}
draw();
```

## cancelAnimationFrame

```js
const myReq = requestAnimationFrame(callback);
cancelAnimationFrame(myReq);
```

setTimeout의 clearTimeout과 같이 requestAnimationFrame의 동작을 멈추는 메서드.
