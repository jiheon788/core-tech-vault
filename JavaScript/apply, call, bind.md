# apply, call, bind
세 가지 모두 **JavaScript의 함수 호출 방식**을 제어하는 메서드로, **`this`의 값을 설정**하는 데 사용된다

### apply
- 함수를 **즉시 호출**하면서 `this`를 지정
- **형식:** `func.apply(thisArg, [arg1, arg2, ...])`

### call
- 함수를 **즉시 호출**하면서 `this`를 지정
- **형식:** `func.call(thisArg, arg1, arg2, ...)`

### bind
- `this`와 인자를 설정한 **새로운 함수를 반환**합니다. (즉시 실행 X)
- **형식:** `const newFunc = func.bind(thisArg, arg1, arg2, ...)`