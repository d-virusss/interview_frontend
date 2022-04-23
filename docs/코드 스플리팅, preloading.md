# 코드 스플리팅, preloading

### 코드 스플리팅

webpack에서 제공하는 기능이다. 코드를 다양한 번들로 분할하고, 요청에 따라 로드하거나 병렬로 로드할 수 있다. 일반적으로 세가지 방법이 있다.

1. Entry Points → entry 설정을 사용하여 코드를 수동분할

webpack.config.js 에서 설정할 수 있다.

1. Prevent Duplication → Entry dependencies / SplitChunksPlugin 을 사용하여 중복 청크를 제거하고 청크를 분할한다.

옵션 적용을 통해 중복 의존성을 제거할 수 있다.

1. Dynamic Imports → 모듈 내에서 인라인 함수 호출을 통해 코드를 분할한다.

ECMAScript 제안에 따른 import() 구문 사용

→ import 구문은 Promise를 반환

→ CommonJS 명세를 따르기 때문에 import 하는 파일의 export default 를 {default: ... } 로 받아야 한다.

→ 비동기적으로 chunk를 로드할 수 있다.

→ 컴포넌트를 로드할 경우에는 컴포넌트 자체를 state에 넣는 방식으로 구현해야 한다.

**React.lazy, Suspense 사용하는 방식 (React 16.6 버전 이후 도입)**

1. React.lazy

컴포넌트를 렌더링하는 시점에 비동기적으로 로드하도록 하는 유틸함수

```jsx
const SplitMe = React.lazy(() => import('./SplitMe'));
```

1. Suspense

```jsx
import React, {Suspense} from 'react';
 
(...)

<Suspense fallback={<div>loading...</div>}
   <SplitMe />
</Suspense>
```

fallback props 를 통해 로딩 중 보여줄 JSX를 지정할 수 있다.

1. loadable 라이브러리 사용

React 공식문서에서 SSR 시 사용을 권고하는 라이브러리이다.

```jsx
**const SplitMe = loadable(() => import('./SplitMe'), {
   fallback: <div>loading...</div>
}**
```

SSR을 지원한다.

1. preload

 loadable 을 사용해 preload 메소드를 사용할 수 있다.

```jsx
const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>
});
 
function App() {
  const [visible, setVisible] = useState(false)
  const onClick = () => {
    setVisible(true)
  };
  const onMouseOver = () => {
    SplitMe.preload();
  };
  return (
    <div className='App'>
      <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p onClick={onClick} onMouseOver={onMouseOver}> 
            Hello React!
          </p>
            {visible && <SplitMe />}
        </header>
    </div>
  );
}
 
export default App;
```

mouseOver 시 preload로 js 파일 로드가 시작된다.

### Preloading

렌더링이 되지 않아도 특정 함수를 호출하여 js를 미리 불러오는 기능이다.

### 사용법

```jsx
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./notify').then(({ default: notify }) => {
      notify();
    });
  };
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Click Me</button>
      </div>
    );
  }
}

export default App;
```

import 를 함수로 사용하면 webpack에서 코드를 분리하고, import 가 호출될 때 chunk.js 로 분리하여 로드를 하게 된다.