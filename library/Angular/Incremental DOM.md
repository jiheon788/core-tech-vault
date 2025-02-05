# Incremental DOM
DOM은 웹페이지에 대한 인터페이스로 브라우저가 웹페이지의 콘텐츠와 구조를 어떻게 보여줄 지에 대한 정보를 담고있다. DOM 업데이트를 효율적으로 처리하기 위해 대표적으로 리액트의 [[Virtual DOM]]과 앵귤러의 [[Incremental DOM]] 두가지가 접근 방식이 있다.

![[Pasted image 20250131205303.png]]
Angular v9의 Ivy 렌더러에서 도입된 증분DOM은 코드 변경점을 찾을 때 메모리상에 가상의 표현을 사용하지 않는다. 변경 사항이 발생하면 템플릿을 명령어 묶음(command set)으로 컴파일한 결과를 실행하여 DOM을 직접 조작한다.

- 상태를 유지하지 않으므로 계산과 DOM 작업이 반복되어 속도가 상대적으로 느림
- 메모리 사용량 감소 (모바일 기기에 기반한 애플리케이션에 적합)
- 템플릿을 명령어 묶음으로 변환하여 정적 분석을 통해 트리쉐이킹 유리

```tsx
function template(ctx) {
  elementStart(0, 'div');               // <div> 태그 열기
    elementStart(1, 'h1');              // <h1> 태그 열기
      text(2, ctx.title);               // 텍스트 노드 추가 (title 값)
    elementEnd();                       // <h1> 태그 닫기
    elementStart(3, 'p');               // <p> 태그 열기
      text(4, ctx.description);         // 텍스트 노드 추가 (description 값)
    elementEnd();                       // <p> 태그 닫기
  elementEnd();                         // <div> 태그 닫기
}
```