


CSS는 렌더링 차단 리소스이다. CSS 파일이 다운로드 되지 않으면 CSSOM이 만들어지지 않고, CSSOM이 없으면 렌더트리도 생성되지 않아서 화면이 그려지지 않는다. 

(참고. [[브라우저 렌더링 파이프라인]])

자바스크립트도 기본적으로 **렌더링을 막는 리소스**다. 또한, **DOM 파싱**(HTML 해석)도 중단시킨다. 하지만 `defer`와 `async` 속성을 사용하면, 이 문제를 해결할 수 있다.

- **defer**: DOM이 모두 해석된 후 자바스크립트를 실행한다.
- **async**: 자바스크립트를 비동기적으로 다운로드하고, 완료되면 바로 실행한다.

# **CSS가 브라우저에 적용되는 3가지 방법**

- Inline(인라인) CSS
    
    ```html
    <p style="color: blue; font-size: 18px;">
      content here
    </p>
    ```
    
- Internal(내부) CSS
    
```html
<head>
	<style>
		body {
		  color: blue;
		  font-size: 18px;
		}
	</style>
</head>
```
    
- External(외부) CSS
    
    ```html
    <head><link rel="stylesheet" type="text/css" href="./main.css"><link rel="stylesheet" type="text/css" href="../side/side.css"></head>
    ```
    

따라서, 어떤 방법을 사용하든 CSS가 모두 파싱 된 후, **CSSOM이 만들어져야 브라우저가 화면을 그릴 수 있다**.

# **Critical CSS**

CSS 파일이 커지면 다운로드 시간이 늘어나고, **CSSOM**을 생성하는 데도 시간이 오래 걸린다. **Critical CSS**는 화면에 먼저 보여야 하는 부분의 CSS만 미리 추출해서 빠르게 렌더링하는 기술이다.

- **External CSS**: 아무 속성 없이 외부 CSS 링크를 사용하면, HTML을 다운로드한 후 CSS 파일을 다운로드해야 하므로 FCP(First Contentful Paint)가 늦어진다. 이 때문에 External CSS는 Critical CSS에 적합하지 않다.
- **Internal CSS**: HTML의 `<style>` 태그 안에 CSS가 포함되어 있기 때문에 HTML이 로드되면 바로 사용할 수 있다. 중요한 건, 화면에 보이는 부분만 Internal CSS로 작성해야 하며, 크기는 작아야 한다. 권장 크기는 14kB 미만이다.
- **최적화 방법**: Critical CSS를 Internal CSS로 먼저 넣고, 이후에 External CSS를 비동기적으로 로드하면, FCP를 빠르게 하면서도 나머지 스타일을 불러올 수 있다.
- **CSS-in-JS** 라이브러리의 Critical CSS: 스크롤 영역과 상관없이 현재 페이지에서 필요한 모든 스타일을 포함하는 방식이다.

```html
<!-- 1. Internal CSS (Critical CSS) -->
<style type="text/css"> 
.accordion-btn {background-color: #ADD8E6;color: #444;cursor: pointer;padding: 18px;width: 100%;border: none;text-align: left;outline: none;font-size: 15px;transition: 0.4s;}.container {padding: 0 18px;display: none;background-color: white;overflow: hidden;}h1 {word-spacing: 5px;color: blue;font-weight: bold;text-align: center;}
</style>

<!-- 아래는 크리티컬 css를 제외한 영역 -->
<!-- 2. 비동기적으로 미리로드, 로드가 완료되면 rel을 다시 스타일시트로 바꾼다 (as는 우선순위표현) -->
<link rel="preload" as="style" href="styles.css" onload="this.onload=null;this.rel='stylesheet'">

<!--3. JS실행 하지 않는 브라우저에서 대체수단 제공 -->
<noscript><link rel="stylesheet" href="styles.css"></noscript>
```


# CSS 관련 도구

### CSS Preprocessor

sass, less, stylus와 같은 전처리기는 기존의 css를 확장하는 역할이다. 변수 선언, 중첩, 조건문 및 반복문을 통해 스타일을 생성할 수 있으며, 빌드시점에 css 파일로 컴파일 된다.

- 미리 컴파일된 css: 빌드 도구를 사용해 스타일을 묶어 하나의 css파일로 만들어 html에 포함한다. 런타임 오버헤드가 없음
- SASS: 모듈화, 중첩 구문 가능, 전처리기를 통해 css로 변환 (`.scss` 및 `.less` 파일을 빌드 타임에 `.css` 파일)
- postCSS: 오토프리픽스(크로스브라우징 지원), 폴리필 등을 적용하는 후처리기로, 변환된 CSS 파일에 추가적인 처리를 해 최종적으로 브라우저에 제공한다.

### CSS-in-JS

CSS-in-JS 방식은 React 같은 컴포넌트 기반 프레임워크와 잘 맞는다. 스타일을 JS 변수나 함수로 정의하고, 동적으로 CSS를 생성하여 적용할 수 있다.

- 작동 순서
    1. JavaScript 템플릿 리터럴을 통해 CSS 코드 작성
    2. 번들링된 소스코드에 포함하여 브라우저로 전달
    3. 런타임에 해석되며, SSR 환경에서는 미리 CSS를 추출하는 방식도 제공된다.
- 개발모드: 스타일 태그를 통해 주입한다. (HMR 이점)
- 프로덕션 모드: 라이브러리마다 다르지만, `<style>` 태그를 쓰거나 외부 CSS 파일을 요청하는 방식이 사용된다.

##### CSS-in-JS 스타일 적용 방식

###### 런타임 스타일시트(Runtime stylesheets)
- js가 실행된 후 스타일이 적용된다. js가 브라우저에서 로드될 때, 스타일을 생성
    1. HTML 파일이 브라우저에 로드되었을 때는 `<style>` 태그 및 외부 CSS 링크가 없는 상태로 로드
    2. JavaScript 파일안에 스타일 관련 소스파일이 있고, 일련의 과정을 거쳐 스타일을 입히게 된다.
        1. 스타일 태그를 추가하여 요소에 스타일을 집어넣는 방식
        2. cssom에 직접 스타일 rule을 추가하는 방식 (insertRule)
            - **stitches.js** 와 같은 제로런타임 라이브러리에서 동적으로 스타일 규칙 삽입할때 이방법을 사용한다.
- **단점**:
    - 스타일 생성이 JavaScript 실행 후에 이루어지므로 초기 렌더링이 지연될 수 있다.
    - 페이지 로드 시 Critical CSS가 빠르게 적용된 후, JavaScript가 실행되면서 런타임 스타일이 덧입혀진다. 이로 인해 스타일이 두 번 입혀지는 과정이 발생할 수 있다.

###### 정적 CSS 추출(Static CSS extraction 또는 zero-runtime)

- 빌드 시점에 미리 css파일을 생성하고 링크 태그를 통해 정적 css파일을 로드한다. 런타임 비용이 없다.
- 정적 CSS 추출은, Critical과 같은 라이브러리를 통해 above-the-fold 영역의 Critical CSS를 따로 추출하여 사용할 수 있다
- **동적 변수 처리**: 빌드 시 결정되지 않은 값들은 CSS 변수를 사용하거나, `@vanilla-extract/dynamic` 패키지 등을 통해 런타임에서 계산된 값을 스타일에 반영할 수 있다.

###### 속도비교

- **FCP(First Contentful Paint)**: 런타임 스타일 시트가 더 빠르다. 제로런타임 방식이 외부 css 파일을 적용하는 구조이기 때문이다. 하지만 크리티컬css를 적용하면 제로런타임에서 fcp 개선 가능
- **TTI(Time to Interactive)**: 런타임 스타일시트는 js가 모두 로드되고 실행되어야 스타일이 적용되기 때문에 js크기가 클 경우 tti가 지연된다.
- **최선의 선택**: SSR 환경에서는 **Critical CSS를 적용한 정적 CSS 추출 방식**이 가장 효율적이다. 이 방식은 파일 크기를 최소화하고, FCP와 TTI 모두 빠르게 최적화할 수 있는 방법으로 결론지을 수 있다.

###### 번외 **Atomic CSS**

- 웹사이트의 시각적 요소를 작은 단위로 나누고, 단위별로 이름을 지정해 CSS를 작성하는 방법 (태일윈드)
- 미리 정의된 css파일을 통해 클래스네임만 적용되기에 css 코드양이 적다 (재사용성이 좋음). 이러한 방식은 의미보다는 시각적 기능에 중점을 둔 클래스 네이밍을 사용한다.
- 코드 중복을 줄이고, CSS 파일 크기를 최소화할 수 있어 성능이 향상된다. 필요한 클래스만 적용되기 때문에 전체적인 CSS 양이 줄어든다.

> Emotion의 메인테이너는 런타임 스타일시트 방식의 런타임 환경에서의 오버헤드와 번들 크기의 증가 등의 성능 문제로 Sass 또는 tailwindCSS(utility-first CSS)로 회귀한다는 글이다. 특히 런타임 성능 비용이 너무 높다는 말을 강조한다. CSS-in-JS의 DX는 좋지만 말이다.