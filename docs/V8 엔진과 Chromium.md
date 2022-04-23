# V8 엔진과 Chromium

### 브라우저의 구조

![engine](https://user-images.githubusercontent.com/55905801/164891736-c0d3a137-f095-4565-9074-0a6dc4ab0274.png)

- User Interface: 주소 표시줄, 이전/다음 버튼, 북마크 메뉴 등. 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분
- Browser Engine: User Interface와 Rendering Engine 사이의 동작을 제어
- Rendering Engine: 요청한 콘텐츠를 표시, HTML을 요청하면 HTML과 CSS를 파싱 하여 화면에 표시함
- Networking: HTTP 요청과 같은 네트워크 호출에 사용됨
- Javascript Interpreter(또는 Engine): 자바스크립트 코드를 해석하고 실행함. 크롬에서는 [V8 엔진](https://beomy.github.io/tech/javascript/javascript-runtime/#%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%97%94%EC%A7%84-v8)을 사용함
- Display Backend: 기본적인 위젯(콤보 박스 등..)을 그림
- Data Persistence: Local Storage, 쿠키 등 클라이언트 사이드에서 데이터를 저장하는 영역

![rendering engine](https://user-images.githubusercontent.com/55905801/164891739-ec9ebdd6-7015-4b29-a684-f3e25a36be5f.png)

크롬은 Js를 실행하는 엔진으로서 V8엔진을 사용하고, 렌더링 엔진으로서는 Blink를 사용한다.
