# JS layout, paint 과정 없앨 수 있는 기법들

1. documentFragment

  DOM의 단편적인 부분을 정의할 수 있는 노드이다. 부모가 없는 최소화된 경량화 문서객체이다. 기본적으로 DOM과 동일하게 동작하지만, HTML과 DOM트리에는 영향을 주지 않으며, 메모리에서만 정의된다.

 body 태그에 appendChild 메소드로 추가된 이후에 DocumentFragment에는 남아있는 textContent가 없다.

 DocumentFragment는 활성화된 DOM의 일부가 아니라 메모리상에서만 존재한다. 따라서 DocumentFragment에 변경이 일어나도 DOM구조에는 변경이 없기 때문에 브라우저가 화면을 다시 렌더링 하지 않는다

**→ Reflow, Repaint가 일어나지 않는다.**

2. position absoluted position fixed

3. RAF 사용하여 렌더링 파이프라인 탈 때 스타일관련된 것들 실행되도록