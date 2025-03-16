# RxJS 개념과 구성요소 
RxJS(Reactive Extensions for JavaScript)는 [[반응형 프로그래밍]]으로 비동기 데이터 스트림을 효과적으로 관리 하기 위해 사용한다. 데이터 스트림이란 시간에 따라 전달되는 데이터의 흐름을 의미한다. 예를 들어 클릭 이벤트, 서버응답, 사용자 입력 등 모두 데이터 스트림이 될 수 있다.

함수형 프로그래밍 스타일로 비동기 작업을 처리하며, 옵저버블, 연산자, 구독 개념을 통해 작업을 구성한다.

>[!tip]
>**리액트에서는 왜 잘 쓰이지 않을까?**
>반응형 프로그래밍의 ‘상태 변화를 관찰하고 반응하는 방식의 동작’과 리액트의 ‘컴포넌트의 [[React의 상태 (State)|상태]] 변화가 view에 반영되는 동작‘은 비슷하나 목적이 다르다
>- 리액트: 컴포넌트 렌더링에 중점 
>  - 반응형 프로그래밍: 이벤트 스트림에 중점

#### **Observable (관찰 가능한)**

Observable은 값을 푸시(Push-Based) 방식으로 생성하며, 지연 실행을 통해 1개의 객체에서 0개 또는 여러 값을 방출하고, 구독을 취소할 수 있다.

- Push-Based 프로듀서
- 지연 실행
- 1개의 객체에서 0 또는 여러 값을 방출 (zero or multiple values)
- 구독 취소 가능

```tsx
const button = document.getElementById('button');

// fromEvent로 생성
const observable = fromEvent(button, 'click')

// 구독
const subscription = observable.subscribe(event => console.log(event));

// 구독 해제
subscription.unsubscribe();

// ----------
// new Observable로 직접 생성
const observable = new Observable(function subscribe(subscriber) {
  const id = setInterval(() => { 
    subscriber.next('hi');
  }, 1000);
});
observable.subscribe((x) => console.log(x));
```

- **생성**: `new Observable` 또는 RxJS의 `of`, `from` 같은 생성 함수로 생성.
- **구독**: `subscribe`를 호출하여 데이터 수신을 시작.
- **실행 취소**: `unsubscribe`로 실행 중단 가능.

### Observer (관찰자, 소비자)

Observable에서 전달된 값의 소비자(Consumer)이다. next, error, complete 콜백 함수의 인터페이스를 가지며 **어떻게 소비할지 명시**하는 역할이다.

```tsx
// Observable에서 전달된 각 유형에 대한 콜백 인터페이스 (next, error, complete)
const observer = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

observable.subscribe(observer); 
```

#### Subscriptions (구독)

`Subscription`은 **Observable**이 **Observer**에 데이터를 발행하도록 연결(구독)하는 과정이다. Observable은 [[Cold Stream vs Hot Stream#Cold Stream|콜드 스트림]]이므로, 구독을 통해 실제로 데이터를 발행하기 시작한다.

```tsx
// 클릭이벤트에 Observable 등록
import { fromEvent } from 'rxjs'

const button = document.getElementById('button');
const observable = fromEvent(button, 'click')

// A.subscribe(B) : A send notification to B (옵저버가 인자로 넘어감)
// 볼드 처리가 옵저버로 넘어간다 (실사용에서는 넥스트, 에러, 컴플리트 모두 넘긴다)
const subscription = observable.subscribe(event => console.log(event));

// 내부적으로 옵저버를 만들고 subscriber를 넣어줌
// subscribe(observer?: PartialObserver<T>): Subscription;
// subscribe(next: (value: T) => void, error: null | undefined, complete: () => void): Subscription; 

subscription.unsubscribe();
```

#### Subscriber

Observable의 데이터를 처리하는 `Observer`의 구체적인 구현체이다.

1. Observable에서 값을 전달하는 매개체로 Subscriber가 생성되고,
2. 구독을 하면 Subscriber를 통해 값을 전달한다

```tsx
// 데이터 소스가 내부에 있음: 콜드 스트림
const observable = new Observable(subscriber => {
  console.log('Observable started');  // 2. 구독 시 발행(호출)
  subscriber.next('A'); // 매개체(subscriber)를 통해 값을 전달
  subscriber.next('B');
  subscriber.complete(); 
});

// observer.subscribe(observer)

// 1. 구독자가 정의한 콜백함수
observable.subscribe({ // 옵저버
  next: value => console.log(`Received: ${value}`),
  error: err => console.error(`Error: ${err}`),
  complete: () => console.log('Stream completed'), 
});

// Received: A
// Received: B
// Stream completed
```

#### Subject
Subject는 Observable이자 Observer 이다.

- Observable로서 데이트 스트림을 다른 Observer에게 전파 (멀티캐스트로 작동)
- 다른 Observable로부터 데이터를 받을 수 있다. (subscribe, unsubscribe)

Subject의 종류
- `BehaviorSubject`: 초깃값을 가지며 새로운 구독자는 가장 최근값(현재값)을 즉시 전달 받음
- `ReplaySubject`: 특정 개수 또는 시간 동안 데이터를 기록하고 새로운 구독자에게 이전 데이터를 재생 **(버퍼에 캐싱된 만큼)**
- `AsyncSubject`: Observable **실행이 완료될 때** 마지막 값만 전달
- `Void Subject:` 값이 아닌 이벤트 자체만 전달하며, 데이터가 중요하지 않을 때 사용


#### Pipe
[[RxJS 연산자]]의 조립 라인
