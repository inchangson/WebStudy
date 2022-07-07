### Javasciprt 동작 원리
- JavaScript 엔진
    - 자바스크립트 코드를 실행하는 프로그램 | 인터프리터
        - 컴파일러 : 프로그램 전체 스캔 & 기계어로 번역(초기 오랜 시간 소비, 전체 실행 시간은 빠름 - 실행파일 제작)
        - 인터프리터 : 한 번에 한 문장씩 번역
    - 표준적인 인터프리터 | 바이트 코드로 컴파일하는 just-in-time 컴파일러로 구현
        - Just-in-time 컴파일(JIT) -> 필요할 때 필요한 만큼만
            - 프로그램을 실행하기 전에 처음 컴파일 & 프로그램 실행 시점에 필요한 부분을 즉석으로 컴파일
            - 처음 실행될 때 인터프리트 & 자주 쓰이는 코드 캐싱
            - 초기 구동 시에는 소스 코드를 실행 단계에서 컴파일하는 데 시간과 메모리 소비 -> 정적 컴파일 프로그램에 비해 실행속도 손해
- JavaScript 엔진 구성 요소
    - Memory Heap : 메모리 할당 발생 장소
    - Call Stack : 호출 스택이 쌓이는 장소
- V8 
    - 구글이 주도하여 C++로 작성한 JavaScript의 대표적인 엔진
    - 인터프리터 대신 자바스크립트 코드를 효율적인 머신 코드로 번역 & just-in-time 컴파일러
    - 작동 방식
        1. 자바스크립트 소스코드를 parser에 넘김
        2. parser는 파싱을 통해 AST(Abstract Syntax Tree)로 변환
        3. AST를 인터프리터를 통해 byte code로 변환(V8의 경우 Ignition이라는 인터프리터 사용)
        4. byte code 실행 & 작동
        5. 자주 사용되는 코드는 TurboFan(최적화 컴파일러)으로 보내짐
            - 최적화 방법
                - Hidden Class : 비슷한 것들끼리 분류 / 사용
                - Inline Caching : 필드 오프셋을 캐싱하여 사용
        6. TurboFan은 코드를 Optimized Machine Code로 컴파일하고 사용
            + 최적화 가정이 틀린 경우 deoptimize 진행 & 인터프리터로 돌아감
    - 사용 엔진
        - 풀코드젠 : 간단하고 매우 빠른 컴파일러, 단순하고 상대적으로 느린 머신 코드 생산
            - 처음 코드를 실행할 때 풀코드젠을 이용해서 파싱된 자바스크립트 코드를 변형없이 직접 머신 코드로 번역
        - 크랭크샤프트 : 복잡한 just-in-time 최적화 컴파일러, 고도로 최적화된 코드 생산
    - 여러 개의 쓰레드(단일 호출 스택 사용 -> 단일 쓰레드 기반 언어)
        - 구동되는 환경에서는 주로 여러 개 쓰레드 사용
        - 메인 쓰레드는 코드를 가져와서 컴파일 & 실행
        - 컴파일을 위한 별도 쓰레드 -> 코드 최적화를 하는 동안 메인 쓰레드는 코드 실행
        - 프로파일러 쓰레드 -> 사용자가 어떤 메소드에서 많은 시간을 보내는지 런타임에 알려줌 -> 크랭크샤프트가 최적화
        - 가비지 컬렉터 스윕 처리용 쓰레드
    - Event Loop
        - 비동기 방식으로 동시성 지원
        - 자바스크립트 엔진에서 제공되는 것이 아니라 브라우저나 nodeJS에서 지원
        - 실제 구동 환경에선 여러 개의 쓰레드 사용 -> 단일 호출 스택을 사용하는 자바 스크립트 엔진과 상호 연동을 위해선 '이벤트 루프' 사용 필요
        - 현재 실행중인 이벤트가 없는지 / 콜백 큐에 이벤트가 있는지 반복적으로 확인
    <!-- - 자바스크립트 객체 모델  -->
    - 콜백 큐
        - 버튼 클릭 / DOM 이벤트 / http 요청 / setTimeout 같은 비동기 함수 -> Web API 호출 & 콜백 함수를 Callback Queue에 넣음
        - 콜 스택이 비는 시점에 이벤트 루프를 돌려 해당 콜백 함수를 스택에 넣음
        - 이벤트 발생 시 메세지 추가, 이벤트 리스너 첨부 / 호출 스택의 초기 프레임으로 사용
    <!-- ... -->
    
    
    <!-- 자바스크립트 코드를 처음 수행할 때 V8은 풀코드젠을 이용하여 파싱된 자바스크립트 코드를 변형 없이 직접 머신 코드로 번역한다. 이를 통해 머신 코드의 실행을 매우 빠르게 시작할 수 있다. V8은 이와 같이 중간 바이트코드를 이용하지 않기 때문에 인터프리터가 필요가 없다. 코드가 얼마간 수행된 다음 프로파일러 쓰레드는 충분한 데이터를 얻게 되고 어떤 메서드를 최적화할 지 알 수 있게 된다. 그러면 크랭크샤프트가 다른 쓰레드에서 최적화를 시작한다. 크랭크샤프트는 자바스크립트의 추상구문트리를 고수준 정적단일할당(static single-assignment, SSA)으로 번역하는데 이를 하이드로젠(Hydrogen)이라고 한다. 대부분의 최적화는 이 수준에서 이루어진다. -->


*****
#### 참고
https://velog.io/@namezin/javascript-동작-원리
https://devowen.com/398?category=778540
https://github.com/baeharam/Must-Know-About-Frontend/blob/main/Notes/frontend/engine.md