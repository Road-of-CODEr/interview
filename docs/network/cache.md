# Cache

## 캐싱의 정의는 무엇인가

<details>
<summary>정답 보기</summary>

자주 사용하는 데이터나 값을 미리 복사해서 저장해두는 임시 장소.
네트워크를 통해 리소스를 요청하고 응답받는 것은 비용이 많이 든다. 특히 HTTP1 의 connection less 방식에서는 더더욱...
불필요한 요청을 피하기 위해 브라우저의 HTTP 캐시를 고려해 볼 수 있다.

1. HTTP 요청은 브라우저 캐시로 라우팅되어 유효한 캐시가 있는지 확인
2. 유효한 캐시가 있다면, 캐시에서 응답을 읽는다.

</details>

## 캐싱의 장점은 무엇인가

<details>
<summary>정답 보기</summary>

- 불필요한 네트워크 요청을 줄임으로서 로드 성능 향상이 가능하다.
- 서버 부하 완화.

</details>

## 캐싱을 사용하기 위해 사용되는 header 값은 어떤 것이 있는가

<details>
<summary>정답 보기</summary>

HTTP 캐시의 동작은 요청 헤더와 응답 헤더에 의해 제어된다.

- 요청 헤더
  - 기본적으로 브라우저가 사용자 대신 캐싱을 위한 기본 값을 설정한다.
- 응답헤더
  - 웹 서버가 추가하는 헤더

</details>

## 캐시 헤더를 나열해 보라

<details>
<summary>정답 보기</summary>

- Cache-control: 브라우저가 응답을 캐시하는 방법과 기간 지정. ETag, Last-Modified와 함께 사용 가능.
- ETag: 브라우저가 만료된 캐시 응답을 찾으면 서버에서 해당 토큰(파일 내용 해시)를 보내 파일이 변경되었는지 확인 가능. 서버 응답이 동일 토큰이라면 동일한 파일이므로 다시 다운로드할 필요 없음.
- Last-Modified: Etag와 동일 목적. 컨텐츠 기반이 아닌, 시간 기반으로 파일이 변경되었는지 확인(ETag보다 부정확)
- Expires: 리소스가 만료되는 날짜와 시간을 지정한다. 만료 시간이 지난 경우, 캐시는 해당 리소스를 새로 가져와야 한다.

</details>

## Cache-Control에 사용되는 값들은 어떤 것들이 있는가?

<details>
<summary>정답 보기</summary>

- public: 응답(리소스)이 공개 캐시에 저장될 수 있다.
- private: 응답이 사용자별로 캐시되어 공유되지 않아야 한다.
- no-cache: 캐시된 응답을 사용하기 전에 원본 서버의 검증이 필요하다. 매번 서버에 유효성 확인.
- no-store: 응답을 캐시에 저장해서는 안 됨을 나타낸다.
- max-age: 리소스가 캐시에서 유효한 시간(초 단위)을 의미한다. 변경되지 않을 파일에 대해 긴 시간 캐싱.
- s-max-age: 공유 캐시에서 리소스가 유효한 시간(초 단위)을 의미한다.
- stale-while-revalidate: 캐시가 만료된 후, 새로운 리소스를 가져오기 전까지 캐시된 리소스를 사용할 수 있는 최대 시간(초 단위)을 의미한다.

</details>

## 재검증 과정 설명하라

<details>
<summary>정답 보기</summary>

캐시의 유효 시간이 지난 경우 재검증 처리를 한다. [참고](https://developer.mozilla.org/en-US/docs/Web/HTTP/Conditional_requests)

- 브라우저가 가진 캐시가 유효한 경우 304 응답을 내려준다.
- 기존에 받았던 리소스의 응답 헤더의 `ETag`, `Last-Modified`값을 사용한다.

1. **If-None-Match**: 캐시된 리소스의 `ETag` 값과 현재 서버 리소스의 `ETag` 값이 같은지 확인한다.
2. **If-Modified-Since**: 캐시된 리소스의 `Last-Modified` 값 이후에 서버 리소스가 수정되었는지 확인한다.

</details>
