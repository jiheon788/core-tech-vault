# AngularJS vs Angular
### AngularJS의 MVC (1.x)

AngularJS는 [[MVC, MVVM#MVC|MVC(모델-뷰-컨트롤러]]) 아키텍처 패턴을 기반으로 동작하며, 이는 애플리케이션의 구조를 명확히 정의하고 유지보수성을 높이는 데 중점을 둔다.

- 컨트롤러는 `$scope`를 통해 모델과 데이터를 연결한다.
- 양방향 데이터 바인딩: 모델의 변경 사항이 자동으로 뷰에 반영되고, 뷰의 변경 사항이 모델에 업데이트된다
- AngularJS의 한계
	- 양방향 데이터 바인딩의 복잡성
	- 뷰(View)와 모델(Model)의 직접 연결
	- 변경 사항 추적의 어려움

### Angular의 MVVM (2.x ~)

[[Angular]](Angular 2 이상)는 [[MVC, MVVM#MVVM|MVVM(Model-View-ViewModel)]] 아키텍처 패턴을 채택하여 기존 AngularJS의 한계를 개선한다.

- 컴포넌트는 뷰모델(ViewModel)로 작동하며, 템플릿 바인딩을 통해 뷰와 상호작용한다. (**데이터 바인딩**을 통해 View와 자동으로 데이터를 동기화)
- 뷰는 Angular 디렉티브(예: `ngIf`, `[(ngModel)]`)와 템플릿 문법을 사용하여 데이터를 UI에 반영한다.
- Angular MVVM의 이점
	- 뷰와 모델의 분리
	- 데이터 흐름의 예측 가능성