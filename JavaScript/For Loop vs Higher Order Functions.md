

### 고차함수의 장점

함수형에서는 왜 while, for 문 보다 map, reduce같은 고차함수를 쓰는가? 검색해서 나온 장점은 다음과 같다.

- 선언적인 접근 방식으로 직관적이고, 고차함수를 사용했을 때 결합과 재사용이 쉬워진다.
- 원본데이터의 수정을 막아 불변성을 지킨다.

### 고차함수의 성능저하와 테스트

하지만 여러 테스트를 통해 고차함수의 오버헤드로 인한 성능저하를 확인할 수 있다.

[Testing an Imperative Loop versus Higher Order Functions in JavaScript](https://paramdeo.com/blog/testing-imperative-loops-versus-higher-order-functions-in-javascript)

[Performance of built-in higher-order function vs. for-in loop in Swift](https://medium.com/skoumal-studio/performance-of-built-in-higher-order-function-vs-for-in-loop-in-swift-166fa41b545f)

위 링크의 테스트 중 ‘화씨배열을 섭씨변환 → 조건 필터링 → 합계를 구하는 로직’을 자바스크립트로 구현해보았다.

```jsx
// 큰 배열 생성
const 화씨배열 = Array.from({ length: 1000000 }, () => Math.random() * 200 - 100); // -100 ~ 100 사이 값

// 섭씨 변환 함수
const 섭씨변환 = (화씨) => (화씨 - 32) * 5 / 9;
// 조건 함수
const 조건 = (섭씨) => 섭씨 > 0;

// 방법 1: 고차 함수 테스트
console.time("고차함수");
const sum1 = 화씨배열
  .map(섭씨변환) // 섭씨로 변환
  .filter(조건) // 조건에 맞는 값 필터링
  .reduce((acc, cur) => acc + cur, 0); // 합산
console.timeEnd("고차함수");

// 방법 2: for 루프 테스트
console.time("for루프");
let sum2 = 0;
for (let i = 0; i < 화씨배열.length; i++) {
  const 섭씨 = 섭씨변환(화씨배열[i]);
  if (조건(섭씨)) {
    sum2 += 섭씨;
  }
}
console.timeEnd("for루프");

// 결과
// 고차함수: 60.752ms
// for루프: 8.146ms
// -> 86% 차이 발생
```

고차함수는 체이닝을 통해 map, filter, reduce를 사용하면 반복문을 각각 실행하여 O(3n)의 시간복잡도, for 루프는 하나의 루프에서 모든 작업을 수행하기에 O(n)의 시간복잡도를 가져서 성능 차이가 발생할 수 밖에 없다. 그렇다면 체이닝이 없는 경우에는 어떨까?

```jsx
// 큰 배열 생성
const 화씨배열 = Array.from({ length: 1000000 }, () => Math.random() * 200 - 100); // -100 ~ 100 사이 값

// map 함수 방식
console.time("map 함수");
const 섭씨배열1 = 화씨배열.map(화씨 => (화씨 - 32) * 5 / 9);
console.timeEnd("map 함수");

// for 루프 방식
console.time("for 루프");
const 섭씨배열2 = [];
for (let i = 0; i < 화씨배열.length; i++) {
  섭씨배열2.push((화씨배열[i] - 32) * 5 / 9);
}
console.timeEnd("for 루프");

// map 함수: 36.645ms
// for 루프: 15.676ms
// -> 57% 차이 발생
```

체이닝을 사용할때보다는 적은 차이를 보이지만 그럼에도 for 루프가 빠르다. 이는 map 함수가 내부적으로 콜백함수를 호출하기 때문이다. **각 요소마다 콜백함수가 호출되므로 호출 스택 관리 및 컨텍스트 전환 비용**이 든다. for 루프는 이러한 함수 호출없이 순수하게 반복문을 실행하므로 호출 오버헤드가 없다.

### 결론

위 사례를 통해 고차함수의 성능저하는 명확함을 알 수 있다. 그럼에도 가독성 및 유지보수성을 위해 성능을 포기해야하는건가?

- 루프 메커니즘을 최적화할 필요가 많이 발생하지 않는다. (=고차함수로 인한 성능저하가 크리티컬하지 않다)
- 빠른 조회를 위해 해시맵을 사용하던가, 조기 중단이나 메모이제이션 등 다른 방법이 먼저 수행되어야한다.
- 성능 저하가 실제로 발생할 때 프로파일링을 해라. 성능때문에 고차함수를 사용하지 않는 것은 ‘조기 최적화’이다
- 고차함수를 다시 구현하여 더 빠른 방법을 제공하는 라이브러리도 있다. [https://github.com/codemix/fast.js](https://github.com/codemix/fast.js)

성능저하에 대한 우려보다 코드의 명확성과 재사용성을 높이는 이점이 크기에 고차함수를 사용한다.





