# Virtual DOM
DOM은 웹페이지에 대한 인터페이스로 브라우저가 웹페이지의 콘텐츠와 구조를 어떻게 보여줄 지에 대한 정보를 담고있다. DOM 업데이트를 효율적으로 처리하기 위해 대표적으로 리액트의 [[Virtual DOM]]과 앵귤러의 [[Incremental DOM]] 두가지가 접근 방식이 있다.
![[Pasted image 20250131205029.png]]가상돔은 실제돔의 가벼운 복사본이다. React Element 객체 값의 모임이며 값으로 표현된 UI라고 생각하면 된다.
**가상 DOM**은 **상태가 변경**되면, **새로운 가상 DOM을 생성**하고 이전의 가상 DOM과 비교(diffing)하여 **변화된 부분을 감지 및** 변경된 부분만 실제 돔에 적용하여 효율적으로 화면을 업데이트한다.

- 메모리를 더 많이 사용하지만, Diffing 알고리즘과 배치 업데이트 덕분에 속도가 빠름
- UI 상태를 런타임에 동적으로 생성된 객체 트리로 표현하기 때문에, 컴파일 시점에서 사용되지 않는 코드를 식별하는 정적 분석이 어려움

```tsx
// Virtual DOM 생성
const vdom = {
  type: 'div',
  children: [
    { type: 'h1', props: { text: 'Hello' } },
    { type: 'p', props: { text: 'World' } },
  ]
};
```

