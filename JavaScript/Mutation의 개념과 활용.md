
# Mutation의 개념과 활용
### Mutation
우선 데이터를 바꾸는 행위는 Reassignment(재할당)과 Mutation(변이)로 나눌 수 있다.

**Reassignment (재할당)**
[^1]원시타입은 불변(Immutable)이기 때문에 기존의 값을 변경하는 것이 아니라 새로운 값을 할당한다.

```jsx
let message = "Hello";
message = "World"; // 기존 "Hello"를 수정하는 게 아니라 새로운 "World"라는 값을 만들어서 message에 다시 할당
```

**Mutation (변이)**
Mutation은 데이터를 **직접 변경**하는 작업이다. 참조 타입에서는 데이터를 직접 변경할 수 있다.

```jsx
let arr = [1, 2, 3];
arr[0] = 5; // 배열 내부의 값을 직접 변경

const obj = {
	firstName: 'Jiheon',
	lastName: 'Park'
};

obj.firstName: 'hoon'; // 객체 내부의 값을 직접 변경
```

### 불변성(Immutability) 이란
불변성(Immutability)은 데이터가 생성된 후 그 값을 변경할 수 없는 성질이다. **데이터를 직접 변경하지 않고 복사본을 만들어 사용하는 방식**은 불변성을 지키기 위한 기술적 방법이다


**불변성의 이점:**
- 예측 가능한 코드
- 안정성

**불변성의 한계:**
하지만 불변성을 항상 유지하는 것이 성능적으로 최선은 아닐 수 있다. 복사 비용(Copy Cost)이 발생하기 때문이다. 
예시로 배열 데이터를 특정 키(`id`)를 기준으로 Map 형태로 변환하는 코드를 두가지 방식으로 작성하였다.

**불변성을 지킨 정규화**

```jsx
function normalize(data) {
    return data.reduce((result, record) => ({ ...result, [record.id]: record }), {});
}
```

- 각 반복마다 객체를 복사하여, 복사비용이 증가한다
- 0부터 n-1까지 반복하며 각 단계에서 복사 비용이 O(0), O(1), … , O(n - 1)로 누적된다.
- 등차수열의 합 공식을 적용하면, 총 연산 횟수는:

$$ 0 + 1 + 2 + \dots + (n - 1) = \frac{n(n - 1)}{2} $$

- 가장 큰 차항만 남겨 시간복잡도는 O(n^2)이다

$$ O(0) + O(1) + O(2) + \dots + O(n-1) = O(n^2) $$

**Mutation을 사용한 정규화**

```jsx
function normalizeWithMutation(data) {
    return data.reduce((result, record) => {
        result[record.id] = record; // 기존 객체를 직접 변경
        return result;
    }, {});
}
```

- 동일한 입력에 대해 동일한 출력이 제공하여 참조투명성을 지키며 부수효과가 없다.
- 객체를 복사하지 않고 바로 값을 변경하므로 시간복잡도는 O(n)로 줄어든다

> 단, 이 방식은 로컬 상태만 수정하기에 안전하다. 만약 외부 데이터를 변경한다면, 부수효과(Side Effect)가 발생할 수 있으므로 주의가 필요하다.

### 결론

불변성이 항상 최선은 아니며, 특정 상황에서는 변형(Mutation)이 더 적합할 수 있다.

- **불변성을 유지해야 할 때:**
    - 데이터가 외부에서 공유될 가능성이 있는 경우
    - 복잡한 로직에서 안정성과 디버깅이 중요한 경우
- **변형을 사용해도 될 때:**
    - 로컬에서만 사용하는 경우
    - 성능이 중요한 경우

### 참조

- [https://blog.bitsrc.io/understanding-javascript-mutation-and-pure-functions-7231cc2180d3](https://blog.bitsrc.io/understanding-javascript-mutation-and-pure-functions-7231cc2180d3)

[^1]: 기존 값을 수정하는 게 아니라 새로운 값을 생성해서 변수에 다시 할당하는 것
