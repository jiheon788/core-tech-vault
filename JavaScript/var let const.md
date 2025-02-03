- var: 함수 스코프를 따르며 재선언 가능
```javascript
// 함수스코프: 함수 안에서 선언되면, 그 함수 전체에서 접근 가능하지만, 함수 바깥에서는 접근 불가능!
function test() {
    var x = 10;
    if (true) {
        var y = 20; // 함수 전체에서 접근 가능 (if 블록 안에서 선언했어도!)
    }
    console.log(y); // 20 (가능)
}
console.log(x); // ❌ 에러 (test 함수 밖에서는 x를 모름)
```

- let: 블록 스코프, 재선언 불가 및 선언전에 사용하면 에러
- const: 블록 스코프, 재선언과 재할당불가
```javascript
// 블록스코프: 선언된 `{}` 블록 내에서만 유효하고, 밖에서는 접근할 수 없음!
function test() {
    if (true) {
        let a = 10;
        const b = 20;
    }
    console.log(a); // ❌ 에러 (a는 if 블록 밖에서 접근 불가)
    console.log(b); // ❌ 에러 (b도 if 블록 밖에서 접근 불가)
}
```