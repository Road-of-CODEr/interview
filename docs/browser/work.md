# 브라우저의 동작

## 브라우저의 주소창에 주소를 입력하면 어떤 일들이 일어나는가?

<details>
<summary>정답 보기</summary>

## 브라우저 렌더링

### 시작하기 전에

#### _브라우저란?_

사용자가 선택한 자원을 서버에 요청하고 표시한다.

이 때 자원은 HTML, PDF, Image 등 다양한 형태를 띄며 자원의 주소는 URL(Uniform Resource Identifier)에 의해 정해진다.

#### _브라우저의 기본 구조_

<img width="978" alt="image" src="https://user-images.githubusercontent.com/23524849/232105535-2918bf97-9661-4191-974a-fff66ea33aeb.png">

- 사용자 인터페이스: 주소 표시줄 등 요청한 페이지 외의 나머지 모든 부분
- 브라우저 엔진: 사용자 인터페이스와 렌더링 엔진 사이의 동작 제어
- **렌더링 엔진**: 사용자가 요청한 콘텐츠를 화면에 표시(크롬은 각 탭마다 별도의 렌더링 엔진 인스턴스를 유지)
- 통신: HTTP 요청과 같은 네트워크 호출
- 자바스크립트 해석기: 자바스크립트 코드를 해석하고 실행
- UI 백엔드: 콤보 박스와 창 등의 기본적인 장치를 그림. OS 사용자 인터페이스 체계를 사용함.
- 자료 저장소: HTML5에서 웹의 데이터를 클라이언트에 저장(Local Storage, Session Storage, Cookie, Web SQL Database)

### 브라우저 렌더링 전 데이터 받아오기

#### 주소를 통해 서버 찾아가기

주소창에 주소를 입력하면 URL [https://www.naver.com] 주소 중 도메인 이름에 해당하는 [naver.com] 를 DNS 서버에서 검색한다.

이때 [퓨니코드](https://namu.wiki/w/%ED%93%A8%EB%8B%88%EC%BD%94%EB%93%9C)인지 확인하는 과정이 존재한다.

> xn—…

캐싱 된 DNS 기록들을 먼저 확인하여 만약 해당 도메인 이름에 맞는 IP 주소가 존재하면 그 주소를 바로 반환한다.

일치하는 IP 주소가 존재하지 않으면 DNS 서버 요청

#### DNS 서버 요청

가장 가까운 DNS 서버에서 해당 도메인 이름에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달한다.

DNS 서버가 호스팅하고 있는 서버의 IP 주소를 찾기 위해 DNS Query를 전달한다.

현재 DNS 서버에 원하는 IP 주소가 존재하지 않으면 다른 DNS 서버에 방문하는 과정을, 원하는 IP 주소를 찾을 때까지 반복한다.

해당 도메인 이름에 맞는 IP 주소로 변환하는 과정은 점(.)을 기준으로 계층적으로 구분하여 구성된다.

뒤에서부터 해당 도메인 이름에 맞는 지역 DNS를 탐색하여 root DNS 서버가 나올 때까지 탐색한다.

이를 Recursive Query 라고 한다.

<img width="1178" alt="image" src="https://user-images.githubusercontent.com/23524849/232105885-436d5ff4-4d54-429c-91a6-0d5fcab35042.png">

#### HTML 문서 요청

전달받은 IP 주소를 이용하여 웹 브라우저는 웹 서버에게 해당 웹 사이트에 맞는 HTML 문서를 요청한다.

해당 HTTP 요청 메시지는 TCP/IP 프로토콜을 사용하여 서버로 전송된다.

- preflight: 단순한 get query 가 아니면 브라우저에서 options를 먼저 요청
- upgrade 헤더 고려(ws://)

이 때 웹 서버에서 모든 로직 처리 및 데이터 관리를 하게 되면 과부하가 일어날 가능성이 높다.

때문에 WAS에서 페이지의 로직이나 데이터베이스의 연동 작업 등을 처리하여 웹 서버로 전송한다.

#### 웹 브라우저에 결과 전달

웹 서버는 status code와 함께 서버 요청에 따른 결과 및 상태를 전달한다.

<img width="915" alt="image" src="https://user-images.githubusercontent.com/23524849/232106234-fc7eca1c-2f36-4eb2-99e6-423c868ff42d.png">

### 브라우저 렌더링

#### DOM Tree 빌드

통신을 통해 받아온 HTML 파일은 바이트 형태로 전달된다.

바이트 → 문자 → 토큰 → 노드 → 객체 모델로 파싱하여 DOM Tree를 최종 출력한다.

파싱 도중에 script 태그를 만나면 JS코드를 실행하기 위해 파싱을 중단한다.

중단시키지 않으려면 defer 속성을 추가해주면 된다.

- defer: DOM 생성 직후 JS 파싱과 실행
- async: HTML 파싱, JS 파일 로드를 동시에 진행

이론적으로 스타일 시트는 DOM Tree를 변경하지 않기 때문에 문서 파싱을 기다리거나 중단할 이유가 없음

하지만 스크립트가 문서를 파싱하는 동안 스타일 정보를 요청한다면 → 스타일이 파싱되지 않은 상태라면 스크립트는 잘못된 결과를 내놓기 때문에 문제를 야기함

렌더링 엔진별 다른 정책으로 운영

- 파이어폭스의 게코 엔진은 로드 중이거나 파싱 중인 스타일 시트가 있는 경우 모든 스크립트의 실행을 중단
- 사파리/크롬의 웹킷 엔진은 로드되지 않은 스타일 시트 중 문제가 될만한 속성이 있을 경우 스크립트를 중단

#### CSSOM Tree 빌드

DOM Tree와 동일하게 CSS 파일을 CSSOM Tree로 최종 출력한다.

#### Render Tree 생성

DOM 과 CSSOM 을 결합하여 Render Tree를 생성한다.

렌더러는 DOM 요소에 부합하지만 1:1로 대응하는 관계는 아니다.

예를 들면 <head>, display: ‘none’ 는 렌더 트리에 나타나지 않는다. (visibility: ‘hidden’ 은 나타난다)

필요한 노드만 선택하여 페이지를 렌더링하는 데 사용한다.

#### Layout → Paint

Render Tree의 모든 노드들의 정확한 위치와 크기를 계산한다.

Layout에서 계산된 값들을 기반으로 요소들을 실제 픽셀로 변환하여 화면에 출력한다.

특정 액션과 이벤트에 따라 HTML 요소의 스타일이 변했을 때 크기나 위치를 변경해야 한다.

이때 CSSOM을 새로 만들어야 하기 때문에 Reflow & Repaint 현상이 발생함.

- Reflow: 주요 렌더링 경로의 모든 단계를 모두 재실행함
  - position, display, width, float, height, font-family, top, left, font-size, font-weight, line-height, min-height, margin, padding, border 등
- Repaint: 레이아웃 단계는 건너뛰기 때문에 리플로우 작업보다는 빠른 편임
  - background, background-image, background-position, border-radius, border-style, box-shadow, color, ling-style, outline 등
- Reflow와 Repaint 과정을 피하기 위해서는 GPU 가속 필요: Composition
  → **transform, opacity** 사용 → …3D… translate3D

</details>
