# DOM의 상태관리

## 상태(state)는 어떤 것들이 있는가

<details>
<summary>정답 보기</summary>

큰 의미에서 '지역(local) 상태'와 '전역(global) 상태'가 존재한다.

- 지역 상태는 컴포넌트 내부에만 영향을 미칠수 있으며 특정 객체에서만 접근되고 사용하는 값이다.
- 전역 상태는 다수의 컴포넌트에 영향을 미칠수 있으며 다양한 객체에서 접근되고 사용하는 값이다.

세부적으로는 아래와 같이 고려해볼수 있다.

- 컴포넌트 상태: React와 Vue.js 같은 프론트엔드 라이브러리에서 컴포넌트 상태를 관리한다. 컴포넌트 상태는 컴포넌트 내부에서 관리되며, 컴포넌트의 UI와 관련된 데이터를 저장한다.

- 애플리케이션 상태: Redux와 Vuex 같은 상태 관리 라이브러리에서 애플리케이션 상태를 관리한다. 애플리케이션 상태는 전체 애플리케이션에서 사용되는 데이터를 저장한다.

- 브라우저 상태: 브라우저 상태는 브라우저 내장 객체인 window 객체를 통해 관리된다. 브라우저 상태는 브라우저의 URL, 히스토리, 쿠키 등을 포함한다.

- 서버 상태: 서버 상태는 백엔드에서 관리되며, 데이터베이스, 파일 시스템 등을 통해 관리된다. 서버 상태는 클라이언트에게 전달되는 데이터를 포함한다. 최근엔 react-query나 SWR을 통해 서버 상태를 캐싱하는 방식을 사용하기도 한다.

- 세션 상태: 세션 상태는 서버에서 관리되며, 사용자의 로그인 정보, 세션 ID 등을 저장한다. 세션 상태는 클라이언트와 서버 간의 상호작용을 관리한다.

- 캐시 상태: 캐시 상태는 브라우저나 서버에서 사용되며, 이전에 요청한 데이터를 저장해 빠른 데이터 접근을 가능하게 한다.

  - see also: https://toss.tech/article/smart-web-service-cache

- 로컬 저장소 상태: 로컬 저장소 상태는 브라우저 내장 객체인 localStorage와 sessionStorage를 통해 관리된다. 로컬 저장소 상태는 브라우저에 저장되는 사용자 데이터를 저장한다.

</details>

## 상태를 다루는 방식/철학을 나열하시오

<details>
<summary>정답 보기</summary>

상태를 다루는 방법과 철학은 다양한 프로그래밍 패러다임과 라이브러리, 프레임워크 등에 따라 다르지만, 일반적으로 다음과 같은 방법과 철학들이 사용된다.

- 단방향 데이터 흐름: React와 같은 프론트엔드 라이브러리에서는 단방향 데이터 흐름을 지향한다. 이는 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달하며, 하위 컴포넌트는 이 데이터를 props로 받아 사용한다.

- 불변성: 상태를 변경할 때, 기존 상태를 직접 변경하는 대신 새로운 상태를 만들 경우 이를 불변성(Immutability)이라고 하며, 상태의 불변성을 유지함으로써 상태 변화 추적과 성능 최적화 등을 도모할 수 있다.

- 상태 관리 라이브러리: Redux와 MobX 같은 상태 관리 라이브러리는 상태 관리를 보다 체계적으로 처리할 수 있도록 도와준다. 이러한 라이브러리를 사용하면 상태를 중앙 스토어에서 관리하고, 애플리케이션 전역에서 일관성 있게 상태를 다룰 수 있습니다. (e.g. [Flux](https://haruair.github.io/flux/docs/overview.html)) 최근엔 jotai, recoil 등에서 상태를 atomic 하게 관리하는 방식을 사용하기도 한다.

- 함수형 프로그래밍: 함수형 프로그래밍은 상태를 변경하는 대신, 상태를 입력값으로 받아 새로운 상태를 출력값으로 반환하는 함수를 만드는 방법을 사용한다. 이를 통해 상태 변화를 추적하고, 상태 변경의 영향이 최소화되는 코드를 작성할 수 있다.

- 관찰 가능한 객체 패턴: RxJS와 같은 라이브러리에서는 관찰 가능한 객체 패턴을 사용한다. 이 패턴은 데이터 스트림을 구독하고 변환하는 방식으로 상태를 다룬다.

</details>
