# css-in-js

css 파일과 같은 스타일시트 묶음을 유지보수할 필요가 없다. css-in-js는 문서레벨이 아니라 컴포넌트 레벨로 CSS모델을 추상화하고, 중복 및 의존성을 줄여 유지보수를 용이하게 한다.

 JS를 CSS로 전환하는 고성능의 컴파일러이다.

### 장점

- gloabl namespace 신경쓸 필요 없음 → JSON으로 표현된 것을 CSS로 컴파일할 때, 기본적으로 고유이름 생성
- dependencies → CSS간의 의존관계 관리 문제 X
- minification : 클래스 이름의 최소화
- sharing Constnats : JS와 CSS 상태 공유
- CSS 로드 우선순위 이슈가 없다.

### 단점

- js 해석과정이 추가로 실행되기 때문에 페이지 전환등의 속도가 느려진다.