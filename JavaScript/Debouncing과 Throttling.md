# Debouncing과 Throttling
### 디바운싱(debouncing) 
- 연이어 호출되는 함수들 중 **마지막 함수(또는 제일 처음)만 호출하도록 하는 것**
- 사용자의 입력이 끝난 후에 작업을 수행해야 할 때 유용
- 검색어 입력 시 사용자가 타이핑을 멈춘 후에만 서버에 요청을 보내는 경우

**구현 코드**
연속 호출 중 마지막 호출만 처리하도록 해야한다. setTimeout과 clearTimeout을 활용하여 구현이 가능하다. 타이머가 계속 초기화되기 때문에 불필요한 함수 호출은 일어나지 않는다

```jsx
function debounce(func, delay) {
  let timeoutId;

  return function(...args) {
    **// 기존의 타이머가 존재하면 취소**
    if (timeoutId) {
      clearTimeout(timeoutId);
    }

    // 새로운 타이머를 설정
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}

function handleResize() {
  console.log('창 크기가 변경되었습니다!');
}

const debouncedResize = debounce(handleResize, 300);
window.addEventListener('resize', debouncedResize);
```

### 쓰로틀링(throttling)
- 일정 시간 동안 함수를 한 번만 실행하도록 제어하는 것
- 특정 이벤트가 너무 자주 발생하는 것을 방지하는 데 사용
- 스크롤 이벤트나 창 크기 조정 이벤트처럼 연속적인 이벤트에서 자주 사용

**구현 코드**
쓰로틀링은 특정 함수가 연속 호출될때 일정 시간 간격으로만 실행되도록 제한된다.
```jsx
function throttle(func, delay) {
  let lastCall = 0; // 마지막으로 호출된 시간을 기록

  return function(...args) {
    const now = new Date().getTime(); // 현재 시간
    if (now - lastCall >= delay) {
      // 일정 시간이 지났을 때만 함수 실행
      lastCall = now;
      func.apply(this, args);
    }
  };
}
```