# Async, defer
 [[브라우저 렌더링 파이프라인#1. DOM 생성|브라우저는 html을 읽다가]] 스크립트 태그를 만나면 이를 먼저 실행하기에 렌더링이 차단된다. 외부 스크립트의 경우 다운받고 실행한 후에 남은 페이지 처리가 된다.

```html
<p>...스크립트 앞 콘텐츠...</p>

<script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>

<!-- 스크립트 다운로드 및 실행이 끝나기 전까지 아래 내용이 보이지 않는다. -->
<p>...스크립트 뒤 콘텐츠...</p>
```

그래서 바디태그 가장 바닥에 스크립트를 위치하면 콘텐츠 출력을 방해하지 않는다.

```html
<body>
  ... 스크립트 위 콘텐츠들 ...

  <script src="https://javascript.info/article/script-async-defer/long.js?speed=1"></script>
</body>
```

하지만 html이 아주 크다면 html 전체 다운로드 → 스크립트 다운은 역시 느리다. 이를 피하기 위해 사용할 수 있는 defer, async 속성 두가지가 있다.

### `async`
- 목적: 스크립트를 미리 다운로드하고, ==가능한 한 빨리 실행하는 것==
- **다운로드:** HTML 파싱과 **동시에** 백그라운드에서 다운로드
- **실행:** **다운로드가 끝나는 즉시** 실행 -> HTML 파싱이 멈춘다 -> 실행 후 HTML 파싱 재개
- DOMContentLoaded와 async 스크립트는 **별개로 실행**되며, 순서를 보장하지 않는다.
- usecase: 빠르게 실행해야 하는 독립적인 스크립트 (예: 광고, 분석 스크립트)



### `defer`
- 목적: 스크립트를 **미리 다운로드하고**, ==HTML 파싱이 끝난 후 실행==
- **다운로드:** HTML 파싱과 **동시에** 백그라운드에서 다운로드
- **실행:** **HTML 파싱이 모두 끝난 후** DOMContentLoaded 직전에 실행 -> → **HTML 파싱이 멈추지 않는다.**
- 스크립트 실행 -> `DOMContentLoaded` 이벤트 (순서보장)
- defer는 [^1]외부 스크립트에만 가능
- usecase: DOM이 준비된 후 실행이 필요한 스크립트 (예: 페이지 로직, 이벤트 핸들러)

---

- [^1]:
	- 외부 스크립트: `<script src="script.js" defer></script>` 와 같이 다운로드 받는 스크립트
	- 내부(inline) 스크립트: 
```html
<script defer>
	console.log('이 코드는 즉시 실행됩니다!');
</script>
```
