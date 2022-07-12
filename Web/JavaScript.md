### JavaScript
#### 변수와 연산자
- 변수명에는 문자, 숫자, 기호 $, _만 들어갈 수 있고 첫 글자는 숫자 불가능
- 대소문자 구별함
- ` 역따옴표로 변수나 표현식을 ${}안에 넣어서 표현식을 문자열 중간에 넣기 가능
- let vs var
    - var은 똑같은 이름으로 여러번 선언할 수 있음  
    - var과 달리 let은 선언문 이전에 사용하면 에러 발생(호이스팅 문제, var 선언문이나 function 선언문을 스코프의 선두로 옮긴 것처럼 동작하는 특성)
- 연산 순서
    - not -> and -> or 순서로 처리
    ```javascript
    const value = !((true && false)||((true&&false)||!false));
    !((true && false)||(true&&false)||true);
    !(false||false||true);
    !true
    false
    ```
- 비교 연산자
    - === 사용
    - == 으로도 비교는 가능 but 타입 비교는 안함
    - 1 == '1' 가 true(값을 숫자형으로 바꿔서 비교)
    - undefined == null이 true
- '+' 연산
    - 이항 연산자 +를 사용할 때 피연산자 중 하나가 문자열이면 다른 하나도 문자열로 변환됨
    ```javascript
    alert('1'+2); //"12"
    alert(2+2+'1');//"41"
    ```
    - 다른 산술 연산자의 경우 피연산자가 숫자형이 아닌 경우 숫자형으로 바꿔서 계산
    ```javascript
    alert(6-'2');//4
    alert('6'-'2');//3
    ```
    - 단항 연산자로도 사용 가능
    ```javascript
    let x = -1;
    alert(+x); //-1
    alert(+true); //1
    alert(+""); //0
    //숫자에는 영향이 없고, 숫자형이 아닌 피연산자는 숫자형으로 변환(Number(...)과 동일)
    ```
- ',' 연산
    - 표현식 각각이 모두 실행
    - 마지막 표현식의 결과만 반환됨
    ```javascript
    let a = (1+2, 3+4);
    alert(a); //7
    ```
- || 연산 
    - 첫 번째 true 값을 찾을 때까지 실행 
    - 모두 false로 평가되는 경우 마지막 값 반환
    ```javascript
    alert(1 || 0); //1
    alert(undefined || null || 0); //0(마지막 값 반환)
    ```
- && 연산
    - false값 발견 시 평가 중단 & 반환
    - 모든 피연산자가 true인 경우 마지막 피연산자 반환
    ```javascript
    alert(1 && 0); //0
    alert(null && 5); //null
    alert(1 && 5); //5  
- nullish 병합 연산자 ??
    - a ?? b
    - a가 null도 아니고 undefined도 아니면 a 반환 / 그 외의 경우는 b 반환

#### 함수
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
- indexOf
    - indexOf 함수로 원하는 항목이 몇번째 원소인지 찾을 수 있음
- findIndex
    - findIndex 함수로 객체나 배열 안에 존재하는 항목까지 찾을 수 있음
    - 조건을 반환하는 함수를 넣어서 검사
    ```javascript
    const index = todos.findIndex(todo=>todo.id===3);
    // todos에서 id가 3인 객체의 인덱스 반환
- find
    - findIndex 와 비슷하지만 인덱스 번호를 리턴해주는 것이 아니라 찾아낸 값 자체를 반환해줌
- filter
    - 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열 생성
    - 조건을 반환하는 함수를 넣어서 검사
    ```javascript
    const tasksNotDone = todos.filter(todo => todo.done === false);
    ```
- splice
    - 특정 항목 제거할 때 사용
    - 첫번째 파라미터는 어떤 인덱스부터 지울지, 두번째 파라미터는 몇 개를 지울지 의미
- slice
    - splice와 비슷하지만 기존의 배열을 변화시키지 않는다는 것이 다름
    - 첫번째 파라미터는 어디서부터 자를지, 두번째 파라미터는 어디까지 자를지 의미 -> 기존 배열은 그대로, 잘린 부분은 새롭게 반환
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

#### 비구조화 할당

#### Spread
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

#### rest
- 비구조화 할당 문법과 함께 사용
```javascript
const purpleCuteSlime = {
    name: '슬라임',
    attribute: 'cute',
    color: 'purple'
};
const {color, ...cuteSlime} = purpleCuteSlime;
```
- color에는 purple이, cuteSlime에는 {name:"슬라임", attribute:"cute"}라는 object가 저장
```javascript
const numbers = [0,1,2,3,4,5];
const [one, ...rest] = numbers;
```
- 배열에서도 마찬가지로 one에는 0이, rest에는 [1,2,3,4,5] 배열이 저장
- 함수의 파라미터가 몇개인지 모를때 rest 파라미터를 사용하면 모든 파라미터의 정보를 받아오는 함수 만들기 가능

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

#### 비동기 처리
- Promise
```javascript
const myPromise = new Promise((resolve, reject)=>{
    //구현 내용
})
```

#### 상호작용
- alert : '모달 창(modal window)' -> 등장하는 페이지 외의 버튼을 누르거나 밖의 요소와 상호작용 불가능
- prompt 
```javascript
result = prompt(title, [default]);
//title : 사용자에게 보여줄 문자열, default 필드의 초깃값(선택값)
```
- 사용자는 원하는 값 입력 후 확인 | 취소 선택 가능
- 확인을 누르면 prompt 함수는 사용자 입력 문자 반환
- 취소를 누르면 null을 반환
- confirm
```javascript
result = confirm(question);
//question : 사용자에게 보여줄 문자열
```
- 사용자는 확인 | 취소 선택 가능
- 확인을 누르면 true 반환
- 취소를 누르면 false 반환

#### 함수 표현식 vs 함수 선언문
- 함수 선언문 : 함수는 주요 코드 흐름 중간에 독자적인 구문 형태로 존재
```javascript
function sum(a,b){
    return a+b;
}
```
- 함수 표현식 : 표현식이나 구문 구성 내부에 생성
```javascript
let sum = function(a,b){
    return a+b;
};
```
- 자바스크립트 엔진이 함수 생성 시기가 다름
    - 함수 표현식의 경우 실제 실행 흐름이 해당 함수에 도달했을 때 함수 생성(실행 흐름이 함수에 도달했을 때 함수 사용 가능)
    - 함수 선언문의 경우 스크립트 어디에 있든 사용 가능 -> 초기화 단계에서 함수 선언 방식으로 함수 생성


#### 객체
- {...} -> key(문자형) : value(모든 자료형) 쌍으로 존재
- const로 선언된 객체는 수정될 수 있음(객체 전체를 바꾸려고 할 때만 에러 발생) -> 프로퍼티 하나 변경 시 에러 발생x
- 프로퍼티 값 단축 구문을 사용하여 선언도 가능
```javascript
return {
    name: name,
    age: age,
};
//
return {
    name,
    age,
};// 같은 의미
```
- 객체 프로퍼티에는 예약어를 사용할 수 있음
- in 연산자를 통해 object 안에 해당 프로퍼티가 존재하는지 확인할 수 있음
- for ... in 반복문
```javascript
for(let key in user){
    alert(key);
    alert(user[key]);
    //모든 프로퍼티, 프로퍼티에 해당하는 value 출력
}
```
- 객체 정렬 방식
    - 정수 프로퍼티 : 변형 없이 정수 <-> 문자열 가능한 문자열(문자열의 변형 없이 사용 가능 ex. "49")
    - 정수 프로퍼티의 경우 자동으로 정렬, 이외의 문자형은 객체에 추가한 순서대로 정렬
    - 정수 프로퍼티를 사용하고 싶은 경우 "+49" 방식으로 작성하면 이는 정수 프로퍼티에 해당되지 않기 때문에 입력한대로 사용 가능
- 객체의 참조와 복사
    - 객체는 참조에 의해 저장되고 복사됨
    - 참조하고 있는 객체가 다르면 내용이 프로퍼티와 값이 동일해도 == 연산에서 false가 출력됨
    - 객체를 복사하고 싶다면 for...in 반복문을 돌거나 Object.assign 사용 가능
- 가비지 컬렉션
    - 도달 가능성이 있는 값은 메모리에서 삭제되지 않음
    - root 값에서 도달 가능한 값만 메모리에 보존
        - 현재 함수의 지역 변수와 매개변수
        - 중첩 함수의 체인에 있는 함수에서 사용되는 변수와 매개변수
        - 전역 변수
- this
    - this의 값은 런타임에 결정됨
    - 함수를 선언할 때 this를 사용할 수 있지만 함수가 호출되기 전까지 this엔 값 할당 x
- 옵셔널 체이닝
    - ?. (연산자는 아니고 문법 구조체)을 사용하여 프로퍼티가 없는 중첩 객체를 에러 없이 안전하게 접근 가능
    - 기존에는 && 연산을 통해 정보가 없는 경우 undefined 에러 발생 여부로 판단
    - 옵셔널 체이닝을 사용하면 '앞'의 대상이 undefined나 null이면 평가를 멈추고 undefined 반환
    ```javascript
    let user = {};
    alert(user?.address?.street); //undefined, 에러 x
    alert(user?.address.street); //undefined, 에러 x
    ```
    - user가 null이나 undefined가 아니고 실제 값이 존재하는 경우엔 user.address 프로퍼티가 존재해야함
    - 그렇지 않으면 user?.address.street의 두 번째 점 연산자에서 에러 발생
    - 왼쪽 평가대상에 값이 없으면 즉시 평가를 멈춤(오른쪽에 있는 부가동작 발생 x)
    ```javascript
    let user1 = {
        firstName: "Violet"
    };
    let user2 = null;
    let key = "firstName";
    alert ( user1?.[key] ); //Violet
    alert ( user2?.[key] ); //undefined
- 심볼
    - 심볼을 통해 유일한 식별자를 만들 수 있음
    ```javascript
    let id1 = Symbol("id");
    let id2 = Symbol("id");
    alert(id1 == id2);//false
    ```
    - 심볼은 유일성 보장 -> 제3의 스크립트에서 만든 식별자와 충돌하지 않음
    - 객체의 경우 대괄호를 사용하여 심볼형 키를 만들어야함
    ```javascript
    let id = Symbol("id");
    let user = {
        name: "John",
        [id]: 123 // "id": 123으로는 불가능
    };
    ```
    - 심볼은 for..in 반복문에서 배제됨
    - Object.assign의 경우 심볼 프로퍼티까지 복사
    - Symbol.for("input")을 사용하면 전역 심볼로 선언할 수 있음
    ```javascript
    let id = Symbol.for("id");
    let idAgain = Symbol.for("id");
    alert(id===idAgain); //true
    ```
    - 전역 심볼의 경우는 Symbol.keyFor(sym)을 사용하면 이름을 얻을 수 있음


#### 자료구조와 자료형
- 숫자형
    - e : 10의 거듭제곱 표현
    - 0x : 16진수 표현
    - 0b : 2진수 표현
    - 0o : 8진수 표현
    - num.toString(base) : base 진법으로 num을 표현한 후 문자형으로 변환
    ```javascript
    1e3 // 1 * 1000;
    1.23e6 // 1.23 * 1000000
    0xff // 255
    0b11111111 // 255
    0o377 // 255
    let num = 255;
    num.toString(16) // ff
    num.toString(2) // 11111111
    parseInt('0xff',16); //255
    parseInt('ff',16); //255
    ```
- 문자열
    - 줄바꿈 기호 '\n',"\n" 대신에 ` `안에 직접 줄 바꿈을 넣어서 줄 바꿈 표현 가능
    -  