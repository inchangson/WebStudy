### CORS(Cross-Origin Resource Sharing) - 교차 출처 리소스 공유
- SOP(Same-Origin Policy)의 예외 조항 중 하나
    - 보안 상의 이유로 브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청 제한
    - 웹 애플리케이션은 자신의 출처와 동일한 리소스만 호출 가능
    - 두 URL의 프로토콜, 호스트, 포트(명시한 경우)가 모두 같아야 동일 출처라고 판단 & 두 URL 간 상호작용
- 추가 HTTP 헤더를 사용하여 한 출처에서 실행중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한 부여
- 최신 브라우저는 XMLHttpRequest, Fetch와 같은 API를 사용하는 경우 동일 출처의 리소스 | CORS 헤더를 포함한 응답 반환 필요
    - XMLHttpRequest : 서버와 상호작용을 위해 사용, 페이지의 일부만 업데이트하는 Ajax에서 브라우저<->서버 데이터 교환에서 주로 사용
    - Fetch API : 요청, 응답과 같은 HTTP 파이프 라인에 엑세스, 조작 / XMLHttpRequest에 비해 비동기 요청 수월(Promise 기반, 콜백지옥 탈출)
- CORS 에러 해결 방법
    - Proxy Server 이용(클라이언트)
        - Proxy Server는 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 시스템
        - 프록시 설정을 통해 클라이언트에서 특정 요청만 특정 포트를 사용하게 만들어 서버에 접근 권한 부여
    - Express의 cors middleware 사용(서버)
        - express 개발환경에서 cors를 설치하여 특정 출처 요청 허용 가능
- 작동 방식
    - Preflight Request
        - 먼저 OPTIONS method를 통해 다른 도메인 리소스로 HTTP 요청을 보내 요청 전송이 안전한지 확인 & cross-origin 요청 하는 방법
        1. 자바스크립트의 fetch API를 사용하여 브라우저에게 리소스를 받아오라는 명령을 내림
        2. 브라우저는 서버에게 예비 요청(OPTIONS method)을 보냄
        3. 서버는 응답으로 허용 | 금지에 대한 정보, Access-Control-Allow-Origin 정보를 보내줌 
        4. Access Control Allow Origin과 비교를 통해 CORS 정책 위반 체크 & 요청 전송
    - Simple Request
        1. 본 요청을 바로 서버에게 요청
        2. 서버 응답의 헤더에 Access-Control-Allow-Origin과 같은 값을 보내주면 CORS 정책 위반 여부 검사
        - 까다로운 조건들은 만족시키는 경우에만 가능
            - 요청의 메소드가 GET, HEAD, POST인 경우 
            - Fetch 명세에서 "CORS-safelisted request-header"로 정의한 헤더인 경우 
            - Content-Type & (application/x-www-form-urlencoded / multipart/form-data / text/plain)
    - Credentialed Request
        - XMLHttpRequest 객체나 fetch API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 요청에 담지 않음 
        - credentials 옵션을 통해 인증된 요청 사용 방식, 인증정보 포함 요청 가능
        - CORS 실행 전 요청에는 자격 증명이 포함되지 않아야함 & 실행 전 요청에 대한 응답은 Access-Control-Allow-Credentials: true 를 지정하여 자격 증명으로 실제 요청 수행을 나타내야함
            - 프론트에선 withCredentials: true 옵션 추가
            - 서버에선 Access-Control-Allow-Credentials: true 옵션 추가
            -> http request header에 쿠키 정보 추가 & 받아오기
        - credentials 옵션을 통해 요청에 인증과 관련된 정보를 담을 수 있음(HTTP cookies, HTTP Authentication 정보)


- HTTP 응답 헤더
    - Access-Control-Allow-Origin 
        - 서버가 단일 출처를 지정하여 해당 출처가 리소스에 접근하도록 허용
        - CORS 관련 설정을 위한 미들웨어 라이브러 사용
        - *를 사용하여 모든 출처 요청 받을 수 있음(보안 문제 발생)
    - Access-Control-Expose-Headers
        - 브라우저가 접근할 수 있는 헤더를 서버의 화이트리스트에 추가
        - 서바가 응답 헤더로 보여질지 설정
    - Access-Control-Max-Age
        - preflight request 요청 결과를 캐시할 수 있는 시간 나타냄
    - Access-Control-Allow-Credentials
        - 플래그가 true일 때 요청에 대한 응답을 표시할 수 있는지 나타냄
    - Access-Control-Allow-Methods
        - 리소스에 접근할 때 허용되는 메소드 지정
    - Access-Control-Allow-Headers
        - preflight request에 대한 응답 -> 실제 요청시 사용할 수 있는 HTTP 헤더를 나타냄
- HTTP 요청 헤더
    - cross-origin 공유 기능을 사용하기 위해 클라이언트가 HTTP 요청을 발행할 때 사용할 수 있는 헤더
    - origin : cross-site 접근 요청 | preflight request 출처 | origin 요청이 시작된 서버를 나타내는 URI
    - Access-Control-Request-Method : 어떤 HTTP 메소드를 사용할지 서버에게 알려주기 위해 preflight request할 때 사용
    - Access-Control-Request-Headers : 실제 요청에서 어떤 HTTP 헤더를 사용할지 서버에게 알려주기 위해 preflight request할 때 사용


*****
#### 참고
https://developer.mozilla.org/ko/docs/Web/HTTP/CORS
https://evan-moon.github.io/2020/05/21/about-cors/
https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏
https://willseungh0.tistory.com/170
https://velog.io/@hustle-dev/JavaScript-CORS