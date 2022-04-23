# 호이스팅과 TDZ

ES6 이후부터의 JS 엔진 동작을 이해하기위해 생긴 개념.

 ‘JS 엔진은 식별자들을 최상단으로 끌어올린다'는 개념이다.

js의 변수 선언은 선언 → 초기화 단계

- **선언 단계**: 변수명을 등록하여 자바스크립트 엔진에 변수의 존재를 알린다.
- **초기화 단계**: 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다.

이후에 변수를 참조하여 할당이 가능하다

### Var 선언자의 호이스팅

```jsx
console.log(a); // undefined
var a = 10;
```

와 같은 코드는 호이스팅을 통해

```jsx
var a;
console.log(a); // undefined
a = 10;
```

과 같은 순서로 이해할 수 있다.

- 함수선언식으로 선언한 함수는 최상단으로 끌어올려진다.

```jsx
function f1(){ // 이게 함수선언식
	console.log('this is f1');
}
var a = 2;
```

에서

```jsx
var a;
function f1(){
	console.log('this is f1');
}
a = 2;
```

와 같이 수정된다.

- 함수표현식으로 선언한 함수는 할당한 변수가 var 일 경우에 변수명만 상단으로 끌여올려지고 함수선언은 끌어올려지지 않는다.

```jsx
var f2 = function f2() {
	console.log('this is f2');
}
```

에서

```jsx
var f2;
f2 = function f2(){
	console.log('this is f2');
}
```

로 변한다.

### let, const 의 호이스팅 / TDZ(Temporal Dead Zone)

let, const 로 선언한 변수는 var처럼 호이스팅은 일어나지만 초기화가 되기전까지 참조가 불가능하다. 이 상태를 표현하기 위해 let, const 로 선언한 변수는 TDZ에 있다고 한다. 선언 전에 변수를 사용하는 것을 허용하지 않는 **개념상의 공간**이다.

```jsx
typeof a; // 'undefined'
typeof variable; // ReferenceError : Cannot access 'a' before initialization
let variable;
```

아예 선언이 되지 않은 변수는 ‘undefined’ 이지만 TDZ에 있는 식별자에 접근하는 것은 에러를 발생시킨다.

`var`, `let`, `const` 키워드 모두 호이스팅이 일어난다

- var - 초기화 전에도 참조가능하지만 초기화이전이므로 `undefined` 이다.
- let, const - 호이스팅 후 초기화 전까지 TDZ 에 있으므로 ReferenceError가 발생한다.