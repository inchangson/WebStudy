### JavaScript
#### 기본 문법
- let vs var
    - var은 똑같은 이름으로 여러번 선언할 수 있음(에러가 발생하지 않지만 관리하기 어려움)
    - var과 달리 let은 선언문 이전에 사용하면 에러 발생(호이스팅 문제, var 선언문이나 function 선언문을 스코프의 선두로 옮긴 것처럼 동작하는 특성)
- Hoisting
    - 선언되지 않은 함수, 변수를 끌어올려서 사용(초기화, 선언이 밑에 있어도 선언은 된것으로 실행 -> undefined 상태로 돌아감)
    - Hoisting이 발생하는 코드는 이해도 어렵고 유지보수가 어려움 -> 방지하는 것이 좋음
    - var 대신 const, let을 위주로 사용하는 것이 좋음(const와 let은 변수 생성과정이 달라서 엑세스 불가 에러 발생)
- 비교 연산자
    - === 사용
    - == 으로도 비교는 가능 but 타입 비교는 안함
    - 1 == '1' 가 true(값을 숫자형으로 바꿔서 비교)
    - undefined == null이 true
- 화살표 함수
```javascript
//
const add = (a, b) => a + b;
//
const add(a,b){
    return a+b;
}
```


#### 배열 내장함수
- forEach
```javascript
const arr = ['first', 'second', 'third'];
for(let i=0;i<arr.length;i++){
    console.log(arr[i]);
}
```
```javascript
const arr = ['fisrt','second','third'];
arr.forEach(tmp=>{
    console.log(tmp);
});
```
- map
```javascript
//기본
const arr = [1,2,3,4,5];
const squared = [];
for(let i=0;i<arr.length;i++){
    squared.push(arr[i]*arr[i]);
}
```
```javascript
//forEach 사용
const arr = [1,2,3,4,5];
const squared = [];
arr.forEach(n=>{
    squared.push(n*n);
});
```
```javascript
//map 사용
const arr[1,2,3,4,5];
const square = n => n * n;
const squared = arr.map(square);
// map 함수의 파라미터에 변화를 주는 함수 전달
// const squared = arr.map(n => n * n); 가능
```
- findIndex
    - findIndex 함수로 객체나 배열 안에 존재하는 항목까지 찾을 수 있음(첫번째 인덱스, 없으면 -1)
    - 조건을 반환하는 함수를 넣어서 검사
    ```javascript
    const index = todos.findIndex(todo=>todo.id===3);
    // todos에서 id가 3인 객체의 인덱스 반환
    ```
- filter
    - 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열 생성
    - 조건을 반환하는 함수를 넣어서 검사
    ```javascript
    const tasksNotDone = todos.filter(todo => todo.done === false);
    ```
- shift & pop
    - shift는 첫번째 원소 추출 & 삭제
    - pop은 마지막 원소 추출 & 삭제
- unshift
    - 배열의 맨 앞에 새 원소 추가
- concat
    - 여러개 배열 이어주기
    - 기존 배열에는 변화 X, 새롭게 반환
- join
    - 배열 값들을 문자열 형태로 합쳐줌
    - 파라미터로 문자를 넘겨주면 값들 사이에 특정 문자가 들어간 상태로 합쳐짐
    ```javascript
    var arr = ['abc', 'def', 'ghi'];
    var str = arr.join(';');
    alert(str); //abc;def;ghi
    ```
- reduce
    - 첫번째 파라미터는 accumulator과 current를 파라미터로 가져와서 결과를 반환하는 콜백함수
    - 두번째 파라미터는 reduce함수에서 사용할 초깃값
    ```javascript
    const numbers = [1,2,3,4,5];
    let sum = numbers.reduce((accumulator,current)=>{
        console.log({accumulator,current});
        return accumulator + current;
    },0);
    //0,1 -> 1,2 -> 3,3 -> 6,4 -> 10,5
    ```
- includes
    ```javascript
    const isAnimal = name => ['고양이','개','거북이','너구리'].includes(name);
    //true | false 반환
    ```


#### Spread 문법
```javascript
const slime = {
    name : '슬라임'
};
const cuteSlime{
    ...slime,
    attribute: 'cute'
};
//배열에서도 사용 가능
const animals = ['개', '고양이', '참새'];
const anotherAnimals = [...animals, '비둘기'];
```


#### rest 문법
- 비구조화 할당 문법과 함께 사용
```javascript
const purpleCuteSlime = {
    name: '슬라임',
    attribute: 'cute',
    color: 'purple'
};
const {color, ...cuteSlime} = purpleCuteSlime;
```
- color에는 'purple'이, cuteSlime에는 {name:"슬라임", attribute:"cute"}라는 object가 저장
```javascript
const numbers = [0,1,2,3,4,5];
const [one, ...rest] = numbers;
```
- 배열에서도 마찬가지로 one에는 0이, rest에는 [1,2,3,4,5] 배열이 저장
- 함수의 파라미터가 몇개인지 모를때 rest 파라미터를 사용하면 모든 파라미터의 정보를 받아오는 함수 만들기 가능
```javascript
function sum(...rest){
    return rest.reduce((acc,current)=>acc+current,0);
}
const result = sum(1,2,3,4,5,6);
```

#### 프로토타입과 클래스
- 프로토타입
    - 같은 객체 생성자 함수를 사용하는 경우, 특정 함수 또는 값을 재사용할 수 있음
    - 객체 생성자 함수 아래에 .prototype.[원하는키] = 를 통해 설정 가능
    ```javascript
    function Animal(type,name,sound){
        this.type = type;
        this.name = name;
        this.sound = sound;
        this.say = function(){
            console.log(this.sound);
        };
    }//객체가 수행하는 코드가 똑같아도 객체가 생성될때마다 함수가 새로 생성됨
    Animal.prototype.say = function(){
        console.log(this.sound);
    };//재사용
    ```
- 클래스
    - ES6에서는 class 문법 도입
    - 클래스 내부 함수를 '메서드' => 자동으로 prototype으로 등록
    - extends를 통해 상속
    - constructor로 초기화 가능
    - 
    ```javascript
    class Animal{
        constructor(type, name, sound){
            this.type = type;
            this.name = name;
            this.sound = sound;
        }
        say(){
            console.log(this.sound);
        }
    }
    class Dog extends Animal {
        constructor(name, sound) {
            super('개', name, sound);
        }
    }
    class Cat extends Animal {
        constructor(name, sound) {
            super('고양이', name, sound);
        }
    }
    const dog = new Animal('멍멍이', '멍멍');
    const cat = new Animal('야옹이', '야옹');
    dog.say(); //멍멍
    cat.say(); //야옹
    ```


#### this
- this의 값은 런타임에 결정됨
- 함수를 선언할 때 this를 사용할 수 있지만 함수가 호출되기 전까지 this엔 값 할당 x
- 동일한 함수라도 다른 객체에서 호출한 경우 'this'가 참조하는 값이 달라짐
```javascript
let user = {name: "John"};
let admin = {name: "Admin"};
function sayHi(){
    alert(this.name);
}
user.f = sayHi;
admin.f = sayHi;
user.f(); //John
admin.f(); //Admin
```


#### 이벤트 버블링
- 요소에 이벤트 발생 -> 할당된 핸들러 작동 -> 부모 요소의 핸들러 작동 -> 최상단 조상 요소까지 반복
```HTML
<form onclick="alert('form')">FORM
    <div onclick="alert('div')">DIV
        <p onclick="alert('p')">p</p>
    </div>
</form>
```
- 이벤트가 발생한 가장 안쪽 요소는 event.target, 현재 요소(실행 중인 핸들러가 할당된 요소)는 this
    - 클릭 이벤트를 예시로 들면, event.target은 실제 클릭한 요소
    - this는 작동한 핸들러 함수가 붙어있는 요소
- 이벤트 캡쳐
    - 이벤트 버블링과 반대 방향으로 진행되는 전파 방식(최상위 요소에서 해당 태그까지)
    ```javascript
    div.addEventListener('click', logEvent, {
        capture: true //default는 false
    });
    ```
- event.stopPropagation()을 사용하면 이벤트 처리 & 버블링 중단(부모 요소로 일어나는 버블링 막아줌)


#### Scope
- 전역(Global) : 코드의 모든 범위에서 사용 가능
- 함수(Function) : 함수 안에서만 사용 가능
- 블록(Block) : if, for, switch등 특정 블록 내부에서만 사용 가능
- 함수 내부에서 같은 이름으로 새롭게 변수를 선언해도 전역 변수의 값이 바뀌진 않음
- const와 let을 블록 내에서 선언하게 되면, 블록 내부에서만 사용 가능하고, 블록 밖에 같은 이름의 변수가 존재해도 영향을 끼치지 않음
- var은 function scope로 선언됨 -> 블록 내부에서 선언한 value가 블록 밖의 value에도 영향을 미침 / 전역은 X


#### Hoisting
- 선언되지 않은 함수, 변수를 끌어올려서 사용(초기화, 선언이 밑에 있어도 선언은 된것으로 실행 -> undefined 상태로 돌아감)
- Hoisting이 발생하는 코드는 이해도 어렵고 유지보수가 어려움 -> 방지하는 것이 좋음
- var 대신 const, let을 위주로 사용하는 것이 좋음(const와 let은 변수 생성과정이 달라서 엑세스 불가 에러 발생)


#### 변수 유효범위와 클로저
- 렉시컬 스코핑(Lexical scoping) : 스코프는 함수를 호출할 때가 아니라 함수를 어디에 선언하였는지에 따라 결정됨
    - inner함수가 outer함수 내부에 선언된 경우(outer함수는 전역에 선언) : inner함수의 렉시컬 스코프는 전역, outer, 본인까지 참조 가능
    - 스코프 체인 과정 : 함수 스코프에서 검색 -> 없으면 외부 함수 스코프 검색
```javascript
function outerFunc() {
    var x = 10;
    var innerFunc = function () { console.log(x); };
    return innerFunc;
}

var inner = outerFunc();
inner();
```
- outerFunc()가 innerFunc() 반환 후 사라짐 but, 코드 실행 시 10이 출력됨
- 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저라고 함
- 클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경(Lexical environment)인 스코프를 기억하여 자신이 선언됐을 때의 환경 밖에서 호출되어도 그 환경에 접근할 수 있는 함수
- 현재 상태를 기억하고 변경된 최신 상태를 유지할 때 자주 사용(ex. 버튼 토글 상태 변경, 카운터 - 함수를 리턴하는 함수 안에 변수 선언)