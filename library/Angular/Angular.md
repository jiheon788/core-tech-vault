# Angular
### **Angular의 특징**

Angular는 Google에서 개발한 오픈소스 웹 애플리케이션 프레임워크이다

- 단일 페이지 애플리케이션(SPA) 지원
- Angular는 모듈성과 재사용성을 강조한 Component 기반 구조를 제공
- 의존성 주입([[Dependency Injection (DI)]]): 효율적인 서비스 관리 및 객체 의존성 해결을 제공
- 양방향 [[데이터 바인딩(Data Binding)]]: 데이터와 뷰가 실시간으로 동기화
- [[Typescript]] 기반
- [[Angular CLI]] 지원
- 다양한 내장 [[Directive]]와 파이프
- 뷰 캡슐화
- [[RxJS]] 기반으로 비동기 데이터 스트림 
- [[Incremental DOM]] 렌더링 엔진 사용


>[!tip]
>앵귤러는 브라우저의 shadowDOM API를 사용하여 뷰를 캡슐화한다. 쉐도우DOM은 특정 요소 안에 캡슐화된 DOM을 만드는 기술이다.

>[!tip] Angular에서 RxJS를 많이 사용하는 이유
>Angular의 많은 핵심 기능은 RxJS의 옵저러블을 기반으로 설계되었다. RxJS를 사용하여 비동기 데이터 스트림을 효율적으로 관리하고, 복잡한 비동기 작업을 간단하게 처리할 수 있도록 돕는다.
>
>Angular의 Zone.js는 모든 비동기 작업(setTimeout, HTTP 요청 등)을 감지하고, **앱 전체의 Change Detection을 실행**한다. 이는 비효율적인 DOM 업데이트를 초래할 수 있다. RxJS를 활용하면 Change Detection이 불필요하게 실행되지 않도록 데이터 흐름을 제어할 수 있다.


### **구성 요소**

**Modules**

- 애플리케이션의 기능을 논리적으로 그룹화하여 관리하는 컨테이너
- Angular 애플리케이션은 최소 하나의 루트 모듈(AppModule)을 가짐
- NgModule 데코레이터를 사용하여 정의하며, **구성요소(Component, Directive, Pipe, Service 등)을 하나로 묶어 구성**하고 의존성을 주입
- e.g. 루트 모듈(AppModule), 기능 모듈(Feature Module)

**Components**

- 애플리케이션의 UI를 담당하는 기본 단위
- 뷰(HTML)와 로직(TypeScript)를 결합하여 애플리케이션의 UI를 정의
- 하나의 애플리케이션은 여러 컴포넌트로 구성
    - Module is tree of components
- @Component 데코레이터를 사용하여 정의

**Services**

- 컴포넌트 간의 데이터를 공유하거나 비즈니스 로직을 구현
    - 컴포넌트의 뷰와 직접적인 관계가 없는 로직
- DI를 통해 컴포넌트에 주입됨
    - @Injectable 데코레이터를 사용하여 정의

**Templates**

- HTML과 Angular 지시문(Directive)을 결합하여 뷰를 정의

```tsx
@Component({
  template: ` <p>Title: {{ taskTitle }}</p> `,
})
export class TodoListItem {
  taskTitle = 'Read cup of coffee';
}

// 결과물:
// <p>Title: Read cup of coffee</p>
// HTML 안쪽에 동적 데이터 프로퍼티를 반영하는 것을 문자열바인딩 (interpolation) 이라고 한다
```



