# Cold Stream vs Hot Stream

### Cold Stream
- 옵저러블의 파이프라인은 새 구독자에 대해 처음부터 설정되고 각 구독자는 옵저러블의 로직에 **독립적인 실행**
- ===구독 시점에 데이터 발행. 각 구독자에게 독립적 실행 (생성 → 구독 → 발행)===
- 유니캐스트
- 여러 구독자가 있는 경우 동일한 계산 또는 데이터 페치를 여러 번 실행
- RxJS의 옵저버블은 기본적으로 Cold observable

```tsx
// 구독이 시작된 후 난수 생성
const cold = Rx.Observable.create((observer) => {
  observer.next(Math.random())
})

cold.subscribe((a) => console.log(`Subscriber A: ${a}`))
cold.subscribe((b) => console.log(`Subscriber B: ${b}`))
// 동시에 구독했지만 다른 값을 받음
```


### Hot Stream
- 모든 구독자 간에 단일 실행 경로 공유, 새로운 구독자가 구독하면 기존 데이터 스트림에 가입, 파이프라인은 한번 설정되고 결과는 모든 구독자에게 브로드캐스트 됨
-  ===생성 시점부터 계속 데이터 발행, 구독 여부와 관계없이 실행 (생성 → 발행)===
- 멀티캐스트
- **중복 계산을 피하고 싶을 때 효율적**
- 다중 구독 관계가 생기면서 구독과 해제의 관리가 주의 필요

```tsx
// 옵저버블 생성 함수의 외부에서 난수 생성
const x = Math.random()

const hot = Rx.Observable.create((observer) => {
  observer.next(x)
})

hot.subscribe((a) => console.log(`Subscriber A: ${a}`))
hot.subscribe((b) => console.log(`Subscriber B: ${b}`))
```

###### 요약
| 특징          | **Cold Stream** | **Hot Stream**    |
| ----------- | --------------- | ----------------- |
| 데이터 발행 시점   | 구독 시 시작         | 생성 시 시작           |
| 구독자 간 실행 경로 | 독립적             | 공유                |
| 캐스팅 방식      | 유니캐스트           | 멀티캐스트             |
| 중복 계산       | 발생              | 없음                |
| 목적          | 구독자마다 독립된 결과 필요 | 데이터 공유로 효율적 처리 필요 |

### Cold → Hot으로 바꾸는법

| 방법                   | 캐싱  | 멀티캐스트 | 주요 용도                                                                                        |
| -------------------- | --- | ----- | -------------------------------------------------------------------------------------------- |
| `publish`- `connect` | ✗   | ✓     | - 한 번 실행되면 여러 구독자가 같은 데이터를 받을 필요가 있는 상황<br>- 보통 refCount()와 많이 쓰임<br>- 명시적으로 connect() 호출 필요 |
| `Subject`            | ✗   | ✓     | - 외부에서 데이터 푸시 또는 Observable 제어                                                               |
| `shareReplay`        | ✓   | ✓     | - 캐싱이 필요하며, 새로운 구독자도 이전 데이터를 받을 필요가 있는 경우                                                    |
| `publishReplay`      | ✓   | ✓     | - Observable의 실행을 수동으로 제어하고 싶을 때<br>- 명시적으로 connect() 호출 필요                                  |

여러 구독자가 같은 데이터를 받는 법: 
```tsx
const cold = Rx.Observable.create((observer) => {
  observer.next(Math.random())
})

const hot = cold.publish() // publish: Hot으로 변경, 실시간으로 동일한 값 공유

hot.subscribe((a) => console.log(`Subscriber A: {a}`))
hot.subscribe((b) => console.log(`Subscriber B: {b}`))

// 구독 후에 connect를 호출하면 값이 방출되기 시작
hot.connect()
```


구독자가 없더라도 데이터 스트림 (이벤트)를 방출하는 방법:
```tsx
import { Subject } from 'rxjs';

const subject = new Subject<number>();

// 구독자가 없어도 데이터를 직접 발행
setInterval(() => subject.next(Math.random()), 1000);

subject.subscribe(val => console.log('Subscriber 1:', val));
setTimeout(() => subject.subscribe(val => console.log('Subscriber 2:', val)), 3000);
```


첫 구독자가 생길 때부터 캐시된 데이터 없이 데이터 스트림 (이벤트)를 방출하는 방법
```tsx
import { interval } from 'rxjs';
import { share } from 'rxjs/operators';

const hot = interval(1000).pipe(share());

hot.subscribe(val => console.log('Subscriber 1:', val)); // 즉시 구독
setTimeout(() => hot.subscribe(val => console.log('Subscriber 2:', val)), 3000); // 3초 후 구독
```

첫 구독자가 생길 때, 직전의 캐시된 데이터를 포함하여 데이터 스트림 (이벤트)를 방출하는 방법: 
```tsx
import { interval } from 'rxjs';
import { shareReplay } from 'rxjs/operators';

const hot = interval(1000).pipe(shareReplay(1)); // 최신 값 1개를 캐시

hot.subscribe(val => console.log('Subscriber 1:', val)); // 즉시 구독
setTimeout(() => hot.subscribe(val => console.log('Subscriber 2:', val)), 3000); // 3초 후 구독
```