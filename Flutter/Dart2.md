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

- Super Class가 가지고 있는 method는 Sub Class에서 그대로 | 재정의(@override)해서 사용 가능
- 코드 재사용 & 클래스 간소화 -> 수정 및 추가 용이

#### 접근 지정자
- 캡슐화
    - 어떤 객체가 어떤 목적을 수행하기 위한 데이터(멤버 변수) & 기능(메소드)을 적절하게 모으는 것
    - 