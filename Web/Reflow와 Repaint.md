# Reflow와 Repaint
### Reflow
`reflow`는 브라우저가 페이지의 ===레이아웃을 다시 계산하는 과정===이다. DOM의 구조가 변경되거나 CSS 스타일이 변경되면, 브라우저는 각 요소가 화면에 어떻게 배치될지 다시 계산해야 한다. 이 과정은 모든 자식 요소와 관련된 부모 요소까지 영향을 주기 때문에 비용이 많이 드는 작업이다. 

> [!example]
> CSS에서 요소의 `width`나 `height` 속성을 변경하면, 브라우저는 해당 요소뿐만 아니라 연관된 모든 요소의 배치를 다시 계산한다.

### Repaint
repaint는 ===계산 결과를 화면에 다시 그리는 과정===이다. 요소의 모양이나 스타일이 변경될 때 발생한다. 요소의 레이아웃은 그대로이고, 색상이나 배경 등의 스타일만 변경되는 경우이다.

> [!example]
>`background-color`

### Reflow와 Repaint 최적화 방법

1. **reflow를 유발하는 CSS 속성 사용을 최소화**
	 `width`, `height`, `margin`, `padding`, `border` 등의 속성은 요소의 레이아웃을 다시 계산하게 하므로 reflow를 일으킨다. 가능한 한 미리 CSS에서 스타일을 설정해 초기 로드 시에만 계산이 이루어지도록 하고, 이후에는 변경하지 않는 것이 좋다.

2. **CSS 애니메이션 최적화**
	애니메이션에 `transform`과 `opacity` 속성만을 사용하는 것이 성능에 유리하다. 이 두 속성은 GPU 가속을 사용할 수 있어 reflow를 일으키지 않고 repaint만 발생시키므로 CPU 자원을 적게 사용한다.

3. **`will-change` 속성 사용**
	CSS의 `will-change` 속성을 사용하여 브라우저에 특정 요소가 변경될 것이라고 미리 알릴 수 있다. 예를 들어, `will-change: transform`으로 미리 GPU에서 요소를 준비하게 하여 reflow 및 repaint에 미치는 영향을 줄일 수 있다. 

>[!warning]
> `will-change` 속성은 너무 자주 사용하면 메모리 낭비가 발생하므로 필요한 요소에만 적용해야 한다.

