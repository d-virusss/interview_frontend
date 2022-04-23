# position relative vs absolute 속도 비교

position relative 일 때는 position: relative인 HTML 객체가 부모, 자식의 다른 렌더트리에 영향을 끼친다. 따라서 Reflow가 일어날 때 더 많은 요소들의 변경이 필요하고 많은 연산이 필요하게 된다.

position absolute 일 때는 부모, 자식 렌더트리의 위치나 속성에 영향을 받지 않으므로 Reflow가 일어날 때 해당 요소를 변경하는데에 필요한 만큼의 연산만 필요하게 된다

→ position: absolute를 활용하여 렌더트리의 Reflow를 최적화할 수 있다.