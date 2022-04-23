# 브라우저의 렌더링

### Real DOM(Document Object Model)

브라우저는 문자열 형식의 HTML을 곧바로 이해할 수 없기 때문에, 브라우저가 이해할 수 있도록 변환이 필요하다. DOM은 브라우저 렌덜이 엔진의 HTML parser에 의해 생성된 **‘트리 구조의 Node 객체 모델’** 이다.

정의에 Model 이 들어가는 것처럼, DOM은 HTML, XML 문서의 프로그래밍 interface이다. DOM은 프로그래밍 언어가 DOM구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용등을 변경할 수 있게 돕는다.

**DOM의 목적**

→ JS를 사용하여 문서에 대한 추가, 삭제, 이벤트 처리를 처리하는 인터페이스를 제공하는 것

DOM 또한 객체이기 때문에 상속관계로 이루어져있다.

예를 들어, img 엘리먼트에 disable 속성이 안들어가는 이유는 HTMLInputElement에만 disabled property가 있기 때문이다.

![make DOM tree](https://user-images.githubusercontent.com/55905801/164891080-a63a6f24-9d7f-41f8-a4ae-41f5482f5bab.png)

### 렌더링 과정

1. DOM 트리 생성 (HTML parser에 의한 DOM Tree)
2. Style Rules 생성 (CSS parser에 의한 Rules 생성) (1 과 병렬적으로 작동)
3. DOM트리 + Style Rules 접합 및 Render Tree 생성

   → display: none; 과 같은 속성은 화면에서 공간을 차지하지 않기 때문에 렌더 트리에서 제외됨

4. Layout (Reflow)

   → 뷰포트 내의 각 노드들의 정확한 위치와 크기 계산

   → %, vh, vw와 같이 상대적인 위치, 크기를 실제 화면에 그려지는 px 단위로 변환하는 과정

5. Paint

   → Layout 과정이 완료되면 요소들의 위치, 크기, 스타일 계산이 완료된 렌더트리 존재

   → 렌더트리 이용하여 브라우저는 요소들을 실제 화면에 그린다.

   → backgroud-color, color 등의 경우에는 빠르게 painting 되지만, 그라데이션, 그림자 효과 등 복잡한 스타일은 Painting 소요시간이 비교적 오래걸린다.

6. 렌더트리의 변화

   → 특정 이벤트에 따라 HTML 요소의 크기, 위치 등이 변경되면 영향받는 자식 노드, 부모 노드 들을 포함하여 Layout을 다시 수행한다.

   → 다시 Layout (계산하는 과정)을 수행하는 걸 Reflow 라고 하고

   → 다시 화면에 그려주는 과정을 Repaint라고 한다.

### Reflow가 일어나는 경우

- 페이지 초기 렌더링 (최초 Layout)
- 브라우저 리사이징 (Viewport 크기 변경)
- 노드 추가 또는 제거
- 요소의 위치, 크기 변경
- 폰트 변경, 이미지 크기 변경

### Repaint가 일어나는 경우

- Reflow만 수행됐을 때는 실제 화면에 반영되지 않는다.
- 그려주는 과정이 Repaint이다.
- 반드시 Reflow가 진행되어야만 Repaint가 진행되는 것은 아니다. background-color, opacity 등 레이아웃에는 영향을 주지 않는 스타일 속성이 변경되었을 때는 Repaint만 수행된다.

### 브라우저 성능저하의 원인

1. 잦은 Reflow와 Repaint

### 렌더링 최적화

1. Reflow 최소화

   → Reflow 발생하는 속성보다, Repaint만 발생하는 속성 사용

   → border, margin 등 위치가 조금이라도 변경되는 건 Reflow가 발생한다

2. 영향을 주는 노드 최소화하기

   → 레이아웃 변화가 많은 요소의 경우(변화가 찾은 컴포넌트의 경우), position을 absolute / fixed 로 사용하면 좋다.

   → fixed 의 경우 layout이 수정되지 않기 때문에 reflow과정이 필요없어져 repaint 연산비용만 들게 되어 효율적이다.

3. 프레임 줄이기

   → 덜 부드럽게 표현하게 되면 성능개선 (사용자 경험은 나빠지겠지만..)

4. Reflow / Repaint 가 모두 발생하지 않는 속성도 있다.

   → transform, opacity, cursor, orphans, perspective
