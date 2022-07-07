### DOM
#### DOM 개념
- 문서 객체 모델(Document Object Model)
- HTML, XML 문서의 프로그래밍 interface
    - 브라우저는 HTML 코드 해석 -> 요소들을 트리 형태로 구조화 & 표현 문서(데이터) 생성 -> 이를 DOM이라고 함
    - DOM을 통해 웹 컨텐츠 렌더링 & 컨텐츠 추가, 수정, 삭제, 이벤트 처리 ...
- nodes, objects로 표현 -> 웹 페이지를 스크립트, 프로그래밍 언어에서 사용 가능하게 연결시켜줌
- 웹 페이지의 객체 지향 표현 & 자바스크립트, 스크립팅 언어를 이용해 DOM 수정 가능
- 문서의 구조화된 표현
```HTML
<html>
    <body>
        <p>Hello</p>
        <div><img src="tmp.png"/></div>
    </body>
</html>
```
- 위의 예시의 경우
    - HTMLElement 자식 노드로 BodyElement
    - BodyElement 자식 노드로 ParagraphElement와 DivElement


#### DOM & JavaScript
- DOM을 통해 JavaScript 언어는 웹 페이지 | XML 페이지 & 요소들과 관련된 모델이나 개념들에 접근 가능
- 문서의 모든 element를 DOM과 JavaScript와 같은 스크립팅 언어를 통해 접근 & 조작 가능
- JavaScript와는 독립적인 기술 표준

#### DOM 접근 방법
- 스크립트를 작성할 때 문서 자체를 조작하거나 문서의 children을 얻기 위해 document 또는 window elements를 위한 API를 사용 가능

#### DOM element 사용
- 생성
    - document.createElement를 사용하여 파라미터로 해당 태그 이름을 넘겨주면 태그에 해당하는 element 생성 가능
    - 생성된 element에는 setAttribute를 사용하여 속성을 넣거나, textContent를 사용하여 태그 안에 텍스트를 넣을 수 있음
```javascript
const newChild = document.createElement('div');
newChild.setAttribute('id',"new")
newChild.textContent = "NewChild"
newChild.classList.add("children")
```
```HTML
<div id="new" class="children">NewChild</div>
```

- 조회
    - id, className, tagName으로 검색도 가능(getElementsBy ~)
    - css 선택자를 통해서 element를 찾기 위해선 querySelector 사용
        - 전체 선택자 : * => 문서 내의 모든 요소
        - 유형 선택자 : elementname => elementname을 가진 모든 요소
        - 클래스 선택자 : .classname => classname라는 클래스 이름을 가진 모든 요소
        - ID 선택자 : #idname => idname라는 id 이름을 가진 모든 요소
    - querySelector은 css 선택자에 대응하는 element 중 첫번째 element 반환
    - querySelectorAll은 css 선택자에 대응하는 element 모두 반환
        - NodeList라는 유사배열로 반환 
        - 배열은 아니지만 숫자로 인덱싱된 key, length 사용 가능
        - 배열처럼 사용하고 싶은 경우 [...'NodeList']나 Array.from('NodeList') 사용
```HTML
<div id="parent">
    <div id="first" class="children">First</div>
    <div id="second" class="children">Second</div>
    <div id="third" class="children">Third</div>
</div>
```
```javascript
document.querySelector('#first')
//<div id="first" class="children">First</div>
document.querySelector('.children')
//<div id="first" class="children">First</div>
document.querySelectorAll('.children')
//NodeList(3) [div#first.children,div#second.children,div#third.children]
```

- 삽입
    - appendChild를 사용하여 부모 element의 마지막 자식으로 추가 가능
    - insertBefore를 사용하면 기준이 되는 형제 element앞에 element 앞에 추가 가능
    - insertAdjacentElement를 사용하면 타겟 element의 상대적인 위치에 추가 가능
        - 'beforebegin' : 타겟 element의 앞
        - 'afterbegin' : 타겟 element의 첫번째 자식 앞
        - 'beforeend' : 타겟 element의 마지막 자식 뒤
        - 'afterend' : 타겟 element의 뒤
```javascript
parent.appendChild(newChild)
parentElement.insertBefore(newElement, siblingElement);
targetElement.insertAdjacentElement('where', newElement);
```

- 삭제
```javascript
parentElement.removeChild();
parentElement.remove();
```


*****
#### 참고
https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction
https://iamdaeyun.tistory.com/entry/DOM-01Document-Object-Model-DOM의-뜻
https://www.youtube.com/watch?v=zyz1eJJjsNE