# message API
서로 다른 브라우징 컨텍스트에서 실행되는 두개의 독립된 스크립트 환경 간의 통신 (서로 다른 윈도우)

### postMessage
- 단방향, 일회성
- 다른 도메인도 가능 -> 도메인 검증 과정 필요

### MessageChannel API
- 1:1 커뮤니케이션
- 지속적인 양방향 커뮤니케이션
- 다른 도메인도 가능

>[!tip] postMessage vs MessageChannel API
>`window.postMessage`를 쓰게 되면 해당 window 객체에 직접적으로 메시지가 넘어가지만, `MessageChannel`은 직접적으로 window 객체를 통하는 방식이 아닌, **중간에 한 번 메시지를 중개해서 넘겨주는 역할**을 한다. 
>일대일 메시지 커뮤니케이션이 지속적으로 필요한 것이 아니라면, postMessage가 적합하다.
### BroadcastChannel API
- 1:N
- 같은 도메인
- 다른 창이나 탭에서 일어나는 유저의 행동 감지 목적 (특정탭에서 로그아웃하면 다른탭에서 반영 가능)

### 공유 워커 (SharedWorkers)
- 같은 도메인에서 접근하면 스크립트 공유 가능한 서비스 워커
- 일반 브라우저 JS 런타임과 독립적인 JS 런타임을 가짐
- BroadcastChannel과 유사하지만 조금 더 복잡할때 사용한다. (하나의 서버와 다양한 클라이언트 사이에서 상태공유)

