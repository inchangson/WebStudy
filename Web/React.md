React Hook

    1. Class를 이용한 코드 작성 필요 없이, state & 기능들 사용할 수 있도록 만든 라이브러리
    2. 함수 컴포넌트를 클래스 컴포넌트처럼 사용 가능
    3. useSate, useContext, useReducer ...
    4. 함수 컴포넌트에서 React state & 생명주기 기능을 연동할 수 있게 해줌
1. UseState
    1. Const [state, state변경함수] = useState(기본 state값)
    2. 새로운 객체를 만들어서 객체에 변화를 줘야함(react의 불변성 때문에 input[name]=value로 직접 접근해도 리렌더링 되지 않음
2. useContext
    1. props를 글로벌하게 사용할 수 있게 도와줌
    2. 부모 & 자식 관계보다 복잡한 관계 -> context를 통해 조금 쉽게 사용가능
    3. 단일 export할 수 있는 변수 생성 & ContextProvider 생성
3. useEffect
    1. 리액트 컴포넌트가 랜더링 될 때마다 특ㄲ정 작업 실행
    2. 컴포넌트가 mount 됐을 때, unmount 됐을 때, update 됐을 때 작업 처리
    3. useEffect(function, deps) -> 수행하고자 하는 작업, 검사하고자 하는 특정 값 | 빈 배열
    4. componentDidMount + componentDidUpdate를 합친 형태
4. useHistory
    1. 
5. useRef
    1. 


React 관련


1. Super & props
    1. super 선언 전까지 construct 안에 this 키워드 사용 불가능
2. Axios
    1. 브라우저, NodeJS를 위한 HTTP 비동기 통신 라이브러리
    2. Get / Post 반환값은 Promise 객체
3. JSX에서는 {test.length>0 && <h1> asdf </h1>} 방식으로 조건부로 넣기 가능
4. ComponentDidMount는 DOM에 렌더링 된 후에 작동
5. this.state.asdf = ~~ 는 constructor에서 밖에 사용 불가능, setState사용 필요
6. NavLink to / path / query 
7. Concurrent -> 사용자의 입력을 보여주는 것이 우선순위가 더 높다고 판단 & 렌더링 인터럽트
8. let vs var
    1. var은 똑같이 var A = x; var A = y; 해도 에러 X
    2. let은 에러 발생
    3. var과 달리 let은 선언문 이전에 사용하면 에러 발생(호이스팅 문제 - var 선언문이나 function 선언문을 스코프의 선두로 옮긴 것처럼 동작하는 특성)
9. 리액트 기초
    1. class ~ extends React.Component 생성
    2. render() 메소드 & this.props 사용 가능
    3. constructor(props) & super를 통해 초기 this.state 지정 가능
10. Reducer
    1. 현재 상태와 액션 객체를 파라미터로 받아서 새로운 상태를 반환해주는 함수
    2. Const [state, dispatch] = useReducer(reducer, initialState)
    3. State는 컴포넌트에서 사용할 수 있는 상태, Dispatch는 액션을 발생시키는 함수
11. Redux
    1. 언제 어디서든 원하는 state 사용하기 위함
    2. 외부에 store를 두고 관리
12. 컨테이너 컴포넌트
    1. 내부에 DOM 엘리먼트가 직접적으로 사용 X
    2. 리덕스에 직접 접근 가능
13. 프레젠테이셔널 컴포넌트
    1. 뷰만 담당하는 컴포넌트
    2. DOM 엘리먼트, 스타일 가지고 있음
    3. 리덕스 스토어 접근 권한 X, props로만 데이터 가지고 올 수 있음
    4. 대부분 State를 가지고 있지 않음 / UI에 관련된 경우 가능
14. Bind
    1. this를 수정하게 해주는 내장 메소드 
    2. 함수처럼 호출 가능한 ‘특수 객체’ 반환 / this가 고정된 func 호출과 동일
15. Axios
    1. Fetch API -> body 프로퍼티 사용 & url이 함수 인자로 사용됨
    2. Axios -> data 프로퍼티 사용 & url이 option 객체로 사용됨
    3. HTTP 통신 요구사항을 컴팩트한 패키지로써 사용 가능
    4. 브라우저 <-> NodeJS를 위한 Promise API 활용 HTTP 비동기 통신 라이브러리
        1. GET : 데이터를 받아옴, 데이터를 가져와서 보여줌(값이나 상태 변화 X)
        2. POST : 새로운 리소스 생성 / 파일 업로드 (주소창에 쿼리스트링 남지 않음 -> GET보다 안전)
16. npm vs npx
    1. npx는 nodeJS 패키지를 실행시키는 도구
    2. 실행 과정에서 로컬에 저장되어 있는 경우 실행 / 존재하지 않는다면 npx가 가장 최신 버전을 설치하고 실행
17. Recoil
    1. 등장배경
        1. 컴포넌트의 상태는 공통된 상위요소까지 끌어올려야 공유 가능 & 이 과정에서 트리가 다시 렌더링 되어야함
        2. Context는 단일 값만 저장 가능
    2. 장점
        1. 공유상태도 React의 내부상태처럼 get/set 인터페이스로 사용 가능(boilerplate-free API)
    3. 주요개념
        1. Atoms
            1. 상태의 단위 / 업데이트 & 구독 가능

