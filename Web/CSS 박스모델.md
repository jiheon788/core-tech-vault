# CSS 박스모델

![[Pasted image 20250203191943.png]]

모든 [[HTMLxCSS#HTML|HTML]] 요소는 박스(box) 모양으로 구성되며, 이것을 박스 모델 이라고 한다 (html 요소가 어떻게 공간을 차지하는지 설명하는 개념)

- 콘텐츠: 실제내용
- 패딩: 안쪽여백
- 보더: 테두리
- 마진: 바깥 여백

### box-sizing 옵션
- box-sizing: border-box -> 가로세로 길이가 패딩과 테두리를 포함, 
- box-sizing: content-box ->  width와 height 속성은 기본적으로 content 영역만을 지정