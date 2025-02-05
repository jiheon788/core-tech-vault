# 인터넷 창에 www.google.com를 입력하면 생기는 일
### 1.  DNS 조회

사용자가 "[www.google.com"을](http://www.google.xn--com"-jy1s/) 입력하면, 브라우저는 먼저 이 도메인 이름을 IP 주소로 변환해야한다. (aka. DNS 조회, DNS Lookup) 
브라우저는 캐시된 DNS 기록을 먼저 확인하고, 없으면 로컬 DNS 서버에 요청하여 "[www.google.com"에](http://www.google.xn--com"-eg0s/) 해당하는 IP 주소를 얻는다.

### 2. TCP 연결 수립
IP 주소가 확인되면, 브라우저는 서버와 [[TCP vs UDP#TCP|TCP]]연결을 수립한다. 이 과정에서 브라우저는 서버와 [[3-way handshake]]를 수행한다. 
### 3. HTTP 요청

TCP 연결이 수립되면, 브라우저는 [[HTTP vs HTTPS#HTTP (HyperText Transfer Protocol)|HTTP]] 또는 [[HTTP vs HTTPS#HTTPS (HyperText Transfer Protocol Secure)|HTTPS]] 요청을 보낸다.

>[!tip]
> 만약 HTTPS를 사용할 경우, 이 단계 이전에 SSL/TLS 핸드셰이크도 수행한다. 이 과정에서는 브라우저와 서버가 암호화된 연결을 설정하기 위해 보안 인증서를 교환하고, 암호화 키를 협상한다.

### 4. 서버 응답 수신
서버는 요청을 받고, 해당 리소스(HTML, CSS, JavaScript, 이미지 등)를 브라우저에게 응답한다. (with. [[HTTP 상태 코드]]: 200 OK)

### 5. 브라우저 렌더링

받은 리소스들을 바탕으로 [[브라우저 렌더링 파이프라인]]을 진핸한다. DOM과 CSSOM을 생성하고, 렌더 트리를 구성한 뒤, 레이아웃과 페인트 단계를 통해 웹 페이지가 화면에 표시된다.