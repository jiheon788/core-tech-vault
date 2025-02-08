# requestIdleCallback
`requestIdleCallback`은 브라우저가 유휴 시간(즉, 주된 작업이 끝나고 남은 여유 시간)에 함수를 실행하도록 예약하는 API이다. 페이지의 주요 렌더링 성능에 영향을 주지 않으면서 덜 중요한 작업(예: 분석 데이터 처리, 캐시 정리 등)을 효율적으로 처리할 수 있다.

- 브라우저가 심심할 때 해도 되는 작업을 맡기는 도구
- **모든 브라우저에서 지원되지 않는다** (특히 Safari). 
	- `setTimeout`으로 폴리필할 수 있다.
- 중요한 작업은 사용하지 말고, 페이지의 퍼포먼스에 덜 민감한 작업에 활용

##### 기본 문법
```javascript
requestIdleCallback(callback, options);
```

- **callback**: 브라우저가 유휴 시간일 때 호출할 함수
- **options** _(선택 사항)_: `timeout`을 설정할 수 있어, 브라우저가 유휴 시간이 없더라도 일정 시간이 지나면 강제로 실행

```javascript
requestIdleCallback((deadline) => {
  while (deadline.timeRemaining() > 0 && tasks.length > 0) {
    doTask(tasks.pop());
  }
});
```

- `deadline.timeRemaining()`은 [^1]남은 유휴 시간을 밀리초 단위로 반환
- `tasks`라는 작업 목록을 남은 유휴 시간 동안 하나씩 처리

##### Usecase
- 백그라운드 데이터 처리
- 비동기 로깅, 분석 데이터 수집
- 비중요한 DOM 업데이트

---

- [^1]: 다음 중요한 작업(예: 렌더링, 사용자 입력 처리 등)을 수행하기 전까지 남은 "여유 시간"