# Backpressure (역압)

### Backpressure 정의
발행자가 끊임없이 emit하는 무수히 많은 데이터를 적절하게 제어하여 데이터 처리에 과부하가 걸리지 않도록 제어하는 기술
프로듀서가 데이터를 너무 빠르게 생성하고 컨슈머가 이를 제때 처리하지 못할 때 발생하는 문제이다. (데이터 흐름의 불균형)

- 데이터 소비 속도가 생산 속도를 따라가지 못할 때 발생
	- 프로듀서의 생성 속도가 너무 빠르거나
	- 컨슈머의 처리 속도가 상대적으로 느림
	- 네트워크 지연 또는 시스템 부하
- 대기중인 데이터가 지속적으로 쌓이게 되어 오버플로나 시스템 다운이 일어날 수 있음

### Backpressure 처리 방법

- 손실 작업 (Loss)
	- Debounce Approach
	- Throttle Approach
	- Pausable Approach

- 무손실 작업 (Lossless)
	- Pausable Approach
	- Buffer Count Approach
	- Buffer Time Approach

e.g.
- 유튜브의 실시간 라이브 댓글은 일정갯수의 댓글을 쌓아 한번에 표출한다 → (Lossless) Buffer Count Approach
- 한번에 직원들의 정보를 대량으로 수정할 때는 요청을 나누어서 보낸다. 요청을 보내고 응답이 오면 다음 요청 → (Lossless) Pausable Approach