### Dart Week 1
#### Dart언어
- 구글이 2011년 웹 프론트엔드 개발을 위해 만든 언어
- 객체 지향형 언어 + C언어와 유사한 문법
- hot reload(실행 중인 앱에 즉시 결과 표시)
- AOT(Ahead of time compile) 컴파일로 네이티브 코드 생성 -> 모든 플랫폼에서 빠른 속도 제공
    - 소스 코드를 미리 컴파일 하는 방식
    - 다시 컴파일할 필요없이 바로 실행 가능 -> 컴파일에 소요되는 시간이 빠짐
    - 필요할 때 필요한 만큼만 컴파일하는 JIT와 비교되는 방식
    - Just-in-time 컴파일(JIT) -> 필요할 때 필요한 만큼만 + 처음 실행될 때 인터프리트, 자주 쓰이는 코드 캐싱

#### 기본 문법
- import 사용 -> 패키지 내 라이브러리 사용
- //, /* */ 주석 사용
- 함수도 c언어와 비슷한 구조로 선언(리턴 값/타입 지정 안해도 됨)
```dart
add(int a, int b){
    return a+b;
}
```
- main() 존재
- 변수는 var 키워드 사용 가능(다트의 모든 변수는 객체임)
- 문자열 리터럴 출력은 ‘내용’ “내용” 모두 가능 + 변수는 ‘내용 ${표현식}’으로 표현 가능

#### 변수
- 모든 변수는 객체 + 모든 객체는 클래스의 인스턴스
    - 클래스 : 객체를 만들기 위한 설계도 
    - 객체 : 소프트웨어 세계에 구현할 대상(실체)
    - 인스턴스 : 소프트웨어 세계에 구현된, 메모리에 할당된 구체적 실체
- 타입 추론이 가능한 경우 타입 어노테이션 가능
```dart
int number = 10;
var number = 10;
//대신 사용 가능
```
- 타입이 예상되지 않는 경우 dynamic 키워드 사용 가능(Object도 사용 가능) - 타입 변경도 가능
    1. 제네릭 타입 지원(하나의 값이 여러 다른 데이터 타입을 가질 수 있음)
    2. main()과 같은 최상위 함수 지원
    3. public, protected, private 키워드 없음(_로 private 사용 가능)
        - 접근 지정자
        - public : 모든 접근 허용 / 어떤 클래스든 접근 허용
        - protected : 상속받은 클래스 | 같은 패키지(비슷한 성격의 클래스끼리 분류)에서만 접근 가능
        - private : 외부에서 접근 불가능 / 같은 클래스 내에서만 접근 가능

#### 키워드
1. 식별자 : 변수, 함수 등의 이름
    1. 내장 식별자 존재 -> 클래스명, 타입명, import시 prefix로 사용 불가능
    ```dart
    import 'package' as static;
    //에러 발생
    ```
2. 키워드
    1. 특정 문맥에서 특별한 의미를 가지는 단어
    2. 특정 문맥이 아닌 경우 식별자로 사용 가능
    3. 문맥 키워드
        - sync : 동기 함수 지정
        - async : 비동기 함수 지정
        - hide : import할 때 라이브러리의 일부만 제외하고 싶을 때 제외할 부분 선택
        - on : 특정 예외 에러 지정 & 처리
            - Dart에서는 모든 클래스가 하나의 슈퍼클래스만 가질 수 있음 
            - 여러 클래스 계층에서 클래스 코드 재사용 방법 필요
            - with 키워드를 통해 mixin 사용 가능
            - on 키워드를 통해 mixin으로 선언된 클래스 확장 | 클래스 제한 가능(on키워드를 사용하려면 mixin 키워드를 사용해서 mixin 선언 필수)
        - show : import할 때 라이브러리의 일부만 사용하고 싶을 때 사용할 부분 선택
3. 예약어
    1. 식별자로 사용할 수 없는 특별한 단어
    2. await, yield의 경우 비동기/동기 함수의 바디에서는 식별자로 사용 불가능
    3. 그 외에도 많은 예약어가 있음

#### 상수
1. final -> 런타임에 상수가 됨
2. const -> 컴파일 시점에 상수가 됨
3. 런타임에 상수화 되는 final의 경우 다른 함수에서 받아온 값으로 설정 가능
4. const는 컴파일 시점에 상수화 되었기 때문에 다른 함수에서 받아온 값으로 설정 시 오류 발생
<!-- 5. 컴파일 에러 -> 프로그램이 컴파일링 되는 것을 방해하는 신택스 에러 | 파일참조 오류와 같은 문제(실행 가능한 프로그램으로 컴파일 되지 못함) ex) 신택스 에러, 타입체크 에러
6. 런타임 에러의 경우 -> 실행 가능한 프로그램으로 컴파일 됨 but 실행중에 버그 발생 ex) 0나누기, null참조, 메모리 부족 -->

#### 함수 
- 변수가 함수 참조 가능
- 함수 인자로 함수 전달 가능
- 이름이 있는 선택 매개 변수 지정 가능
```dart
getAddress(String par1,{String par2, String par3}){}
//중괄호로 매개변수를 감싸줘야함
getAddress('seoul',{par2:'gangnam',par3:'123'});
```
- 위치적 선택 매개변수
```dart
getAddress(String par1, [String par2=init2, String par3=init3]){}
//대괄호로 선언
getAddress('seoul');
getAddress('seoul','gangnam');
```
- 익명 함수 & 람다식 사용 가능
```dart
//익명 함수
var multi = (_a,_b){
    return _a * _b;
};
//람다식
sub(_a,_b) => _a-_b;
```

#### 연산자
1. 산술 연산자(+,-,*,/,~/,%,++,--)
2. 할당 연산자(=,+=,-=,*=,/=,~/=)
3. 관계 연산자(==,~=,>,<,>=,<=)
4. 비트 & 시프트 연산자(&,|,^,~,>>,<<)
5. 타입 검사 연산자
    1. as : 형 변환(상위 타입으로 변환 가능)
    2. is : 특정 객체가 특정 타입이면 true
    3. is! : 특정 타입이 아니면 true
6. 조건 표현식
    1. 삼항 연산자
    ```dart
    a>0 ? '양수':'음수';
    ```
    2. 조건적 멤버 접근 연산자
        - 좌항이 null이면 null리턴 / 아니면 우항의 값 리턴
        ```dart
        employee?.name
        //employee가 null이면 null return
        //null이 아니라면 employee.name return
        ```
    3. ?? 연산자
        - 좌항이 null이 아니면 좌항 값 리턴 / null이면 우항 값 리턴
        ```dart
        employee.name??'new name'
        //좌항이 null이 아니면 좌항값인 employee.name return
        //null이라면 'new name' return
        ```
7. 캐스케이드 표기법
    - .. => 객체의 속성이나 멤버 함수를 연속으로 호출할 때 유용
    ```dart
    Employee employee = Employee()
    ..name = 'Kim'
    ..setAge(25)
    ..showInfo()
    ```

#### 조건문
1. if, if~else문
1. switch문 -> break 주의 / default 처리
2. assert(조건식) -> 조건식이 거짓이면 에러 발생
    
#### 클래스
1. 모든 객체는 클래스의 인스턴스 & 모든 클래스는 Object 클래스의 자식
2. 클래스 외부에서 기능을 하는 함수는 Function / 클래스 내부에 있는 멤버 함수는 Method
3. 기본적으로 객체 생성 시 new 키워드 생략

#### 생성자
1. 기본 생성자 
    - 생성자를 생략 가능
    - 클래스명과 동일, 인자x
    - 클래스에 생성자가 없는 경우 자동으로 기본 생성자 제공 + 만약 상위 클래스가 존재한다면 상위 클래스의 기본 생성자를 호출
2. 이름 있는 생성자
    - 생성자에 이름 부여 
    - 이름 없는 생성자 2개 선언 불가능 -> 중복 선언 에러 발생
    - 이름 있는 생성자 선언시 기본 생성자 생략 불가능
3. 초기화 리스트
    - 생성자의 구현부 실행 전에 인스턴스 변수 초기화 가능
    - 초기화 리스트는 생성자 옆에 : 으로 선언할 수 있음
    ```dart
    class Person{
        String name;

        Person() : name = ‘Kim’ { 
            print('$name');
        }
    }    
    ```
4. 리다이렉팅 생성자
    - 본체가 비어있고 메인 생성자에게 위임 가능 -> this를 통해 파라미터로 할당 가능
```dart
Person(this.name, this.age){
    print
}
Person.initName(String name) : this(name,20);
//
var person = Person.initName('Kim');
```
5. 상수 생성자 : 인스턴스 변수가 모두 final, 생성자는 const 키워드 필수
```dart
class Person{
    final String name;
    final num age;

    const Person(this.name,this.age);
}
main(){
    Person person1 = const Person('Kim',20);
    Person person2 = const Person('Kim',20);
    //person1과 person2는 같은 인스턴스를 참조
}
```
6. 팩토리 생성자
    - 팩토리 메소드 패턴 : 객체 생성을 캡슐화 & 서브 클래스에서 어떤 클래스를 만들지 결정
    - 조건문을 만들어 본인 클래스 인스턴스 생성이 아닌 자식 클래스의 인스턴스를 리턴 받을 수 있게 만듦

*****
#### 참고
https://brunch.co.kr/brunchbook/dartforflutter
