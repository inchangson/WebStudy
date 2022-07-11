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
// const squared = arr.map(n => n * n); 가능
```
    - map 함수의 파라미터에 변화를 주는 함수 전달
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

#### 