### 브라우저 동작 방식
#### HTTP 요청 흐름
1. 브라우저 주소창에 도메인 url 입력
- URL(Uniform Resource Locator) : 인터넷에서의 자원의 위치
    - https:// 통신에 사용된 프로토콜
    - mydomain.com 서버의 도메인
    - /test 요청 path
2. 브라우저가 url의 ip주소를 찾기 위해 캐시에서 DNS 기록 확인
    1. DNS(Domain Name Server) : url과 ip주소 매핑
    2. DNS 쿼리는 브라우저 캐시 확인(이전에 방문한 웹 사이트의 DNS 기록을 일정 기간 저장함
    3. 레코드가 없다면 OS에 시스템 호출을 통해 OS의 DNS 레코드 캐시에 존재하는지 화인
    4. OS에도 레코드가 없다면 라우터에서 DNS 기록을 저장한 캐시 확인
    5. 모든 단계에서 DNS기록을 찾지 못한 경우 ISP(Internet Service Provider)의 DNS 서버 확인
    6. 위와 같은 캐싱 정보를 통해 네트워크 트래픽 규제 & 데이터 전송 시간 개선
    7. ISP의 DNS 서버를 DNS Recursor라고 함 / 다른 DNS 서버는 Name Server라고 함
    8. => maps.google.com을 입력하는 경우 -> DNS 리커서가 루트 네임 서버에 연결 -> 루트 네임 서버는 리커서를 .com 도메인 네임 서버로 리디렉션 -> .com 네임 서버는 google.com 네임 서버로 리디렉션 -> google.com 네임 서버는 DNS 기록에서 maps.google.com과 일치하는 IP 주소를 찾아 DNS 리커서로 변환 & 브라우저로 보냄
3. 인터넷 프로토콜(Internet Protocol)을 사용하여 연결 구축
- HTTP 요청의 경우 TCP(Transmission Control Protocol) 전송 제어 프로토콜 사용
- IP은 송신 호스트와 수신 호스트가 패킷 교환 네트워크에서 정보를 주고받는 데 사용하는 정보 위주의 규약 & OSI 네트워크 계층에서 호소트의 주소 지정과 패킷 분할 조립 기능 담당
4. 브라우저가 웹 서버에 HTTP 요청 보냄
5. 서버가 요청 처리 & HTTP 응답 보냄
- Status Code
    1. 1xx -> 정보 메시지만 나타냄(요청 받음)
    2. 2xx -> 서버와의 요청 성공
    3. 3xx -> 요청 완료를 위해 추가 작업 조치 필요 / 요청 리소스 URI 변경
    4. 4xx -> 클라이언트의 request에 에러가 있음
    5. 5xx -> 서버측 오류, request 수행 불가
6. 브라우저가 HTML 컨텐츠 제공

#### CORS(Cross-Origin Resource Sharing) - 교차 출처 리소스 공유
- SOP(Same-Origin Policy)의 예외 조항 중 하나
- 추가 HTTP 헤더를 사용하여 한 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한 부여
- 최신 브라우저는 XMLHttpRequest | Fetch와 같은 API를 사용하는 경우 동일 출처의 리소스 | CORS 헤더를 포함한 응답 반환 필요
- 작동 방식
    - Preflight Request
        - 자바스크립트의 fetch API를 사용하여 브라우저에게 리소스를 받아오라는 명령을 내리면 브라우저는 서버에게 예비 요청을 보냄 -> 서버는 응답으로 허용 | 금지에 대한 정보를 보내줌
        - 서버가 보내준 응답 헤더에는 Access-Control-Allow-Origin이 포함됨(리소스 접근 가능한 출처)
        - Access Control Allow Origin과 비교를 통해 CORS 정책 위반 체크
    - Simple Request
        - 본 요청을 바로 서버에게 요청
        - 서버 응답의 헤더에 Access-Control-Allow-Origin과 같은 값을 보내주면 CORS 정책 위반 여부 검사
    - Credentialed Request
        - 인증된 요청 사용 방식
        - XMLHttpRequest 객체나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 요청에 담지 않음 
        - credentials 옵션을 통해 요청에 인증과 관련된 정보를 담을 수 있음
- 해결 방법
    - Access-Control-Allow-Origin 세팅
        - CORS 관련 설정을 위한 미들웨어 라이브러 사용
        - *를 사용하여 모든 출처 요청 받을 수 있음(보안 문제 발생)
    - Webpack Dev Server로 리버스 프록싱
        - 라이브러리를 이용해서 ‘target : https://api.evan.com' & ‘/api’ 방식으로 설정 가능
        - 위의 경우 /api로 시작하는 URL로 보내는 요청에 대해 브라우저는 https://api.evan.com으로 요청을 프록싱 -> CORS 정책 우회 가능


#### HTTP(HyperText Transfer Protocol)
- 의미
    1. 웹 서버 <-> 클라이언트 문서 교환 통신 규약
    2. 웹에서 사용하는 프로토콜 & TCP/IP 기반 서버 클라이언트 요청 응답 전송
- 특징
    1. 비연결성 지향(vs 소켓 통신은 양방향 연결, 스트리밍이나 실시간 채팅에 사용)
        1. 브라우저를 통해 사용자가 요청 -> 서버 접속 & 응답 데이터 전송 -> 연결 종료
        2. 간단 & 자원 적게 소모
        3. 연결 종료 후 추가 요청 대상을 알 수 없음 -> 쿠키, 세션, 히든 폼 필드로 해결 필요
    2. 단방향성 : 사용자의 요청에 서버가 응답
    3. TCP/IP 이용
- 동작
    1. 클라이언트가 브라우저를 통해 URI로 요청을 보내면 서버는 요청 처리 & 클라이언트 응답
    2. URI : 특정 리소스를 식별하는 통합 자원 식별자
    3. URL은 URI의 서브셋 / URL은 위치, URI는 식별
- HTTP Method
    1. GET : URI가 가진 정보 검색용 요청
    2. HEAD : GET과 동일 방식, BODY없음
    3. POST : 요청 자원 생성
    4. PATCH : 요청 자원의 일부를 수정
    5. PUT : 요청 자원 수정
    6. CONNECT : 요청한 자원에 대한 양방향 연결, 클라이언트가 HTTP 프록시 서버에 요청(클라이언트와 서버 사이에서 데이터를 전달해 주는 서버)
- 문제점
    1. 평문 통신 -> 도청 가능
    2. 통신 상대 확인 불가능 -> 위장 가능
    3. 완전성 증명 불가능 -> 변조 가능


#### HTTPS
- HTTP의 문제점 해결
- 소켓 부분을 인터넷 상에서 정보 암호화(SSL(Secure Socket Layer) 프로토콜로 대체 : 클라이언트 <-> 웹서버 데이터 암호화
    1. Layer 추가
    2. 대칭키 암호화 방식 & 공개키 암호화 방식 사용


#### TCP(Transmisson Control Protocol)
- 특징
    1. 일반적으로 TCP와 IP를 함께 사용, IP가 데이터 배달 처리, TCP는 패킷 추적 및 관리
    2. 신뢰성 있는 데이터 전송 / 연결 지향형 프로토콜
    3. 3way handshake로 연결 / 4way handshake로 연결 해제
    4. 흐름 제어, 혼잡 제어, 오류 제어로 신뢰성 보장
    5. 전송 순서 보장, 수신 여부 확인 가능
    6. 패킷 전송에서 논리적 경로 배정 / 수신측에선 패킷 재조립
    7. HTTP통신 / 이메일 / 파일 전송 등에 사용
- 흐름 제어
    1. 송신측과 수신측 사이의 데이터 처리 속도 차이 해결 기법
    2. 송신측과 수신측의 TCP버퍼 크기 차이로 인해 생기는 데이터 처리 문제 해결
        1. Stop & Wait
            - 전송한 패킷에 대한 응답(ACK)를 받으면 다음 패킷 전송(비효율적)
        2. Sliding Window
            1. 수신측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답(ACK) 없이 패킷을 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법
            2. 최초의 윈도우 크기는 호스트들의 3way handshaking을 통해 수신측 윈도우 크기로 설정 -> 이후 수신축의 버퍼에 남아있는 공간에 따라 변경
            3. 윈도우 크기는 수신측에서 송신측으로 확인(ACK)를 보낼 때 헤더에 담아 보냄
            4. 윈도우에 포함된 패킷 전송 & 수신측으로부터 확인 응답(ACK)가 오면 윈도우를 옆으로 옮겨 다음 패킷 전송


#### 웹 렌더링 엔진 동작 과정
1. 렌더링 엔진은 서버로부터 응답받은 HTML 문서를 받아옴
2. 렌더링 엔진은 HTML 문서를 파싱 & DOM 트리 구축
3. CSS 파일과 함께 포함된 스타일 요소를 파싱(CSSOM)
4. DOM 트리와 CSSOM의 결과물을 합쳐 렌더 트리 구축
5. 렌더 트리 각 노드에 대해 화면 상에서 배치할 곳 결정
6. UI 백엔드에서 렌더 트리의 각 노드 그림