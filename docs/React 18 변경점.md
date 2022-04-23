# React 18 변경점

### React 의 SSR 단계

1. 서버에서 앱 전체의 data를 fetch 한다.
2. 앱 전체를 HTML로 서버의 응답으로서 render한다.
3. 클라이언트에서 앱 전체를 위한 JS를 로드한다.
4. 클라이언트에서 JS 로직을 서버에서 생성한 HTML에 연결한다 (hydration이라고 함)

→ 동기적으로 실행된다는 점이 가장 중요하다.

이제 React 18을 사용하면 Suspense를 사용하여 앱을 작은 독립 유닛으로 분할할 수 있다. 이 유닛들은 독립적으로 동작하며 다른 나머지 앱을 block하지 않는다. 그 결과 사용자는 컨텐츠를 더 빨리 볼 수 있게 되어 컨텐츠와의 상호작용을 빠르게 개시할수 있다. 병렬적으로 로드되기 때문에 앱의 가장 느릔 부분으로 인한 병목현상이 생기지 않는다.

복잡한 앱의 경우에 많은 애플리케이션 코드가 다운되기 때문에 SSR을 사용하지 않을 경우 JS를 로드하는 동안 빈 페이지만 표시된다. 

SSR 을 사용하면 서버의 React 컴포넌트를 HTML로서 사용자에게 보낼 수 있다. HTML은 interactive 하지 않지만 사용자로 하여금 JS가 로드되는동안 화면에 무언가를 보여줄 수 있다.

React는 어플리케이션 코드가 모두 로드되면 컴포넌트 트리를 메모리에 렌더링하고 DOM노드 생성이 아니라 모든 논리를 기존 HTML에 연결하여 HTML을 interactive하게 만든다.

SSR은 앱을 완전히 interactive 하게 만들지는 않지만 JS가 로드되는 동안 정적 콘텐츠를 보여줌으로써 앱의 비대화형 콘텐츠를 빨리 표시하고, 전체적인 퍼포먼스를 향상시킨다. 그리고 SEO에도 도움이 된다.

### 리액트의 문제점 2가지

### API fetching issue

서버사이드에서 HTML을 클라이언트로 보내주기 전에 먼저 렌더링에 필요한 데이터들을 api서버에 호출해야하는 상황이 있다. 이런 경우에 api 응답이 느리면 해당 api 를 사용하는 컴포넌트로 인해 사용자는 첫 페이지가 로드되기까지 오랜 시간을 기다려야한다.

### Hydration issue

브라우저는 리액트와 같은 SPA를 실행 시 JS를 다음과 같은 순서로 다룬다.

1. Fetch JS
2. Load JS
3. Hydrate

JS 번들 파일의 용량이 크다면 위의 과정이 오래걸린다.

결국 개발자는

1. 용량이 큰 복잡한 로직이 담긴 컴포넌트를 제외한다.
2. JS 코드들이 완전히 수화될때까지 유저를 기다리게 한다.

두 가지 선택지 중 골라야 되는 상황이 된다.

즉, 리액트를 사용하는 모던 웹에서

interaction blocking(표시는 되는데 이벤트핸들러가 달리지 않은 상황) vs show FTP(First Time to Paint) 두 가지의 UX 문제는 서로 trade-off 관계에 있을 수 밖에 없다.

React 17까지는 waterfall 방식의 rendering을 했지만

React 18부터는 빠르게 준비되는 부분부터 렌더링/hydration 시켜주며 이 기능을

- Streaming HTML
- Selective Hydration

이라고 한다.

### HTML Streaming

기존의 `renderToString` 을 통해 SSR 구현 시 브라우저에서는 서버에서 보내주는 HTML 페이지를 하나의 파일로 통째로 받았지만 새로운 버전에서는 `pipeToNodeWritable` 을 이용하여 HTML 코드를 작은 청크로 나누어 보내줄 수 있습니다.

Suspense로 렌더링이 오래걸리는 컴포넌트(Comment)를 감싼 후, fallback props로 Spinner등으로 표시해줄 수 있습니다.

Comment 컴포넌트가 렌더링할 준비가 모두 끝나면 리액트는 추가적인 HTML 코드를 스트리밍합니다.

```html
<div hidden id="comments">
  <!-- Comments -->
  <p>First comment</p>
  <p>Second comment</p>
</div>
<script>
  // This implementation is slightly simplified
  document.getElementById('sections-spinner').replaceChildren(
    document.getElementById('comments')
  );
</script>
```

이전 버전까지는 전체 페이지가 준비되기까지 사용자가 페이지의 다른 부분을 전혀 볼 수 없었습니다.

준비된 부분부터 보는 것이 가능하다는 것은

1. FTTB(First Time To Byte) 가 줄어든다는 뜻
2. 사용자 느낌을 떠나 정량적인 수치로도 렌더링 퍼포먼스의 향상이 있다

는 걸 의미합니다.

`<Suspense>` 컴포넌트를 사용하하여 전체 페이지를 각각의 작은 청크로 나누어 렌더링할 수 있게 돕습니다. 기존에는 `renderToString` 과 함께 사용할 수 없어 SSR에는 적용이 안되고 코드 스플리팅에만 사용이 됐지만 React18부터는 서드파티 라이브러리(lodable-component) 를 사용하지 않고도 SSR 환경에서 정상적으로 이용할 수 있게 됐습니다.

### Selective Hydrating

 `<Suspense>` 를 활용하여 FCP(First Content Paint) 에서 손해를 보는 상황에서는 벗어날 수 있게 됐지만, 너무 복잡한 컴포넌트가 있다고 가정해봅시다. 다른 엘리먼트들의 JS가 전부 로드되고 hydration할 준비가 되었더라도 복잡한 컴포넌트 하나 때문에 다른 준비된  엘리먼트들이 기다리고 있을 수 있습니다.

 이제 Selective Hydrating을 사용하면 렌더링하는데 비용이 큰 컴포넌트들을 `<Suspense>` 로 감싸, 폴백 엘리먼트를 내보내는 동안에도 다른 엘리먼트들은 hydrating을 시작할 수 있게 됐습니다.

 또한 어떤 컴포넌트를 먼저 hydartion 시킬지에 대한 우선순위 또한 정할 수 있게 되었습니다.

 `<Suspense>` 로 둘러싸인 컴포넌트들은 일반적으로 돔트리에 배치된 순서에 따라 순차적으로 진행이 됩니다.

```html
<Layout>
  <NavBar />
  <Suspense fallback={<Spinner />}>
    <Sidebar />
  </Suspense>
  <RightPane>
    <Post />
    <Suspense fallback={<Spinner />}>
      <Comments />
    </Suspense>
  </RightPane>
</Layout>
```

→ 기본적으로는 Sidebar가 먼저 hydration 진행됨

하지만 사용자가 `<Sidebar/>` 가 hydration 되기 전에 `<Comments/>` 에 대해 관심을 가지고 interaction을 발생시켰다면 리액트는 해당 클릭 이벤트를 기록하고 `<Comments/>` 에 대한 hydration의 우선순위를 높여서 진행합니다. `<Comments/>` 에 대한 hydration이 완료되면 앞서 기록했던 클릭 이벤트를 실행하고 남은 `<Sidebar/>` 의 처리도 마저 진행합니다.

→ `<Suspense>` 를 좀 더 세분화해서 여러 부모 자식 관계를 가진 컴포넌트를 대상으로 적용시킨다면 해당 기능의 장점이 좀 더 극명히 드러납니다.

### State Batch Update

batching은 업데이트 대상이 되는 상태값들을 하나의 그룹으로 묶어서 한번의 리렌더링에 업데이트가 모두 진행될 수 있게 해주는 것을 의미합니다.

→ 한 함수 안에서 setState를 아무리 많이 호출시키더라도 리렌더링은 단 한번만 발생합니다.

→ 이전버전에서도 batch update가 지원됐지만 브라우저 이벤트에서만 적용이 가능하고 api 호출의 콜백, timeouts 함수에서는 작동하지 않았습니다.

```jsx
function handleClick() {
    fetchSomething().then(() => {
      // React 17 and earlier does NOT batch these because
      // they run *after* the event in a callback, not *during* it
      setCount(c => c + 1); // Causes a re-render
      setFlag(f => !f); // Causes a re-render
    });
  }

setTimeout(() => {
  setCount(c => c + 1); // re-render occurs
  setFlag(f => !f); // re-render occurs again!
}, 1000);
```

### Automatic Batching

18버전부터는 `React.createRoot`
 를 이용해 브라우저 이벤트 뿐만 아니라 timeouts, promises를 비롯한 모든 이벤트에서 batching이 자동으로 적용되게 할 수 있습니다. 리엑트 팀은 여러 상황에서 발생할 수 있는 렌더링 횟수를 줄임으로 인해 퍼포먼스 개선을 기대할 수 있다고 하는데요, 이 기능을 `automatic batching`
이라고 합니다.

### Transition

상태 업데이트의 우선순위를 정하기 위해 상태 업데이트 대상을 2가지로 나눈다.

1. Urgent updates : 즉각적인 변화를 기대하는 상태 값들을 대상으로 함
2. Trasition updates : 사용자가 상태 값의 변화에 따른 모든 업데이트가 뷰에 즉각적으로 일어나는 것을 기대하지 않는 상태값들을 대상으로 함

페이지에서 사용자의 타이핑에 따라 화면이 달라지는 부분은 크게 `입력 폼` 과 `결과 창` 두가지 입니다.

**입력 창**

은 네이티브 이벤트를 발생하는 UI이므로 유저는 타이핑이 입력창에 즉각적으로 반영되기를 기대할 것입니다.

**결과 창**

은 직관적으로 어디에서 검색 결과를 가져오는 작업을 하는 공간으로 느껴지기에

**입력 창**

보다 UI 업데이트가 느린 것에 대해 자연스럽게 받아들여집니다.

입력 창과 결과 창에 사용하는 상태 값은 항상 동일한 시간에 업데이트되기 때문에 이로인해 결과 값에 따라 입력 창의 업데이트가 지연될 가능성이 발생합니다. 지금까지는 리엑트가 모든 상태 업데이트의 우선순위를 동일하게 처리하였기 때문에 사람이 기대하는 것에 맞춰서 뷰의 각 부분에 세밀하게 우선순위를 주어 렌더링하는 것이 매우 어려웠습니다.

리엑트 18에서 소개된 `startTransition`을 이용해 각 상태 업데이트에 대한 우선순위를 정해줄 수 있게 되었습니다.

```jsx
import { startTransition } from 'react';

// Urgent: Show what was typed
setInputValue(input);

// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```

**startTransition**으로 둘러싸인 부분은 클릭이나 키 입력에 의해 우선순위 높은 상태 업데이트가 발생하게 되면 렌더링 업데이트가 중단되고 키 입력이 다 끝난 이후의 업데이트만 발생하게 됩니다.

transition을 이용해 UI가 크게 달라지는 부분이 빈번하게 발생하더라도 사용자와 페이지간의 상호작용을 신속하고 원활하게 유지할 수 있습니다. 또한 더 이상 사용자에게 보여지는 부분과 관련이 없는 콘텐츠를 렌더링하는데 있어서 시간을 낭비하지 않아도 됩니다.

유저에게 transition 업데이트가 백그라운드에서 진행됨을 알려주고 싶을 수 있습니다. 이때는 `useTransition`
 훅을 이용해 `<Spinner/>`
와 같은 UI를 표시해줄 수 있습니다.

```jsx
import { useTransition } from 'react';

const [isPending, startTransition] = useTransition();
...
{isPending && <Spinner />}
...
```

보통 오토컴플리트 구현을 위해 debounce 기능을 사용합니다. debounce에는 setTimeout을 사용하는데요, startTransition은 setTimeout과는 다르게 함수 호출 스케쥴을 뒤로 미루는 것이 아닙니다. startTransition에 넘겨지는 콜백함수는 동기적으로 호출되며 콜백함수 안에서 일어나는 상태 업데이트는 `transitions`로 마킹되어 리엑트가 업데이트를 처리할때 어떤 우선순위로 처리해야 할지를 알려줍니다. 이 말은 즉 timeOut 함수와 같이 macroTask에 의해 둘러싸인 상태 업데이트보다 먼저 처리됨을 의미합니다. 고사양의 디바이스에서는 이러한 차이가 미세할 수 있지만, 최적화가 필수적인 디바이스 환경에서 이러한 차이는 큰 영향을 미칠 수 있습니다.

→ 비동기호출로 인해 macroTask에 등록되는 것이 아니라 콜백함수가 동기적으로 호출되며 다른 macroTask를 기다리지 않아도 된다!

`startTransition`은 크게 리액트가 UI 업데이트를 위해 크고 복잡한 일을 함으로 써 대기 시간이 발생하거나 느린 네트워크 환경에서 데이터를 받아오기 위해 기다리는 상황에서 사용한다고 합니다.

### 결론

- HTML 스트리밍을 사용하여 HTML을 원하는 타이밍에 내보낼 수 있다.
- Selected Hydration 을 통해 HTML과 JS가 완전히 로드되기 전에 가능한 빨리 hydration이 가능하다

위 기능을 통해 해결하는 3가지 문제

1. HTML 전송하기 전에 모든 데이터가 server에 load되기를 기다릴 필요가 없다. 앱의 shell 을 표시할 충분한 시간만 있다면 바로 HTML을 보내고 나머지 HTML을 준비되는 대로 스트리밍 한다.
2. Hydrating을 하기 위해 모든 JS가 로드되기를 기다릴 필요가 없다. 서버렌더링과 함께 코드스플리팅을 사용할 수 있다.
3. 페이지와의 상호작용을 시작하기 위해 모든 컴포넌트가 수화될 때까지 기다릴 필요가 없다. Selective Hydration을 사용하여 사용자가 상호작용하는 구성요소에 우선순위를 부여하고 조기에 interactive하게 만들 수 있다.

→ Suspense 컴포넌트는 이러한 모든 기능의 옵션으로서 기능한다. 개선은 React 내부에서 자동으로 이루어지며, isLoading에서 Suspense로의 변화는 작은 것 같지만 이러한 모든 개선을 가능하게 한다.