### Dart Week 2
#### 상속
```dart
class SuperClass {
    //변수
    //함수
}
class SubClass extends SuperClass{
    @override
    //함수
}
```

- Super Class가 가지고 있는 method는 Sub Class에서 그대로 사용 | 재정의(@override)해서 사용 가능
- 코드 재사용 & 클래스 간소화 -> 수정 및 추가 용이

#### 접근 지정자
- 접근 지정자
    - 다트에서는 두 종류의 접근 지정자 존재
    - private : 자바와는 다르게 동일 클래스에서만 접근 가능한 것이 아니라 같은 라이브러리(자바 기준 패키지)에서도 접근 가능
    - public : 접근 범위 제한 없이 모든 곳에서 접근 가능
    - 아무 키워드 없이 사용하면 public, 변수나 메소드 앞에 _ 사용시 private
- 캡슐화
    - 어떤 객체가 어떤 목적을 수행하기 위한 데이터(멤버 변수) & 기능(메소드)을 적절하게 모으는 것
    - 하나의 기능을 수행하는 객체를 만드는 것
- 추상화
    - 객체의 공통화된 데이터와 메소드를 묶어서 이름을 부여하는 것
    - 공통 속성을 추출해나가는 과정 -> 코드 재사용률 높임

#### Getter & Setter
- 사용 목적
    - 변수를 public으로 선언하면 의도하지 않은 곳에서 접근하여 변경하는 일 발생 가능
    - 실수로 변경된 값을 다시 수정하는 데에 어려움
    - 클래스의 내부 정보를 공개하지 않고, 정보를 은닉하는 방법 사용 가능
- 사용 방법
    - 캡슐화를 통해 사용 가능
    - 멤버 변수를 private으로 선언 & 변수에 접근할 수 있는 메소드는 public으로 선언
```dart
class Person {
    String _name;
    get name{
        return this._name;
    }
    set name(String name){
        this._name = name;
    }
}
```

#### 추상 클래스
- 목적은 추상 메소드가 포함된 클래스를 상속받는 자식 클래스가 반드시 추상 메소드를 구현하도록 하기 위함
- 추상 메소드를 가질 수 있는 클래스 / 일반 클래스에서는 추상 메소드 선언 불가능
- 추상 클래스는 미완성 클래스 -> 객체 생성 불가능, 참조형 변수 타입으로 사용 가능
- 추상 클래스는 일반 메소드도 정의 가능, 하지만 임플리먼트 되는 클래스에서 재정의 되어야함
- extends 상속 클래스의 경우 하나의 클래스만 상속 가능 but 추상 클래스는 여러 개를 임플리먼트 할 수 있음
```dart
abstract class Person{
    eat();
}
class Developer implements Person{
    //오버라이딩 필수(@override 어노테이션 생략 가능)
    @override
    eat(){
        print('Developer eat a meal');
    }
}
```

#### 컬렉션
- List
    ```dart
    List<dynamic> tmp = ['Red', 1, 2.5];
    var tmp2 = [1, 2.5, 'Red'];
    //여러 타입의 데이터를 저장도 가능
    ```
    - 인덱스로 접근 가능, 0 base
    - add, addAll로 데이터 추가
    - remove, removeAt, clear로 요소 삭제 
    
    - sort, isEmpty, reversed 같은 메소드, 프로퍼티 사용 가능
- Set
    - List와 다르게 데이터 순서가 없고 중복 요소 허용 X
    - 선언 및 초기값 지정도 List와 비슷하지만 List는 []를, Set은 {} 사용
    - 순서가 없어서 인덱스 접근 불가능 -> for..in 문을 통해 접근 가능
- Map
    - 키와 값으로 이루어짐 -> 키에 대한 값이 매칭, 빠른 탐색 가능
    - 순서가 존재하지 않지만 키로 정수를 사용하면 순서를 가진 것처럼 사용 가능
    - 키는 중복 불가능, 값은 중복 가능
    ```dart
    Map<key type, value type> tmp = {
        key1 : value1,
        key2 : value2,
        key3 : value3
    };
    //or
    Map<key type, value type> tmp = Map();
    tmp[key1] = value1;
    tmp[key2] = value2;
    ```
    - update 메소드를 이용해서 매핑값을 변경할 수 있음
    ```dart
    tmp.update(key1, (value) => 'NewRed', ifAbsent: () => 'NewColor');
    tmp.update(key5, (value) => 'NewBlue', ifAbsent: () => 'NewColor');
    //존재하는 키의 경우 업데이트, 키가 없는 경우 ifAbsent로 지정한 값으로 새로 지정
    ```


#### 제네릭
- 컬렉션에서 <> 안에 타입매개변수를 지정하는 것('매개변수화 타입을 정의한다'고 함) 
```dart
List<String> colors = [];
List<int> numbers = [];
```
- 특정 타입을 미리 지정해주는 것이 아니라 필요에 따라 지정할 수 있도록 하는 타입(코드 일반화)
- 매개변수화 타입을 제한할 수 있음
```dart
class Student extends Person{
    eat(){ ~~ }
}
class Manager<T extends Person>{
    // 해당 특정 클래스와 클래스의 하위 클래스가 타입 매개변수가 될 수 있음
    eat(){ ~~ }
}
//
var temp1 = Manager<Person>();
var temp2 = Manager<Student>();
```
- 메소드에서도 제네릭 사용 가능
```dart
class Person{
    T getName<T>(T param){
        return param;
    }
}
//
print(person.getName<String>('Kim'));
```


#### 비동기 처리
- Isolate
    - 모든 코드가 실행되는 공간
    - 싱글 쓰레드, 이벤트 루프를 통해 작업 처리
    - 다른 언어의 경우 쓰레드가 서로 메모리를 공유하는 구조
    - isolate의 경우 쓰레드가 자체적으로 메모리를 가지고 있음, 메모리 공유 x
    - 다른 쓰레드가 같은 작업하려면 message를 주고 받아야함 but 공유 자원에 대한 처리 필요 x
    - 새로운 isolate는 spawn을 통해 만들 수 있음
     <p align="center"><img src="https://t1.daumcdn.net/thumb/R1280x0.fpng/?fname=http://t1.daumcdn.net/brunch/service/user/2Kn8/image/kGl8skuXDaGhoLOtRHXaBhzFHwM.png"></p>    
```dart
Isolate.spawn(isolateTest, 1);
Isolate.spawn(isolateTest, 2);
```
```dart
//ReceivePort 객체 생성으로 isolate 간 message 주고받기 가능
ReceivePort mainReceivePort = new ReceivePort();
```
- Future
    - 작업의 결과값을 나중에 받는 것
    - Future 상태
        - Uncompleted : future 객체를 만들어서 작업을 요청한 상태
        - Completed : 요청 작업이 완료된 상태
            - data : 정상적으로 작업 수행 & 결과값 리턴
            - error : 작업 처리 중 문제 발생 시 에러 발생
    - 이벤트 루프에 의해 순차적으로 처리
        - 처음 future를 생성하고 작업을 시작하면 Uncompleted future가 event queue에 들어감
        - 다른 작업들도 event queue에 들어가면서 작업 실행
        - future가 작업을 끝내면 Completed future가 event queue에 들어가고 event loop에 의해 호출되면 결과값 처리
        <p align="center"><img src="https://t1.daumcdn.net/thumb/R1280x0.fjpg/?fname=http://t1.daumcdn.net/brunch/service/user/2Kn8/image/0EjTckKUpPO1rfL9EHJvBtkMQB4.PNG"></p>
```dart
Future<T> tmp = new Future(){
    // 작업 시작을 알리는 Uncompleted future 상태로 event queue에 들어감
    // 별도의 쓰레드로 진행됨 (모두 처리되기 전에 다음 작업 수행중)
    // return
}
tmp.then((ret){
    //작업이 완료되면 출력됨
    print(ret);
}, onError: (e){
    //에러가 발생하면 출력됨
    print(e);
});
```
- async, await
    - 함수명 뒤에 async 키워드를 붙임
    - await가 붙은 작업은 해당 작업이 끝날 때까지 다음으로 넘어가지 않고 대기
- stream 
    - 비동기 처리에서 어느 타이밍에 데이터가 들어올지 알기 어려움 -> 데이터를 만드는 곳 + 관찰자를 만들어서 데이터가 생성되는 것을 관찰
    - listener가 대상의 변화를 관찰 & 변화를 알려줌 -> Observer pattern 
        1. Stream 만들기
        2. 연결(listen)
        3. 데이터 처리 과정
    ```dart
    void main(){
        print("st");
        // 일반적인 데이터 저장 & 관찰
        Stream.fromIterable([1,2,3,4,5])
            .listen((int x) => print('iterable : ${x}'));
        // 비동기 처리를 위해 Stream.fromFuture() 사용
        Stream.fromFuture(getData(2))
            .listen((x) => print('from Future : ${x}'));
        Stream.fromFuture(getData(5))
            .listen((x) => print('from Future : ${x}'));
        print("ed");
    }
    Future<String> getData(int n) async{
        await Future.delayed(Duration(seconds: n)); // 5초간 대기
        print("Fetched Data");
        return "${n}초 후 데이터";
    }
    /* 
    st
    ed
    iterable : 1
    iterable : 2
    iterable : 3
    iterable : 4
    iterable : 5
    Fetched Data
    from Future : 2초 후 데이터
    Fetched Data
    from Future : 5초 후 데이터
    */
    ```
- async*, yield
    - 함수에 return 대신에 yield를 사용하여 제너레이터 함수를 만들 수 있음
    ```dart
    main(){
        var stream = getData();
        stream.listen((x) => print(x));
    }
    Stream<int> getData() async* {
        for(int i=0;i<5;i++){
            yield i;
        }
        //i 대신 데이터를 yield하는 경우에는 순차적으로 데이터를 받을 수 있음
    }
    /*
    0
    1
    2
    3
    4
    */
    ```


*****
#### 참고
https://brunch.co.kr/brunchbook/dartforflutter<br>
https://software-creator.tistory.com/9<br>