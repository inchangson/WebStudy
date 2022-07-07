### 브라우저 동작 원리
#### 브라우저 컴포넌트
- 사용자 인터페이스(UI) : 요청한 페이지를 보여주는 창 외의 모든 UI / 주소창, 뒤로가기, 앞으로 가기, 북마크, 환경설정 ...
- 브라우저 엔진 : 사용자 인터페이스 <-> 렌더링 엔진 중개자 역할 / 새로고침 버튼 클릭 -> 새로 렌더링
- 렌더링 엔진 : HTML / CSS / JavaScript 파싱 & 결과물 페이지로 보여줌
- 네트워크 레이어 : HTTP, HTTPS 같은 프로토콜을 이용해 외부 리소스 받아옴 & 서버에 요청 보낼때 사용
- JavaScript Interpreter : JavaScript 해석 & 실행
- UI 백엔드 : 브라우저가 동작하고 있는 운영체제의 인터페이스를 따르는 UI 처리 / Alert나 Select 박스가 운영체제 별로 다르게 작동
- 자료 저장소 : 브라우저 자체에서 하드디스크와 같이 데이터를 로컬에 저장하기 위한 레이어 / 쿠키, 로컬 스토리지, 세션 스토리지, IndexedDB, 웹 SQL 등에 접근, 저장
    - 쿠키 : HTTP의 비상태성 해결 용도 / 클라이언트의 웹 브라우저가 지정하는 메모리, 하드디스크에 저장
    - 세션 : HTTP의 비상태성 해결 용도 / 서버의 메모리에 저장

<p align="center">![브라우저 컴포넌트](https://yozm.wishket.com/media/news/1338/image002.png "브라우저 컴포넌트")</p>
<br>


#### 렌더링 엔진의 동작 과정
- 흐름
    1. 렌더링 엔진은 서버로부터 응답받은 HTML 문서를 받아옴
    2. HTML 문서를 파싱 & DOM 트리 구축
    3. CSS 파일과 함께 포함된 스타일 요소를 파싱 -> CSSOM 트리 구축
    4. DOM 트리와 CSSOM의 결과물을 합쳐 렌더 트리 구축
    5. 렌더 트리 각 노드에 대해 화면 상에서 배치할 곳 결정(기하학적 형태 계산)
    6. UI 백엔드에서 렌더 트리의 각 노드 그림(개별 노드를 화면에 페인트)
- 웹 페이지 접속시 네트워크를 통해 HTML 문서 읽기 가능 -> 렌더링 엔진 해석(4단계, Critical Rendering Path)
    1. 파싱
        - 토큰화된 코드를 구조화하는 과정
        - 입력받은 문자열이 정해진 문법을 따르는지 확인하는 과정
        - 어휘(Vocabulary) + 문법 규칙(Syntax rule)로 구성
        - 브라우저는 HTML, CSS, JavaScript 언어 해석 가능(JavaScript는 Interpreter 따로 존재) -> 렌더링 엔진에서는 HTML, CSS 파싱
            - HTML 파싱
                - HTML 문자열을 이용해 Parse Tree 생성
                - Parse Tree는 HTML 코드를 트리 구조로 구조화한 것
                - 브라우저는 이 Parse Tree를 이용해 DOM(Document Object Model) 트리 새로 구축
                - Parse Tree는 단순히 토큰화된 문자열 구조화 / DOM 트리는 상호작용할 수 있는 HTML 엘리먼트로 이루어짐
                - HTML 파서 특징
                    - 자체적으로 에러 복구 시도
                        - ex) html 태그 없거나, 닫는 태그 없거나, 어트리뷰트 쌍따옴표 없이 사용 ... -> 교정 시도
                        - HTML Document Type Definition(DTD)에 의해 정의된 규칙에 따라 교정 시도
                    - 파싱 과정이 중단될 수 있음
                        - 파싱 도중 script, link 같은 외부 태그를 만나면 파싱 중단 & 해석 실행
                        - 태그가 외부 파일을 참조하는 경우 다운로드 & 해석 시작
                        - 외부 컨텐츠 해석 어려운 경우나 script가 DOM을 직접 수정하는 경우 존재 가능성 떄문에 파싱 과정을 중단하고 외부 컨텐츠 먼저 해석
                    - 재시작
                        - 외부 요인으로 DOM이 추가, 변경, 삭제되는 경우 처음부터 다시 파싱
                        - 바이트 -> 문자 -> 토큰 식별 -> 노드 변환 -> DOM 트리 빌드 과정을 다시 진행(많은 시간 소요)
                        <p align="center">![파싱 과정](https://yozm.wishket.com/media/news/1338/image004.png "파싱 과정")</p>
            - CSS 파싱
                - 공식적인 명세 존재 -> HTML에 비해 단순
                - CSS 링크 코드가 HTML 코드 내에 삽입 -> HTML 파싱 도중에 CSS 파싱
                - 전체 파일 다운로드가 완료되면 파싱 진행
                - 코드에서 명세한 내용과 순서를 바탕으로 DOM과 같은 트리 구성 -> CSSOM(CSS Object Model) 트리라고 함
                - 이 트리에 스타일, 규칙, 선택자 정보 
                <p align="center">![CSSOM](https://yozm.wishket.com/media/news/1338/image006.png "CSSOM")</p>
    2. 렌더 트리(프레임 트리) 구축
        - 화면에 나타나는 요소 결정
        - 어떤 요소, 어떤 스타일, 어떤 순서 명세
        - DOM 트리 + CSSOM 트리 조합으로 만들어짐
        - 시각적으로 보이지 않는 엘리먼트가 존재 -> DOM 트리와 1:1 매칭 구조는 아님
    3. 레이아웃 | 리플로우
        - 렌더 트리에서 계산되지 않았던 노드 크기, 위치, 레이어 순서 정보 계산 & 좌표에 나타냄
        - HTML의 루트 오브젝트로부터 재귀적으로 실행
        - 계산 범위에 따라 Global Layout, Incremental Layout으로 구분 가능
            - Global Layout
                - 화면 전체 레이아웃 계산
                - ex) 새로운 폰트 적용, 폰트 사이즈 변경, 뷰포트 사이즈 변경
                - 모든 렌더 트리 노드에 대해 기하학적인 계산 수행 -> 노드가 많을수록 속도 저하
                - 자체적인 최적화 로직 필요 
                -> 더티 비트 시스템(특정 엘리먼트 레이아웃 변경 -> 특정 부분만 다시 계산)
            - Incremental Layout
                - 더티 비트 시스템 활용
                - 레이아웃 렌더 트리를 재귀적으로 탐색 -> 더티한 엘리멘트(레이아웃 변경 엘리먼트) 발견 -> 계산을 즉시 실행X -> 스케줄러로 비동기 일괄 작업 진행(연산 횟수, 범위 감소)
            - **_DOM의 레이아웃과 관련된 값을 직접 읽어오거나 변화를 주는 JavaScript 코드 작성 시 구문을 묶는 것이 좋음_**
            ```JavaScript
            const divWidth = div1.clientWidth;
            div2.style.width = `${divWidth}px`;
            const divHeight = div1.clientHeight;
            div2.style.height = `${divHeight}px`;
            ```
            위의 코드는 div2 너비 변경 후 div1의 높이를 불러옴 -> 레이아웃 변경 발생 가능성 -> 불필요한 계산 추가
            ```JavaScript
            const divWidth = div1.clientWidth;
            const divHeight = div1. clientHeight;
            div2.style.width = `${divWidth}px`;
            div2.style.height = `${divHeight}px`;
            ```
    4. 페인트
        - 레이아웃 단계를 통해 화면에 배치된 엘리먼트에 색을 입히고 레이어 위치 결정
        - 루트 오브젝트로부터 재귀적으로 실행
        - 마찬가지로 Global Painting, Incremental Painting 존재
        - z-index 가 낮은 순서대로 먼저 페인팅
        - background-color -> background-image -> border -> children -> outline 순서대로 페인팅
- **_렌더링 단계 중 가장 비용이 많이 드는 단계는 레이아웃, 페인팅 단계 -> 두 연산 최소화 과정이 최적화 과정_**


### HTTP 요청 흐름
1. 브라우저 주소창에 도메인 url 입력
- URL(Uniform Resource Locator) : 인터넷에서의 자원의 위치
    - https://mydomain.com/test 입력 의미
        - https:// 통신에 사용된 프로토콜
        - mydomain.com 서버의 도메인
        - /test 요청 path
2. 브라우저는 url의 ip주소를 찾기 위해 캐시에서 DNS 기록 확인
    - DNS(Domain Name Server) : url과 ip주소 매핑
    1. DNS 쿼리는 브라우저 캐시 확인(이전에 방문한 웹 사이트의 DNS 기록을 일정 기간 저장)
    2. 레코드가 없다면 OS에 시스템 호출을 통해 OS의 DNS 레코드 캐시에 존재하는지 화인
    3. OS에도 레코드가 없다면 라우터에서 DNS 기록을 저장한 캐시 확인
    4. 모든 단계에서 DNS기록을 찾지 못한 경우 ISP(Internet Service Provider)의 DNS 서버 확인
    - 캐싱 정보를 통해 네트워크 트래픽 규제 & 데이터 전송 시간 개선
    - ISP의 DNS 서버를 DNS Recursor라고 함 / 다른 DNS 서버는 Name Server라고 함
    => maps.google.com을 입력하는 경우 -> DNS 리커서가 루트 네임 서버에 연결 -> 루트 네임 서버는 리커서를 .com 도메인 네임 서버로 리디렉션 -> .com 네임 서버는 google.com 네임 서버로 리디렉션 -> google.com 네임 서버는 DNS 기록에서 maps.google.com과 일치하는 IP 주소를 찾아 DNS 리커서로 변환 & 브라우저로 보냄
3. 인터넷 프로토콜(Internet Protocol)을 사용하여 연결 구축
    - HTTP 요청의 경우 TCP(Transmission Control Protocol) 전송 제어 프로토콜 사용
    - IP(Internet Protocol)은 송신 호스트와 수신 호스트가 패킷 교환 네트워크에서 정보를 주고받는 데 사용하는 정보 위주의 규약 & OSI 네트워크 계층에서 호소트의 주소 지정과 패킷 분할 조립 기능 담당
        - TCP(Transmisson Control Protocol) - 데이터 패킷 전송을 위한 연결
            1. 일반적으로 TCP와 IP를 함께 사용, IP가 데이터 배달 처리, TCP는 패킷 추적 및 관리
            2. 신뢰성 있는 데이터 전송 / 연결 지향형 프로토콜
            3. 3way handshake로 연결 / 4way handshake로 연결 해제
                - 3-way-handshake
                    1. 클라이언트는 서버에 SYN(synchronize : 연결 요청) 패킷을 보냄
                    2. 서버에 수락할 수 있는 열린 포트 존재 시, SYN/ACK(acknowledgement: 승인) 패킷을 보냄
                    3. 클라이언트는 SYN/ACK 패킷 수신 & ACK 패킷 전송하여 승인
                    <p align="center">![3 way handshake](https://www.techopedia.com/images/uploads/ad900dc1-ad94-4c7b-a3f8-154ad27c35f1.png "3 way handshake")</p>                  
            4. 흐름 제어, 혼잡 제어, 오류 제어로 신뢰성 보장
            5. 전송 순서 보장, 수신 여부 확인 가능
            6. 패킷 전송에서 논리적 경로 배정 / 수신측에선 패킷 재조립
            7. HTTP통신 / 이메일 / 파일 전송 등에 사용
4. 브라우저가 웹 서버에 HTTP 요청 보냄
5. 서버가 요청 처리 & HTTP 응답 보냄
- Status Code
    1. 1xx -> 정보 메시지만 나타냄(요청 받음)
    2. 2xx -> 서버와의 요청 성공
    3. 3xx -> 요청 완료를 위해 추가 작업 조치 필요 / 요청 리소스 URI 변경
    4. 4xx -> 클라이언트의 request에 에러가 있음
    5. 5xx -> 서버측 오류, request 수행 불가
6. 브라우저가 HTML 컨텐츠 제공


*****
#### 참고
https://yozm.wishket.com/magazine/detail/1338/ <br>
https://velog.io/@khy226/브라우저에-url을-입력하면-어떤일이-벌어질까 <br>
https://d2.naver.com/helloworld/59361