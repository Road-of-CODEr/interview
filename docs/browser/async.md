# 브라우저의 비동기 작업

## 브라우저에서 비동기로 처리하는 것들은 어떤 것이 있는가?

<details>
<summary>정답 보기</summary>

### Web APIs

- 타이머
  - setTimeout, setInterval, setImmediate
- HTTP 요청(XMLHttpRequest)
  - async 속성이 걸려있는 스크립트는 파일의 다운로드가 비동기로 된뒤 즉시 실행
  - defer 속성은 async와 같으나 파일 실행이 HTML 파싱이 완료된 다음에 실행
- DOM
  - 이벤트 핸들러

</details>
