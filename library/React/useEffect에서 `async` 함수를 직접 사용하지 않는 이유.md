# useEffect에서 `async` 함수를 직접 사용하지 않는 이유
[[Hooks#useEffect|useEffect]]에서 `async` 함수를 직접 사용하지 않고, 즉시 실행 함수(IIFE)로 감싸는 이유

- `useEffect`는 클린업 함수를 반환해야 하는데, **async 함수는 `Promise`를 반환**하므로 React의 생명주기 관리와 충돌
- [[즉시실행함수 (IIFE)]]를 사용하면, `useEffect` 내부에서 안전하게 `async` 코드를 실행할 수 있음

```tsx
// ❌ 이렇게 하면 에러가 발생할 수 있음
useEffect(async () => {
  const data = await fetchData(); 
}, []);

// ✅ IIFE(즉시 실행 함수)를 사용
useEffect(() => {
  (async function fetchData() {
    const data = await fetchSomething();
    console.log(data);
  })();
}, []);

// ✅ 함수를 따로 선언 후 실행
useEffect(() => {
  async function fetchData() {
    const data = await fetchSomething();
    console.log(data);
  }
  
  fetchData();
}, []);
```
