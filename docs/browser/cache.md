# HTTP 통신에서 캐싱을 활용하기 위해 어떤 값들을 설정할 수 있을까요?


## 캐싱의 정의는 무엇일까요?
자주 사용하는 데이터나 값을 미리 복사해서 저장해두는 임시 장소.
네트워크를 통해 리소스를 요청하고 응답받는 것은 비용이 많이 든다. 특히 HTTP1 의 connection less 방식에서는 더더욱...
불필요한 요청을 피하기 위해 브라우저의 HTTP 캐시를 고려해 볼 수 있다.

1. HTTP 요청은 브라우저 캐시로 라우팅되어 유효한 캐시가 있는지 확인
2. 유효한 캐시가 있다면, 캐시에서 응답을 읽는다.


## 캐싱을 활용하므로써 어떤 점을 더 좋아지게 만들 수 있을까요?
- 불필요한 네트워크 요청을 줄임으로서 로드 성능 향상이 가능하다.
- 서버 부하 완화.


## 캐싱을 사용하기 위해 사용되는 header 값은 어떤 게 있을까요?
HTTP 캐시의 동작은 요청 헤더와 응답 헤더에 의해 제어된다.

- 요청 헤더
: 기본적으로 브라우저가 사용자 대신 캐싱을 위한 기본 값을 설정한다.

- 응답헤더
: 웹 서버가 추가하는 헤더


### Etag 란 무엇일까요?
캐시의 방식중 하나

- Cache-control: 브라우저가 응답을 캐시하는 방법과 기간 지정. ETag, Last-Modified와 함께 사용 가능.
- ETag: 브라우저가 만료된 캐시 응답을 찾으면 서버에서 해당 토큰(파일 내용 해시)를 보내 파일이 변경되었는지 확인 가능. 서버 응답이 동일 토큰이라면 동일한 파일이므로 다시 다운로드할 필요 없음.
- Last-Modified: Etag와 동일 목적. 컨텐츠 기반이 아닌, 시간 기반으로 파일이 변경되었는지 확인(ETag보다 부정확)


### Cache-Control에 사용되는 값들은 어떤 것들이 있을까요?
- No-Cache: 매번 서버에 유효성 확인
- no-store: 캐싱되어야 하지 않는 리소스
- max-age=<seconds>: 리소스가 유효하다고 판단되는 최대 시간. 변경되지 않을 파일에 대해 긴 시간 캐싱. ex) 이미지


### header 란 무엇일까요?
HTTP 메세지는 시작줄, 헤더, 본문으로 구성된다.
요청을 주고 받을때의 부가적인 정보가 포함된다.


#### header의 구조는 어떻게 될까요?
- general header(공통 헤더)
: 요청, 응답 모두 적용되지만, 최종적으로 전송되는 데이터와는 관련이 없는 헤더
     - date: 현재 시간
     - pragma, cache-control: 캐시 제어
     - upgrade: 프로토콜 변경 시 사용
     - via: 중계 서버 이름, 버전, 호스트 명
     - connection: 네트워크 접속을 유지할지 말지 제어. HTTP1.1 은 keep-alive로 연결유지가 default
     - transfer-encoding: 사용자에게 entity를 안전하게 전송하기 위해 사용하는 인코딩 형식을 지정
     - ...

- request header(요청 헤더)
: HTTP 요청에 사용. 컨텐츠와 상관없는 패치될 리소스나 클라이언트 자체에 대한 정보
     - host: 요청하려는 서버 호스트 이름과 포트 번호
     - user-agent: 클라언트 프로그램 정보
     - accept: 클라이언트가 처리 가능한 MIME Type 종류
     - accept-encoding: 클라이언트가 해석 가능한 압축 방식
     - cookie: 쿠키 값. key: value.
     - ...

- response header(응답 헤더)
: 위치 또는 서버 자체에 대한 정보(이름, 버전) 같이 응답에 대한 부가적인 정보
     - server: 웹 서버의 종류
     - age: max-age 시간 내에서 얼마나 흘렀는지
     - referrer-policy: 서버 referrer 정책을 알려주는 값
     - set-cookie: 서버측에서 클라이언트에게 세션 쿠키 정보 설정

- entity header(엔티티 헤더)
: 컨텐츠 길이나 MIME 타입과 같이 엔티티 바디에 대한 자세한 정보를 포함하는 헤더
     - content-type: 리소스의 media type 명시
     - content-length: 바이트 단위를 가지는 개체 본문의 크기
     - content-language: 본문을 이해하는데 적절한 언어
     - allow: 지원되는 HTTP 요청 메서드
     - ETag: 리소스의 버전을 식별하는 고유한 문자열 검사기