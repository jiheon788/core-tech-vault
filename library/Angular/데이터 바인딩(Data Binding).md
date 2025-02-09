# 데이터 바인딩(Data Binding)
데이터 바인딩은 템플릿과 컴포넌트 간에 데이터를 동기화하는 것이다.


**Angular의 데이터 바인딩**
- 단방향 바인딩 (one-way Binding)
    - 문자열 바인딩 (Interpolation)
        - 컴포넌트에서 뷰로 데이터를 전달
        - e.g. `{{ propertyName }}`
    - 속성 바인딩 (Property Binding)
        - DOM 속성에 데이터를 바인딩
        - e.g. `[disabled]="true"
        
```html
<button [disabled]="hasPendingChanges"></button>
```

	- 이벤트 바인딩 (Event Binding)        
		- 뷰에서 컴포넌트로 이벤트를 전달
		- (뷰에서 컴포넌트의 메서드를 호출)
		- e.g. `(event)="handlerMethod()"`
        
```tsx
<button (click)="saveChanges()">Save Changes</button>
// 이벤트 객체 전달은 앵귤러에서 제공하는 $event 변수를 함수 실행 인자로 전달
<button (click)="saveChanges($event)">Save Changes</button>
```
        
- 양방향 바인딩 (Two-way Binding)
    - 데이터와 뷰가 실시간으로 동기화됨
    - 양방향 바인딩은 데이터 변경의 흐름을 추적하기 어려워 디버깅이 복잡해 질 수 있어서 권장하지 않는다.
    - e.g. `[(ngModel)]="propertyName"`

>[!tip]
>정확히는 양방향 바인딩을 지원하지 않는다. 우리가 양방향 바인딩이라 부르는 것은 프로퍼티 바인딩과 이벤트 바인딩을 조합한 것이다. 
>이는 ngModel지시자가 ngModel로 프로퍼티 바인딩된 속성과 ngModelChange로 이벤트 바인딩된 소스를 가지고 있어서 가능한 문법적 편의이다.
>```tsx
<input [(ngModel)]='data'>
<input [ngModel]='data' (ngModelChange)='data = $event'>
>```
>`[()]` 기호는 ngModel만의 전유물이 아니라 컴포넌트, 지시자에도 양방향 바인딩을 사용할 수 있다.