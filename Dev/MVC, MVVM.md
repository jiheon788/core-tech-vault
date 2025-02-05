# MVC, MVVM 아키텍쳐
아키텍쳐란 코드의 재사용성과 유지보수성을 높이기 위해 설계되는 구조를 말한다. 소프트웨어 개발 과정에서 코드 품질을 유지하고 복잡성을 효과적으로 관리하기 위해 다양한 아키텍쳐가 나타났다.

### MVC
![[Pasted image 20250203212037.png]]


Model (데이터), View (화면), Controller (컨트롤러)로 구성된 아키텍처 패턴이다.

1. Model의 데이터를 받아서 화면에 그리고,
2. 화면으로부터 사용자의 동작을 받아 Controller가 처리
3. Model을 업데이트하고 결과를 View에 반영

단순하고 직관적이지만 **View와 Model 사이의 의존성이 높다는 문제**가 있다. 또한 다수의 View와 Model이 컨트롤러를 통해 연결되기에 컨트롤러가 불필요하게 커지는 형상이 발생한다. (Massive-View-Controller)


>[!tip]
>**초창기 MVC**
>- Model: 데이터베이스
>- View: HTML/CSS/JS
>- Controller: 라우터를 통해 데이터 처리 및 새로운 HTML을 만드는 백엔드
>
**jQuery 이후의 MVC**
>
>- Model: ajax를 통해 받는 서버 데이터
>- View: HTML/CSS
>- Controller:JS가 서버의 데이터를 받아 화면을 업데이트 및 이벤트를 처리해서 서버에 데이터를 전달


### MVVM
![[Pasted image 20250203212221.png]]

MVC 패턴의 Controller 대신 ViewModel (View 를 표현하기 위해 만들어진 View 만을 위한 Model)이라는 개념이 도입되며 선언적인 방식으로 개선되었다.

1. **Model**로부터 처리 결과를 받아 ViewModel이 이를 관리
2. ViewModel은 **데이터 바인딩**을 통해 View와 자동으로 데이터를 동기화
3. View의 사용자 요청은 ViewModel을 통해 처리

MVVM 패턴은 데이터 바인딩과 의존성 주입 등의 기능을 사용하여 유연하고 테스트 가능한 코드를 작성할 수 있다.