### TypeScript
#### 정적 vs 동적 언어
- 정적 언어
    - 자료형, 타입을 컴파일에 결정하는 것
    - 변수에 들어갈 값의 형태에 따라 자료형 지정 필요 / 자료형이 맞지 않으면 컴파일 에러
    - 정보가 미리 결정되기 때문에 속도가 빠르고, 타입 에러 발생을 초기에 발견(안정성 높음)
    - 매번 타입을 결정해야하기 때문에 번거로움 + 코드량 증가 + 컴파일은 더 오래걸림
    - C, C#, C++, Java 등
- 동적 언어
    - 컴파일 시 자료형이 결정되는 것이 아니라 런타임에 자료형 결정
    - 타입 없이 변수만 선언하여 값 지정 가능
    - JavaScript, Ruby, Python 등

#### 타입 안정성
- 자바 스크립트는 에러를 최대한 줄여주고자 함
```javascript
[1,2,3,4] + false 
'1,2,3,4false' //string으로 변환
```
```javascript
function divide(a, b){
    return a/b
}
divide("aaa"); //NaN 반환
```
- 객채에 존재하지 않는 메소드 사용 -> 코드 실행 -> 런타임 에러 발생
- 코드가 실행되기 전에 에러를 캐치할 수 있어야함
- 타입스크립트는 에러가 발생할 것 같은 부분이 존재하면 자바스크립트로 컴파일되지 않음
    

#### 타입
```TypeScript
let isLogin: boolean = false;
let num: number = 10;
let str: String = 'test';
let obj: object = { name: 'test', age: 1 }
// object의 경우 모든 타입 값 할당 가능 -> 타입 검사에 엄격한 타입스크립트 목적이 모호해짐
let arr: number[] = [1,2,3];
let arr: readonly number[] = [1,2,3];
// let arr = [1,2 3]; 명시해주지 않아도 가능
let arr: Array<number> = [1,2,3];
```

- tuple
```TypeScript
let arr: [string, number] = ['hi', 10];
// ex) 사용자 정보 저장 [index, id, password] -> 특정 길이 & 데이터 타입 순서 정해짐
// readonly 옵션 사용 가능
// index를 통해 변경 가능
```

- Enum
```TypeScript
enum Team { Manager , Planner = 100, Designer }
// Team.Manager -> 0, Team.Planner -> 100, Team.Designer -> 101
// Team[100] -> 'Planner'
```

- any
```TypeScript
let str: any = 'hi';
any = 1;
// 모든 타입에 대해 허용
```

- unknown
    - 변수의 타입을 미리 알지 못 할 때 사용
```TypeScript
let a:unknown

let b = a + 1 // 에러 발생
if(typeof a === 'number'){
    let b = a + 1 
}//에러 발생 x
```

- void
```TypeScript
let unuseful: void = undefined;
function test(): void{
    // 리턴값 설정 불가능
}
// 변수에는 undefined나 null만 할당 가능 / 함수에는 반환값 설정 불가능
```

- never
```TypeScript
function func(): never{
    while(true){

    }
}
// 함수의 끝에 절대 도달하지 않는다는 의미
```

- union
```TypeScript
function setInfo(id: number | String, name: String){
    return { id, name };
}
// 매개 변수에 number, string 모두 가능하게 파이프를 사용하여 설정 가능
```


#### 인터페이스
- 상호 간에 정의한 약속, 규칙
    - 객체의 속성, 속성 타입
    - 함수 파라미터, 반환 타입
    - 배열과 객체에 접근하는 방식
    - 클래스
```TypeScript
interface CraftBeer {
  name: string;
  age?: number;  // 옵션 속성
}

let myBeer = {
  name: 'Saporo'
};
function brewBeer(beer: CraftBeer) {
  console.log(beer.name); 
}
brewBeer(myBeer); // Saporo

if(myBeer.age<10) // 에러 발생 undefined 가능성
if(myBeer.age && myBeer.age<10) //가능
```
```TypeScript
// 함수 타입도 지정 가능(call signature) - 타입 지정과 함수 선언을 분리할 수 있음
interface login {
    // 함수의 파라미터 & 리턴 타입 지정
  (username: string, password: string): boolean;
}
let loginUser: login;
loginUser = function(id: string, pw: string){
    // 함수 선언
    console.log("test");
    return true;
}
```

#### Interface vs Type
```TypeScript
interface PeopleInterface {
  name: string
  age: number
}
```
```TypeScript
type PeopleType = {
    name: string
    age: number
}
```
- 확장 방법
```TypeScript
interface StudentInterface extends PeopleInterface {
    school: string
}
```
```TypeScript
type StudentType = PeopleType & {
    school: string
}
```

#### 오버로딩
- 여러개의 call signature가 있는 함수
```TypeScript
// 파라미터 타입의 종류가 다른 경우
type Add = {
    (a: number, b: number) : number
    (a: number, b: string) : number
}
const add: Add = (a,b) => {
    if(typeof b === "string") return a
    return a + b
}
```
```TypeScript
// 파라미터 개수가 다른 경우
type Add = {
    (a: number, b: number): number
    (a; number, b: number, c: number): number
}
const add:Add = (a, b, c?:number) => {
    if(c) return a + b + c
    return a + b
}
```


#### Intersection Type & Union Type
```TypeScript
interface Person {
  name: string;
  age: number;
}
interface Developer {
  name: string;
  skill: number;
}
type Capt = Person & Developer;
// { name: string, age: number, skill: string; }
// intersection을 통해 여러 타입 정의를 하나로 합칠 수 있음
function func(inp: Person | Developer)
// inp.name 만 접근가능
// inp.age, inp.skill 접근 불가능
// union type을 사용하면 어느 타입이 들어오든 에러가 나지 않는 방향으로 작동 -> 공통적으로 들어있는 속성만 접근할 수 있음
```

#### 선택적 매개변수
```TypeScript
function sendMessage (message: string, userName?: string): void {
  console.log(`${message}, ${userName}`)
}
sendMessage("hi") // hi, undefined

function sendMessage (message: string, userName?: string = 'everyone'): void {
  console.log(`${message}, ${userName}`)
}
sendMessage("hi") // hi, everyone
sendMessage("hi", "test") // hi, test
```

#### 타입 단언
- as(타입 단언 Type Assertion)
```TypeScript
class Character {
  isWizard() { // 체크함수 }
  isWarrior() { // 체크함수 }
}
class Wizard extends Character {
  fireBall() {
    // 기능
  }
}
class Warrior extends Character {
  attack() {
    // 기능
  }
}

function battle(character: Character) {
  if (character.isWizard()) {
    character.fireBall(); // Property 'fireBall' does not exist on type 'Character'.
  } else if (character.isWarrior()) {
    character.attack(); // Property 'attack' does not exist on type 'Character'.
  } else {
    character.runAway();
  }
}
```
Character 클래스에 fireBall()이나 attack() 메소드 구현 x -> 컴파일 에러 발생
```TypeScript
function battle(character: Character) {
  if (character.isWizard()) {
    (character as Wizard).fireBall(); // <Wizard>character.fireBall();
  } else if (character.isWarrior()) {
    (character as Warrior).attack();  // <Warrior>character.Attack();
  } else {
    character.runAway();
  }
}
```
타입 단언을 통해 컴파일 에러 해결 가능

#### 타입 가드
- isWizard()와 같은 메소드로 타입 체크를 하는 경우 컴파일러는 런타임에 타입을 알 수 있음
- 타입 가드는 런타임에서의 타입 체크를 컴파일러에게 미리 알려주는 기능
```TypeScript
class Character {
  isWizard(): this is Wizard {
    return this instanceof Wizard;
  }
  isWarrior(): this is Warrior {
    return this instanceof Warrior;
  }
}

function battle(character: Character) {
  if (character.isWizard()) {
    character.fireBall(); // 에러발생 x
  } else if (character.isWarrior()) {
    character.attack(); // 에러발생 x
  } else {
    character.runAway();
  }
}
```
- instanceof나 typeof 같은 오퍼레이터도 타입 가드의 일종
```TypeScript
function doSomething(val: string | number) {
  if (typeof val === 'number') {
    val.toFixed(); // Pass, val은 number 타입으로 추론
  } else {
    // Union 타입에서 `number`는 이미 통과했으므로 자동으로 `string`으로 추론됨
    val.toLowerCase(); // Pass, val은 string 타입으로 추론
  }
}
```

#### never 사용 이유
- 조건문에서 에러 핸들링 쉬워짐
```TypeScript
function func(x: string | number): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }
  // 자동적으로 x는 never
  return fail("Unexhaustive!");
}
function fail(message: string): never { throw new Error(message); }
// fail 함수가 성공적으로 실행되고 에러 반환

function func(x: string | number): boolean {
  if (typeof x === "string") {
    return true;
  } else if (typeof x === "number") {
    return false;
  }
  return fail("Unexhaustive!"); //Error
}
function fail(message: string): void { throw new Error(message); } // string을 리턴하면 never랑 똑같이 작동
// fail이 void 형이기 때문에 boolean 타입을 반환하는 func에서 사용 불가능
// 불가능한 위치에 추가적인 코드를 사용하게끔 해 줌 -> 더 나은 에러 메시지를 보여주거나 파일 또는 반복문과 같은 자원을 닫는 데 유용
// ??? string을 리턴했을 때 단점이 뭘까
```
- 두가지 속성을 모두 가진 객체를 가져오는 것 방지 가능
```TypeScript
type VariantA = {
  a: string
}
type VariantB = {
  b: number
}
function fn(arg: VariantA | VariantB): void ~~~
const input = { a: 'test', b: 123 }
fn(input) // 에러 발생 x
```
```TypeScript
type VariantA = {
  a: string
  b?: never
}
type VariantB = {
  b: number
  a?: never
}
function fn(arg: VariantA | VariantB): void ~~~
const input = { a: 'test', b: 123 }
fn(input) // 에러 발생
```
- 로직 상 도달할 수 없는 부분에 작성 -> 에러 핸들링 | 제약 조건 설정




*****
#### 참고
https://yceffort.kr/2022/03/understanding-typescript-never<br>
https://joshua1988.github.io/ts/intro.html<br>
https://yamoo9.gitbook.io/typescript/<br>
https://velog.io/@zeros0623/TypeScript-%EA%B3%A0%EA%B8%89-%ED%83%80%EC%9E%85<br>
https://hyunseob.github.io/2017/12/12/typescript-type-inteference-and-type-assertion/<br>
https://nomadcoders.co/typescript-for-beginners/lobby