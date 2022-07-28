### CSS
#### 적용 방법
1. 인라인 스타일(Inline style)
    - HTML 요소 내부에 style 속성을 사용하여 css 스타일을 적용하는 방법
    - 해당 요소에만 스타일 적용 가능
    ```HTML
    <p style="color:green;">test</p>
    ```
2. 내부 스타일 시트(Internal style sheet)
    - HTML 문서의 <head> 태그 내에 <style> 태그를 사용하여 스타일 지정
    - 내부 스타일 시트는 해당 HTML 문서에만 스타일 적용 가능
    ```HTML
    <style>
        body {background-color: yellow;}
        p {color:red;}
    </style>
    ```
3. 외부 스타일 시트(External style sheet)
```HTML
<link rel="stylesheet" href="/test.css">
```
```CSS
body{background-color: yellow;}
p{color:red;}
```


#### 우선순위
1. 인라인 스타일
2. 내부 / 외부 스타일 시트 -> 가장 마지막에 적용된 스타일 시트
3. 웹 브라우저 기본 스타일


#### 선택자
1. 전체 선택자
    - HTML 문서 내부의 모든 요소 선택
    ```HTML
    <style>
        * {color:red;}
    </style>
    ```
2. HTML 요소 선택자
    - CSS를 적용할 대상으로 HTML요소 이름 직접 사용
    ```HTML
    <style>
        h2{color:ret; text-decoration: underline;}
    </style>
    ```
3. id 선택자
    - 특정 아이디 이름을 가지는 요소만 선택(같은 이름의 아이디 모두 적용)
    ```HTML
    <style>
        #heading{color:red;}
    </style>
    ```
4. class 선택자
    - 특정 집단의 클래스를 가지는 요소를 모두 선택
    ```HTML
    <style>
        .headings{color:red;}
    </style>
    ```
5. 그룹 선택자
    - 위의 선택자들을 같이 사용하고자 할 때 ,로 구분하여 연결
    ```HTML
    <style>
        h2, h3{color:red;}
    </style>
    ```
6. 자손 선택자
    - 해당 요소의 하위 요소 중에서 특정 타입의 요소 선택
    ```HTML
    <style>
        div p {color:red;}
    </style>
    ```
7. 자식 선택자
    - 해당 요소의 바로 밑에 존재하는 하위 요소 중에서 특정 타입의 요소 모두 선택
    ```HTML
    <style>
        div > p {color:red;}
    </style>
    ```
8. 일반 동위 선택자
    - 같은 부모 요소를 가진 요소(형제 요소) 중에 해당 요소보다 뒤에 존재하는 타입 모두 선택
    ```HTML
    <style>
        div ~ p { color:red; }
    </style>
    ```
9. 인접 동위 선택자
    - 해당 요소와 동위 관계 & 요소의 바로 뒤에 존재하는 특정 타입 요소 선택
    ```HTML
    <style>
        div + p { color:red; }
    </style>
    ```
10. 속성 선택자
    - 기본 속성 선택자
        - [속성이름] 선택자 -> 특정 속성을 가지고 있는 요소 모두 선택
        - [속성이름="속성값"] 선택자 -> 특정 속성을 가지고, 속성값까지 일치하는 요소들 모두 선택
    - 문자열 속성 선택자
        - [속성이름~="속성값"] 선택자 -> 특정 속성의 속성값에 특정 문자열로 이루어진 하나의 단어를 포함하는 요소를 모두 선택
            ```HTML
            <style>
                [title~="first"] {
                    background: black;
                    color: yellow;
                }
            </style>
            
            <h2 title="first h2">test</h2>
            <p title="first p">test</p>
            <p title="first-p">test</p>
            ```
            - 속성값이 정확히 "first"거나 띄어쓰기 기준으로 인식되는 단어에 "first"포함("first-p"는 해당X)
        - [속성이름|="속성값"] 선택자 -> 특정 속성의 속성값이 특정 문자열로 이루어진 하나의 단어로 시작하는 요소를 모두 선택
            - [title|="first"] 라면 title의 속성값이 정확히 "first"인 요소나 "first" 바로 다음에 -로 시작하는 요소만을 선택
        - [속성이름^="속성값"] 선택자 -> 특정 속성의 속성값이 특정 문자열로 시작하는 요소 모두 선택
        - [속성이름$="속성값"] 선택자 -> 특정 속성의 속성값이 특정 문자열로 끝나는 요소를 모두 선택
        - [속성이름*="속성값"] 선택자 -> 특정 속성의 속성값에 특정 문자열을 포함하는 요소를 모두 선택




#### 동적 의사(pseudo) 클래스
1. :link -> 사용자가 한 번도 링크를 통해서 연결된 페이지를 방문하지 않은 상태
2. :visited -> 사용자가 한 번이라도 링크를 통해서 연결된 페이지를 방문한 상태
3. :hover -> 사용자의 마우스 커서가 링크 위에 올라가 있는 상태
4. :active -> 사용자가 마우스로 링크를 클릭하고 있는 상태
5. :focus -> 포커스를 가지고 있는 input 요소


#### 구조 의사 클래스
1. :first-child -> 모든 자식 요소 중에서 맨 앞에 위치하는 자식 요소
2. :last-child -> 모든 자식 요소 중에서 맨 마지막에 위치하는 자식 요소
3. :nth-child -> n번째에 위치하는 자식 요소


#### 의사 요소 문법
1. ::first-letter -> 텍스트의 첫 글자 선택
2. ::first-line -> 텍스트의 첫 라인 선택
3. ::before -> 특정 요소의 내용 부분 바로 앞에 다른 요소를 삽입할 때 사용
```HTML
<style>
    p::before {content: url("/img.png");}
</style>
```
4. ::after
5. ::selection -> 해당 요소에서 사용자가 선택한 부분만 선택할 때 사용
```HTML
<style>
    p::selection {color:red}
</style>
```

*****
#### 참고
http://www.tcpschool.com/html/html_expand_css<br>