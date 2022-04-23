# Redux 와 Flux 패턴

### Flux 패턴

Flux는 MVC와 다르게 단방향으로 데이터가 흐른다.

Action → Dispatcher (콜백으로 함수전달) → Store → View

### Action

Dispatcher에서 콜백 함수가 실행 되면 Store가 업데이트 되게 되는데, 이 콜백 함수를 실행할 때 데이터가 담겨 있는 객체가 인수로 전달되어야 한다. 이 객체를 Action이라고 하는데, 대체로 Action 생성자에서 만들어진다.

### Dispatcher

Dispatcher는 Flux의 모든 데이터 흐름을 관리하는 허브 역할을 한다. Action이 발생되면 Dispatcher로 전달되는데, Dispatcher는 전달된 Action을 보고 등록된 콜백 함수를 실행하여 Store에 데이터를 전달한다. Dispatcher는 전체 어플리케이션에서 한 개의 인스턴스만 사용된다.

### Store

어플리케이션의 모든 상태 변경은 Store에 의해 결정이 된다. Store가 변경되면 View에 변경되었다는 사실을 알려준다. Store 또한 Dispatcher 처럼 싱글톤이다.

### View

Flux의 View는 화면에 나타내는 것 뿐만 아니라, 자식 View로 데이터를 흘려 보내는 뷰 컨트롤러의 역할도 한다.