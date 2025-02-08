# Top-Level Await

- 모듈의 최상위 레벨에서 **await**를 사용하는 것
- **Top-level await**를 사용한 모듈이 **하나의 거대한 async 함수처럼 동작,**


```jsx
// A.mjs
let todoList;

const response = await fetch("<https://jsonplaceholder.typicode.com/todos/1>");
todoList = await response.json();

export { todoList };

//----------------------
// B.mjs
import { todoList } from "./todoList.mjs";

console.log(todoList); 
// {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
```
B 모듈에서는 todoList로 바로 접근을 해도, 비동기 처리가 완료됐다는 것을 보장하기 때문에 원하던 결과를 얻을 수 있게 됩니다.
