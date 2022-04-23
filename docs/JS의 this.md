# JS의 this

JS의 this를 활용하면 함수의 매개변수로 따로 context를 받지 않아도 context에 접근할 수 있다.

** **인사이드 자바스크립 this 부분 참조할 것**

JS의 this는 3자지 방식으로 객체를 참조한다.

1. 객체의 메소드를 호출한 경우 → 객체에 binding
2. 함수 내부에서 실행되는 경우 → window 혹은 global 객체에 binding
3. 생성자 함수를 통해 객체를 생성하는 경우 → 새로 생성될 객체에 binding

### Call, Apply 함수를 활용한 명시적 this binding

call 과 apply 함수는 Function.prototype의 메소드이다.

사용법은 다음과 같다.

Array.prototype.slice.apply( this에 binding될 객체, args )

→ Array.prototype.slice() 메소드를 호출하고, 이때 this는 arguments 객체로 바인딩해라.

apply는 두번째 매개변수로 apply를 적용하는 함수로 넘길 매개변수들을 배열로 넘기고 call은 단순히 여러개의 매개변수를 넘긴다.