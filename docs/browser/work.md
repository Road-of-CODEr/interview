# 브라우저의 동작

## 브라우저의 주소창에 주소를 입력하면 어떤 일들이 일어나는가?

<details>
<summary>정답 보기</summary>

### _브라우저란?_

사용자가 선택한 자원을 서버에 요청하고 표시한다.

이 때 자원은 HTML, PDF, Image 등 다양한 형태를 띄며 자원의 주소는 URL(Uniform Resource Identifier)에 의해 정해진다.


### _브라우저의 기본 구조_

<img width="978" alt="image" src="https://user-images.githubusercontent.com/23524849/232105535-2918bf97-9661-4191-974a-fff66ea33aeb.png">

- 사용자 인터페이스: 주소 표시줄 등 요청한 페이지 외의 나머지 모든 부분
- 브라우저 엔진: 사용자 인터페이스와 렌더링 엔진 사이의 동작 제어
- **렌더링 엔진**: 사용자가 요청한 콘텐츠를 화면에 표시(크롬은 각 탭마다 별도의 렌더링 엔진 인스턴스를 유지)
- 통신: HTTP 요청과 같은 네트워크 호출
- 자바스크립트 해석기: 자바스크립트 코드를 해석하고 실행
- UI 백엔드: 콤보 박스와 창 등의 기본적인 장치를 그림. OS 사용자 인터페이스 체계를 사용함.
- 자료 저장소: HTML5에서 웹의 데이터를 클라이언트에 저장(Local Storage, Session Storage, Cookie, Web SQL Database)


### 브라우저의 주소창에 주소를 입력하면 어떤 일들이 일어나는가?

#### 해당 URL의 IP 주소를 찾기 위해 캐시에서 DNS 기록을 확인한다. 이때 [퓨니코드](https://namu.wiki/w/%ED%93%A8%EB%8B%88%EC%BD%94%EB%93%9C)인지 확인하는 과정이 존재한다.

URL에는 고유의 IP 주소가 있다. 하지만 이는 123.456.789.123 처럼 숫자로 이루어지기 때문에 외우기가 어렵고, 이를 편리하게 위해 DNS 라는것이 존재한다.

DNS(Domain Name System)가 자동으로 URL과 IP 주소를 매핑해주기 때문에 우리는 어려운 IP 주소 숫자를 외우지 않아도 된다.

그렇기에 입력한 URL의 IP주소를 찾기위해 캐시에서 해당 URL을 방문한 기록이 있는지 DNS 기록을 확인해본다.

- 브라우저 캐시: 내가 이전에 방문한 웹사이트의 DNS 기록이 있는지 찾는다.
- OS 캐시: OS에서 시스템 호출을 통해 DNS 기록이 있는지 찾는다.
- 라우터 캐시: 라우터에서 DNS 기록이 있는지 찾는다.
- ISP 캐시(Internet Service Provider): ISP는 DNS 서버를 가지는데, 해당 서버의 DNS 기록이 있는지 찾는다.


#### 요청한 URL이 캐시에 없다면 DNS Query로 URL을 호스팅하는 서버의 IP 주소를 찾는다.

DNS 서버가 호스팅하고 있는 서버의 IP 주소를 찾기 위해 DNS Query를 전달한다.

현재 DNS 서버에 원하는 IP 주소가 존재하지 않으면 다른 DNS 서버에 방문하는 과정을, 원하는 IP 주소를 찾을 때까지 반복한다.

해당 도메인 이름에 맞는 IP 주소로 변환하는 과정은 점(.)을 기준으로 계층적으로 구분하여 구성된다.

뒤에서부터 해당 도메인 이름에 맞는 지역 DNS를 탐색하여 root DNS 서버가 나올 때까지 탐색한다.

이를 Recursive Query 라고 한다.

<img width="1178" alt="image" src="https://user-images.githubusercontent.com/23524849/232105885-436d5ff4-4d54-429c-91a6-0d5fcab35042.png">


#### 브라우저가 해당 서버와 TCP 연결을 시작한다.

올바른 IP 주소를 찾으면 정보를 전송해 서버와의 연결을 구축한다.

HTTP 요청에서는 일반적으로 TCP 전송 프로토콜을 이용해 연결을 구축한다.

3-way handshake 라는 연결 과정을 통해 SYN(연결 요청), ACK(승인)을 주고받으면 연결이 구축된다.
(클라이언트에서 SYN 패킷을 보내고, 서버에서 SYN-ACK 패킷을 보내고, 클라이언트가 다시 ACK 패킷을 보내는 과정)


#### 데이터 암호화 통신을 위한 설정이 필요하다.

주소가 HTTPS로 시작하는 경우, 브라우저는 TLS(전송 계층 보안) 또는 SSL(보안 소켓 계층)을 사용하여 데이터를 암호화한다.

이 과정에서 브라우저와 서버는 공개 키와 개인 키를 교환하여 암호화된 통신을 설정한다.


#### 브라우저가 웹서버에 HTTP 요청을 보낸다.

브라우저가 URL에 해당하는 웹 페이지를 요청하는 HTTP 요청을 보낸다.

이 요청에는 요청 메소드(GET, POST 등), URI, 쿠키, 요청 헤더(브라우저 정보, 언어 설정 등) 등의 정보가 포함된다.

- preflight: 단순한 get query 가 아니면 브라우저에서 options를 먼저 요청
- upgrade 헤더 고려(ws://)


#### 서버가 요청을 처리하고 응답을 보낸다.

서버는 브라우저로부터 요청을 수신하고, 브라우저에 HTTP 응답을 보낸다.

이 응답에는 HTTP 상태 코드(200, 404 등), 응답 헤더(컨텐츠 유형, 캐싱 정책 등)와 함께 요청된 데이터(HTML, CSS, JavaScript 등)가 포함된다.

이 때 웹 서버에서 모든 로직 처리 및 데이터 관리를 하게 되면 과부하가 일어날 가능성이 높다.

때문에 WAS에서 페이지의 로직이나 데이터베이스의 연동 작업 등을 처리하여 웹 서버로 전송한다.

웹 서버는 status code와 함께 서버 요청에 따른 결과 및 상태를 전달한다.

<img width="915" alt="image" src="https://user-images.githubusercontent.com/23524849/232106234-fc7eca1c-2f36-4eb2-99e6-423c868ff42d.png">


#### 브라우저가 HTML 컨텐츠를 보여준다.

- HTML 파일을 다운로드

- DOM 트리 빌드

통신을 통해 받아온 HTML 파일은 바이트 형태로 전달된다.

바이트 → 문자 → 토큰 → 노드 → 객체 모델로 파싱하여 DOM Tree를 최종 출력한다.

파싱 도중에 script 태그를 만나면 JS코드를 실행하기 위해 파싱을 중단한다.

중단시키지 않으려면 defer 속성을 추가해주면 된다.

     - defer: DOM 생성 직후 JS 파싱과 실행
     - async: HTML 파싱, JS 파일 로드를 동시에 진행

이론적으로 스타일 시트는 DOM Tree를 변경하지 않기 때문에 문서 파싱을 기다리거나 중단할 이유가 없다.

하지만 스크립트가 문서를 파싱하는 동안 스타일 정보를 요청한다면 → 스타일이 파싱되지 않은 상태라면 스크립트는 잘못된 결과를 내놓기 때문에 문제를 야기할 수 있다.

렌더링 엔진별 다른 정책으로 운영

     - 파이어폭스의 게코 엔진은 로드 중이거나 파싱 중인 스타일 시트가 있는 경우 모든 스크립트의 실행을 중단
     - 사파리/크롬의 웹킷 엔진은 로드되지 않은 스타일 시트 중 문제가 될만한 속성이 있을 경우 스크립트를 중단
     
- CSSOM 트리 구축

DOM Tree와 동일하게 CSS 파일을 CSSOM Tree로 최종 출력한다.

- DOM, CSSOM 트리로 렌더트리 구축

DOM 과 CSSOM 을 결합하여 Render Tree를 생성한다.

렌더러는 DOM 요소에 부합하지만 1:1로 대응하는 관계는 아니다.

예를 들면 <head>, display: ‘none’ 는 렌더 트리에 나타나지 않는다. (visibility: ‘hidden’ 은 나타난다)

필요한 노드만 선택하여 페이지를 렌더링하는 데 사용한다.

- 레이아웃 구축

Render Tree의 모든 노드들의 정확한 위치와 크기를 계산한다.

Layout에서 계산된 값들을 기반으로 요소들을 실제 픽셀로 변환하여 화면에 출력한다.

- 페인트

특정 액션과 이벤트에 따라 HTML 요소의 스타일이 변했을 때 크기나 위치를 변경해야 한다.

이때 CSSOM을 새로 만들어야 하기 때문에 Reflow & Repaint 현상이 발생한다.

     - Reflow: 주요 렌더링 경로의 모든 단계를 모두 재실행한다.
        - position, display, width, float, height, font-family, top, left, font-size, font-weight, line-height, min-height, margin, padding, border 등
     - Repaint: 레이아웃 단계는 건너뛰기 때문에 리플로우 작업보다는 빠른 편이다.
        - background, background-image, background-position, border-radius, border-style, box-shadow, color, ling-style, outline 등
     - Reflow와 Repaint 과정을 피하기 위해서는 GPU 가속 필요: Composition
        → **transform, opacity** 사용 → …3D… translate3D
  
- 합성


</details>
