# DOM의 상태관리

## 상태(state)는 어떤 것들이 있는가

<details>
<summary>정답 보기</summary>

컴포넌트에서 동적인 값을 의미하며, 상태(state)의 종류는 너무 많은거 같다. 거의 보여지는 모든 UI가 state의 집합이다.
state의 변화로 인해서 UI의 변화가 일어나고, 사용자는 state에 따른 다양한 action을 수행한다.
(ex. list를 보여주는데만 해도, loading 상태, list값, fail 상태 와 같은 다양한 상태가 존재한다)

</details>

## 상태를 다루는 방식/철학을 나열하시오

<details>
<summary>정답 보기</summary>

연관있는 상태를 하나의 객체(상태 그룹?) 형태로 관리한다.
이렇게 관리하면 어디에 쓰이는 개념인지 명확히 할 수 있다.
(ex. user의 name인지, group의 name인지...)

변경사항에 따라 값에 변화가 생기는 경우 객체 내부의 값만 신경쓰면 되므로 쉽게 관리할 수 있다.

</details>
