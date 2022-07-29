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

#### 타입
```TypeScript
let isLogin: boolean = false;
let num: number = 10;
let str: String = 'test';
let obj: object = { name: 'test', age: 1 }
// object의 경우 모든 타입 값 할당 가능 -> 타입 검사에 엄격한 타입스크립트 목적이 모호해짐
let arr: number[] = [1,2,3];
let arr: Array<number> = [1,2,3];

//tuple
let arr: [string, number] = ['hi', 10];
// ex) 사용자 정보 저장 [index, id, password] -> 특정 길이 & 데이터 타입 순서 정해짐

// Enum
enum Team { Manager , Planner = 100, Designer }
// Team.Manager -> 0, Team.Planner -> 100, Team.Designer -> 101
// Team[100] -> 'Planner'

// any
let str: any = 'hi';
any = 1;
// 모든 타입에 대해 허용

// void
let unuseful: void = undefined;
function test(): void{
    // 리턴값 설정 불가능
}
// 변수에는 undefined나 null만 할당 가능 / 함수에는 반환값 설정 불가능

// never
function func(): never{
    while(true){

    }
}
// 함수의 끝에 절대 도달하지 않는다는 의미

// union
function setInfo(id: number | String, name: String){
    return { id, name };
}
// 매개 변수에 number, string 모두 가능하게 파이프를 사용하여 설정 가능
```
