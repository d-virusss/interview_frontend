# React에서 SSR 구현

CSR + SSR 혼합 → Universal SSR rendering

서버와 클라이언트 둘 다 Node 사용

Node에서 ReactDOMServer를 사용하여 SSR 구현

→ React 컴포넌트를 파악하고 HTML 로 변환시켜주는 동작을 한다.

ReactDOM.hydrate를 통해 SSR에 의해 렌더링 된 HTML에 hydration을 진행한다.