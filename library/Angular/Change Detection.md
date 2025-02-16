# Change Detection

**Change Detection**은 상태 변화(데이터 변경)를 감지하고, 이를 기반으로 DOM을 업데이트하는 메커니즘이다. (**DOM을 업데이트할지 체크하는 것**)

Angular뿐 아니라 대부분의 Frontend Framework에서 성능적으로 항상 문제인 DOM manipulation을 최소화 하려 한다.

### Change Detection 프로세스
1. 트리거
    - Zone.js가 변경 탐지 트리거
2. 컴포넌트 트리 순회
    - 루트 → 하위 컴포넌트 순차적으로 확인하며 변경 사항이 있는지 검사
    - [[Incremental DOM]]을 사용해 데이터 바인딩 확인
3. DOM 업데이트

>[!tip] 
>[[React의 렌더링 방식#리액트에서 상태변화가 일어나고 화면이 변하는 과정|React의 Change Detection]]
>1. 상태 변경 트리거 (setState, useState, useReducer)
>2. 새로운 Virtual DOM 생성
>3. 새로운 가상 DOM과 이전 가상 DOM을 비교 (Diffing 알고리즘)
>4. Diff 결과를 기반으로 변경된 부분만 실제 DOM에 반영


### Zone.js
Zone.js는 비동기 작업 간에 실행 컨텍스트를 유지하여 비동기 작업을 추적할 수 있도록 한다. (JS VM의 스레드 로컬 저장소와 유사하게 동작)

1. `setTimeout`, HTTP 요청, DOM 이벤트 등 비동기 작업이 실행되면 Zone.js가 가로채서 해당 Zone 컨텍스트에서 작업 수행
2. 작업이 완료되면 앵귤러의 변경 탐지 트리거
3. 변경된 상태 기반으로 DOM 업데이트


### NgZone
**NgZone**은 Angular가 변경 탐지를 관리하기 위해 사용하는 서비스이다. (비동기 코드의 실행 시점을 포착하기 위함) 
Angular는 NgZone을 활용해 애플리케이션에서 발생하는 **비동기 작업**(예: `setTimeout`, HTTP 요청 등)을 추적하고, 비동기 작업이 완료되면 **자동으로 Change Detection을 트리거**한다.

### Change Detection 전략

- Default
    - 모든 컴포넌트와 하위 컴포넌트를 순회하며 변경
    - 데이터 변경 여부와 상관 없이 확인
- OnPush
    - @Input으로 전달된 데이터가 변경되거나, 이벤트가 발생할 때만 변경 탐지
    - 불변 데이터와 함께 사용하여 성능 최적화 목적
    
    ```tsx
    @Component({
      selector: 'app-my-component',
      template: `{{ value }}`,
      changeDetection: ChangeDetectionStrategy.OnPush
    })
    export class MyComponent {}
    ```