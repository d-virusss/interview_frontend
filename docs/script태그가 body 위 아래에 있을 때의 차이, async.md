# script태그가 body 위 / 아래에 있을 때의 차이, async load(async defer)

![defer 1](https://user-images.githubusercontent.com/55905801/164891768-e392638a-3ca6-44b7-ac42-a91d922e2a74.png)

HTML 태그를 파싱하는 중에 `<script>` 태그를 만나면 파싱을 중단하고 js 파일을 로드 후 js코드를 파싱한다. js 파싱 완료 후에 HTML 파싱을 이어서 한다.

### HTML 태그 중간에 script 태그를 두면

1. HTML 읽는 중에 중단 시점이 생겨 그만큼 display에 표시되는 것이 지연된다.
2. DOM 트리 생성 전에 script의 이벤트 등이 생성되지 않은 DOM의 조작을 시도할 수 있다.(지연시간동안?)

→ body 태그 최하단에 두는 게 가장 best practice라고 할 수 있다.

### script 로딩 순서 제어

1. async

```jsx
<script async src="script.js">
```

![defer 2](https://user-images.githubusercontent.com/55905801/164891769-261858e2-fb96-4447-ba67-681d0592d1ad.png)

async 속성을 추가하면 script태그를 만나도 HTML 파싱이 중단되지 않는다. script 로드가 병렬적으로 이루어지다 script 로드가 끝나면 HTML 파싱을 중단하고 script를 실행, script 실행이 끝나면 HTML 파싱을 재개한다.

1. defer

```jsx
<script defer src="script.js">
```

![defer 3](https://user-images.githubusercontent.com/55905801/164891770-b4e153c8-a9b1-4751-b235-2f886e9537f2.png)

defer 속성을 추가하면 script 로드는 병렬적으로 이루어지고, HTML 파싱을 중단하지 않으며 HTML 파싱이 끝난 후에 script 를 실행한다.

### script 태그 내부에서 로딩 순서 제어

1. DOMContentLoaded - DOM 생성이 끝난 후 실행
2. onload - 문서에 포함된 모든 콘텐츠(image, script, css, ...) 가 전부 로드된 후에 실행

```jsx
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>DOMContentLoaded</title>
</head>
<body>
    <script>
    	// window.onload가 가장 앞에 위치!
        window.onload = function(){
            console.log("afterwindowload");
            var target = document.querySelector("#test");
            console.log(target);
        }
		// DOMContentLoaded가 두번째에 위치!
        document.addEventListener("DOMContentLoaded", function() {
            console.log("afterdomload");
            var target = document.querySelector('#test');
            console.log(target);
        });
		// 일반 script 코드가 가장 끝에 위치
        console.log("바로시작")
        var target = document.querySelector('#test');
        console.log(target);
    </script>
    <div id="test">test</div>
</body>
</html>
```

![defer 4](https://user-images.githubusercontent.com/55905801/164891771-93425dc4-185f-4a71-8eb6-1bd6b834395b.png)
