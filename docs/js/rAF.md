# rAF(Request Animation Frame)

## rAF은 무엇일까요?

<details>
<summary>정답 보기</summary>

Web API. 1초에 60회(60fps)의 주기를 가지고, 다음 리페인트(렌더링) 전에 호출되는 함수.

</details>

## 어떤 상황에서 사용할까요?

<details>
<summary>정답 보기</summary>

화면 애니메이션이나 요소의 위치 변화같은 렌더링 로직에서 사용한다.

</details>

## rAF가 Event Loop 구조 내에서 실행되는 과정을 설명해주세요

<details>
<summary>정답 보기</summary>

1. task queue의 메서드가 event loop에 의해 call stack에서 실행.
2. 콜백 메서드가 rAF 수행하는 내용을 가짐
3. Event Loop가 다음 렌더링 전에 화면 갱신이 필요하다 판단
4. 브라우저 렌더링 발생

</details>

## setInterval과 rAF는 어떤 차이가 있을까요?

<details>
<summary>정답 보기</summary>

setInterval의 경우 프레임과는 무관하게 call stack이 비워진 경우에 실행된다.

그렇기 때문에 1프레임(16.6ms)에 여러번 실행될 수 있다.
그렇기 때문에 1프레임(16.6ms)에 실행이 안 될 가능성도 있다 --> 프레임 누수
(프레임 누수의 경우 유저가 애니메이션을 부드럽지 않다고 느낄 가능성이 있다.)

rAF의 경우 1프레임에 1회 실행되기 때문에 자연스러운 애니메이션이나 렌더링이 가능하다.
그러므로 화면 애니메이션이나 요소의 위치 변화같은 렌더링 로직은 setInterval보다 rAF가 적합하다.

</details>
