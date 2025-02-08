# Directive

디렉티브는 DOM 요소의 동작이나 구조를 정의하는 데 사용된다. (component는 view (template)을 포함한 directive의 형태)

**구조 디렉티브 (Structural Directive)**
- DOM 요소의 추가/삭제를 담당
- 예: `ngIf`, `ngFor`
```html
<section class="admin-controls" *ngIf="hasAdminPrivileges">
	The content you are looking for is here.
</section>

<ul class="ingredient-list">
	<li *ngFor="let task of taskList">{{ task }}</li>
</ul>
```

**속성 디렉티브 (Attribute Directive)**
- 요소의 속성이나 스타일을 변경
- 예: `ngClass`, `ngStyle`

**커스텀 디렉티브 (Custom Directive)**
- 개발자가 새로운 동작을 정의
- 생성: `@Directive()` 데코레이터 사용