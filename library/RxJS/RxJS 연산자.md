# RxJS 연산자

### 생성 연산자 (Creation)
- `ajax`
- `create`
- `defer`
- `empty`
- `from`
- `fromEvent`
- `generate`
- `interval`
- `of`
- `range`
- `throw`
- `timer`

### 조합 연산자 (Combination)
- `combineAll`, `combineLatest`
	- 다수의 observable을 결합하여 각 observable에서 값이 발행될 때마다 가장 최근 값들을 조합
	- 모든 observable이 최소 하나의 값을 방출할 때 까지 초기 값을 방출 x
	- 모든 Observable의 최신 값을 유지하면서 동작.
	- v8에서 deprecated -> combineLatestAll 변경
```tsx
// 3가지 observable 병합
combineLatest(timer1$, timer2$, timer3$).subscribe(
	([t1, t2, t3]) => {...}
)

// 프로젝션 함수를 사용
combineLatest(
	timer1$, 
	timer2$, 
	timer3$,
	(t1, t2, t3) => {...}
)
```

- `concat`
	- 여러개의 observable을 순차적으로 실행하고, 이전 Observable이 완료되면, 다음 Observable을 실행
	- 이전 observable이 완료될 때까지 다음 구독을 시작할 수 없음
	- observable을 직접 인자로 받는다 (여러 observable을 병합하여 하나의 observable을 반환) -> 여러 개의 Observable을 직접 연결할 때 사용
	- 순서 중요 -> 처리량이 더 중요하다면 merge 사용 권장
	- `concatAll`: Observable을 반환하는 Observable을 처리

```tsx
concat(obs1, obs2, obs3).subscribe(console.log);
```
 - `merge`
	- 모든 Observable에서 발행된 값을 순서와 상관없이 병합
	- 처리량 중요 -> 순서가 더 중요하면 concat 사용 권장
	- `mergeAll`: Observable을 반환하는 Observable을 처리

- `zip`
	- 각 Observable 값을 동시에 쌍으로 묶어서 발행

- `pairwise`
	- 이전 값과 현재 값을 튜플로 묶음 -> \[(N-1)번째, N번째)\]
	- pairwise의 첫번째 발행 값은? → 첫번째 방출에서는 방출하지 않음

-  `withLatestFrom`
	- 메인 Observable이 값을 방출할 때만 실행.
	- 참조하는 다른 Observable의 가장 최신 값과 함께 방출
	- 참조하는 Observable은 자체적으로 값을 방출해도 아무 일도 일어나지 않음
	- 메인 Observable이 실행될 때만 다른 Observable의 최신 값을 가져오고 싶다면 사용

### 조건 연산자 (Conditional)
- `defaultIfEmpty`
- `every`
- `iif`
- `sequenceEqual`

### 에러 핸들링 연산자 (Error Handling)
### 필터링 연산자 (Filtering)
### 멀티캐스팅 연산자 (Multicasting)
RxJS 연산자는 기본적으로 [[Cold Stream vs Hot Stream#Cold Stream|콜드 스트림]]이다. 멀티캐스팅 연산자를 통해 [[Cold Stream vs Hot Stream#Hot Stream|핫 스트림]]으로 만들 수 있다.
- `publish` : v7 deprecated -> share 연산자 사용
- `multicast`
- `share`
	- 값을 공유하며, 재구독 시 소스 Observable이 다시 시작하지 않음
	- share ~= multicast(() => new Subject()).refCount()
		- Subject를 사용해 데이터를 다중 구독자와 실시간으로 공유
		- 구독자가 없으면 소스 Observable과의 연결이 자동으로 해제(refCount 사용)
		- 새로운 구독자가 추가되더라도 소스 Observable은 현재 상태를 유지
		- 매번 새로운 데이터가 필요하고, 기존 데이터를 유지할 필요가 없을 때 사용
- `shareReplay`: 
	- 이전 n개의 값을 저장하여 새로운 구독자에게 제공
	- 이전 데이터를 유지해서 새로운 구독자에게도 제공할 필요가 있을 때 사용

### 변환 연산자 (Transformation)
- `map`: 각각이 값에 콜백을 적용
- `concatMap`: 내부 Observable을 순서대로 처리
- `switchMap`: 이전 Observable을 취소하고 최신 값 처리
- `mergeMap`: 내부 Observable 변환 및 병합
- `exhaustMap`: 내부 Observable이 완료될 까지 새 요청 무시
- `scan`: 누적 값 계산

### 유틸리티 연산자 (Utiilty)
