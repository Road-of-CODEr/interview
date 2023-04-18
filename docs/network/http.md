# https와 http의 차이는 무엇일까?

<details>
<summary>정답 보기</summary>

## http

클라이언트와 서버간의 통신을 위한 프로토콜(Hypertext Transfer Protocol)

- 메서드? GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE, CONNECT
- REST API 메서드? GET, POST, PUT, DELETE

기본적으로 connection less 프로토콜이지만 'keep-alive', '파이프라이닝' 으로 성능 향상 도모 가능하다. RTT가 줄어들고, 3-way handshake 횟수가 즐어든다.

- 쿠키? 비연결형인 HTTP에서 상태 정보 유지를 위함이다. 클라이언트 로컬에 저장되는 '키: 값'의 쌍으로 만료일자가 존재한다.

하지만 이마자도 HOLB(Head Of Line Blocking) 문제가 있기는 하다.



## http2

한 connection에 여러 요청이 가능하다.

HTTPS 없이도 사용 가능하지만, 브라우저들이 HTTPS 기반하에 HTTP2가 작동하도록 막아두었다.

HTTP2에서 HTTPS를 사용하려면 보안 프로토콜 TLS 1.2 이상이 필요하다.


# https

HTTP 요청 내용을 안전하게 전송하기 위한 프로토콜(Hypertext Transfer Protocol Secure)

HTTP 프로토콜이 TCP 계층에서 동작한다고 하면, 여기에 SSL, TLS 라는 보안 계층을 올려 보안이 보장된 통신 프로토콜

신뢰할 수 있는 SSL 인증서 판매 기관에서 SSL 인증서를 구매해 적용한다.

개인정보 유출 위험을 완전히 막지는 못한다. 다만 암호화한 정보가 유출될 뿐...


