# Javascript 싱글 쓰레드

자바스크립트의 Main Thread인 이벤트 루프는 싱글 스레드이다. 하지만 이벤트 루프만 독립적으로 실행되지 않고 웹 브라우저나 Node JS 같은 멀티 쓰레드 환경에서 실행된다. 즉 JS 자체는 싱글스레드이지만, 런타임은 멀티스레드 환경일 수 있는 것!

### 싱글 스레드로 여러 요청을 처리하는 방식

→ 비동기 작업을 통해 처리한다

JS의 비동기 런타임 과정

![Untitled](Javascript%20%E1%84%89%E1%85%B5%E1%86%BC%E1%84%80%E1%85%B3%E1%86%AF%20%E1%84%8A%E1%85%B3%E1%84%85%E1%85%A6%E1%84%83%E1%85%B3%2088ceeb5e2f24453fbced3b288470146e/Untitled.png)

- **Call Stack**: 자바스크립트에서 수행해야 할 함수들을 순차적으로 스택에 담아 처리
- **Web API**: 웹 브라우저에서 제공하는 API로 AJAX나 Timeout등의 비동기 작업을 실행
- **Task Queue**: Callback Queue라고도 하며 Web API에서 넘겨받은 Callback함수를 저장
- **Event Loop**: Call Stack이 비어있다면 Task Queue의 작업을 Call Stack으로 옮김

CallStack은 일하는곳, Web API는 실행할 수 있는 작업 목록, Task Queue는 작업순서 리스트, Event Loop 는 작업 관리자 느낌

**스레드 정리**

- Call Stack - JS의 메인 스레드에서 제공
- Web API - 브라우저에서 제공
- Task Queue -
- Event Loop - 브라우저 / node 런타임(libuv 라이브러리 사용)에서 제공됨

### JS가 싱글 쓰레드를 사용하는 이유

싱글 쓰레드의 경우에는 동기화(동시성 문제)를 신경쓰지 않아도 되기 때문에 훨씬 간편하다.

### 태스크 Queue vs MicroTask Queue

이벤트 루프가 태스크를 핸들링할 때 함수의 **종류**에 따라 다른 큐에 태스크를 넣는다.

- 태스크 큐에 들어가는 함수들

setTimeout, setInterval, setImmediate, requestAnimationFrame, I/O, UI 렌더링

- 마이크로태스크 큐에 들어가는 함수들

Promise, MutationObserver, process.nextTick

→ 태스크 큐와 마이크로태스크 큐중 누가 먼저일까?

→ 마이크로태스크가 먼저이다.

→ 마이크로태스크 큐의 모든 태스크들을 처리한 후에 태스크 큐의 태스크들을 처리한다.