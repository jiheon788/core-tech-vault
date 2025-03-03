# this
Java에서 this는 인스턴스 자신(self)을 가리키는 참조변수이다. 객체가 자신에 대한 참조 값을 가지고 있다는 뜻이다. 매개변수와 객체 자신이 가지고 있는 멤버변수명이 같을 경우 이를 구분하기 위해 사용된다.

```java
public Class Person {

  private String name;

  public Person(String name) {
    this.name = name; // this.name과 그냥 name은 완전히 다른 놈이다.
  }
}
```

하지만 자바스크립트에서서는 this에 바인딩되는 객체는 한가지로 고정되는게 아니라 해당  ===함수 호출 방식에 따라 this에 바인딩되는 객체가 달라진다.===

### this 바인딩 규칙

1. `new` 사용했을 때 해당 객체로 바인딩
2. [[apply, call, bind]]와 같은 명시적 바인딩을 사용했을 때 인자로 전달된 객체에 바인딩
3. `obj.method()`와 같이 객체의 메소드로 호출할 경우 해당 객체에 바인딩
4. 위 조건에 해당 없다면 
	- 브라우저에서는 `window` 객체
	- 엄격모드(`use strict`)일 경우, `undefined`
5. 위 규칙이 중복된다면 상위 규칙에 따라 `this`값을 설정한다.
6. 화살표 함수의 경우, 위 규칙 모두 무시하고 생성된 시점에서 주변 스코프의 `this`값을 받는다.