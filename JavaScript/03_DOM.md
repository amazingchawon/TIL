# DOM

## 개요

1. 웹 브라우저에서의 JavaScript
    - 웹 페이지의 동적인 기능을 구현
2. JavaScript 실행 환경 종류
    - HTML script 태그
    - js 확장자 파일
        - 파일을 html 파일로 불러와서 웹 브라우저에서 실행
        - node.js를 사용해서 브라우저 없이 실행
    - 브라우저 console

## 문서 구조

1. Document structure
    
    ![image](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiIugUPtI8Tbtt-Y4JsdBpU0tLfE8Xz-4OLXMasdHI8j2RsN4-2hLQpI-qOIzXJgigbTVDurEkVyJHikYq-0Ee8-azUHuQbSweWe-xO0xES9tu7EafCXJvJOJiOwmUUNLBmYOt8pgvKDDk/s1600/html-structure.png)
    
    - HTML 문서는 상자들이 중첩된 형태로 볼 수 있음
        - 상속 관계 존재
    - 브라우저가 문서를 표현하기 위해 사용하는 데이터 구조는 이미지와 같은 모양을 가짐
    - 각 상자는 객체이며 개발자는 이 객체와 상호작용하여 어떤 HTML 태그를 나타내는지, 어떤 콘텐츠가 포함되어 있는지 등을 알아낼 수 있음
    - 이 표현을 Document Object Model, 줄여서 DOM이라고 부름
2. DOM 정의 :
    - The Document Object Model
    - 웹 페이지(Document)를 구조화된 객체로 제공하여 프로그래밍 언어가 페이지 구조에 접근할 수 있는 방법을 제공
    - 문서 구조, 스타일, 내용 등을 변경할 수 있도록 함

## DOM API

1. 설명 :
    - 다른 프로그래밍 언어가 웹 페이지에 접근 및 조작할 수 있도록 페이지 요소들을 객체 형태로 제공하며 이에 따른 메서드 또한 제공

## document 객체

1. 설명 :
    - 웹 페이지를 나타내는 DOM 트리의 최상위 객체
        - HTML 문서의 모든 콘텐츠에 접근하고 조작할 수 있는 진입점
    - 웹 페이지 객체
    - DOM Tree의 진입점
    - 페이지를 구성하는 모든 객체 요소를 포함
    - DOM에서 모든 요소, 속성, 텍스트는 하나의 객체
        - 모두 document 객체의 하위 객체로 구성됨
2. DOM tree
    - HTML 태그를 나타내는 elements의 node는 문서의 구조를 결정
    - 이들은 다시 자식 node를 가질 수 있음
        - 객체 간 상속 구조가 존재

3. DOM 핵심

- 문서의 요소들을 객체로 제공하여 다른 프로그래밍 언어에서 접근하고 조작할 수 있는 방법을 제공하는 API

## DOM 선택

1. DOM 조작 시 기억해야 할 것
    - 웹 페이지를 동적으로 만들기 == 웹 페이지를 조작하기
    - 조작 순서
        - 조작하고자 하는 요소를 선택 (또는 탐색)
        - 선택된 요소의 콘텐츠 또는 속성을 조작

### 선택 메서드

1. `document.querySelector(selector)` : 요소 한 개 선택
    - 제공한 선택자와 일치하는 element 한 개 선택
    - 제공한 선택자를 만족하는 첫 번째 element 객체를 반환 (없다면 null 반환)
2. `document.querySelectorAll(selector)` : 요소 여러 개 선택
    - 제공한 선택자와 일치하는 여러 element를 선택
    - 제공한 선택자를 만족하는 NodeList를 반환
3. 실습
    
    ```html
    <script>
      console.log(document.querySelector('.heading'))
      console.log(document.querySelector('.content'))
      console.log(document.querySelectorAll('.content'))
      console.log(document.querySelectorAll('ul > li'))
    </script>
    ```
    

## DOM 조작

1. 종류
    - 속성 attribute 조작
        - 클래스 속성 조작
        - 일반 속성 조작
    - HTML 콘텐츠 조작
    - DOM 요소 조작
    - 스타일 조작

### 속성 attribute 조작

1. 클래스 속성 조작
    - `classList` property :
        - 요소의 클래스 목록을 DOMTokenList(유사 배열) 형태로 반환
        - 메서드
            - `element.classList.add()` : 지정한 클래스 값을 추가
            - `element.classList.remove()` : 지정한 클래스 값을 제거
            - `element.classList.toggle()` : 클래스가 존재한다면 제거하고 false를 반환, 존재하지 않는다면 클래스를 추가하고 true를 반환
    - 실습 :
        
        ```jsx
        <script>
            // 속성 요소 조작
        
            // 1. 클래스 속성 조작
            // h1 요소를 선택
            const h1Tag = document.querySelector('.heading')
            // h1 요소의 클래스 목록 확인
            console.log(h1Tag.classList)
        
            // h1 요소의 클래스 목록에 red 클래스 추가
            h1Tag.classList.add('red')
            console.log(h1Tag)
            console.log(h1Tag.classList)
        		
            h1Tag.classList.remove('red')
            console.log(h1Tag.classList)
        
            h1Tag.classList.toggle('red')
            console.log(h1Tag.classList)
        </script>
        ```
        
2. 일반 속성 조작 메서드
    - `element.getAttribute()` : 해당 요소에 지정된 값을 반환(조회)
    - `element.setAttribute(name, value)`
        - 지정된 요소의 속성 값을 설정
        - 속성이 이미 있으면 기존 값을 갱신 (그렇지 않으면 지정된 이름과 값으로 새 속성이 추가)
    - `element.removeAttribute()` : 요소에 지정된 이름을 가진 속성을 제정
    - 실습
        
        ```jsx
        <script>
            // 2. 일반 속성 조작
            // a 요소 선택
            const aTag = document.querySelector('a')
            console.log(aTag.getAttribute('href'))
        
            // a 요소의 href 속성 값을 naver로 변경
            aTag.setAttribute('href', 'https://www.naver.com/')
            console.log(aTag.getAttribute('href'))
        
            // a 요소의 href 속성 값 삭제
            aTag.removeAttribute('href')
            console.log(aTag.getAttribute('href'))
          </script>
        ```
        

### HTML 콘텐츠 조작

1. `textContent` property
    - 요소의 텍스트 콘텐츠를 표현
2. 실습
    
    ```jsx
    <script>
      // HTML 콘텐츠 조작
      // h1 요소 선택
      const h1Tag = document.querySelector('.heading')
      console.log(h1Tag.textContent)
    
      // h1 요소의 콘텐츠 값을 변경
      h1Tag.textContent = '내용 수정'
      console.log(h1Tag.textContent)
    </script>
    ```
    

### DOM 요소 조작 메서드

1. `document.createElement(tagName)` : 생성
    - 작성한 tagName의 HTML 요소를 생성하여 반환
2. `Node.appendChild()` : 추가
    - 한 Node를 특정 부모 Node의 자식 NodeList 중 마지막 자식으로 삽입
    - 추가된 Node 객체를 반환
3. `Node.removeChild()` : 삭제
    - DOM에서 자식 Node를 제거
    - 제거된 Node를 반환
4. 실습
    
    ```jsx
    <script>
      // 생성
      const h1Tag = document.createElement('h1')
      console.log(h1Tag) // <h1></h1> -> 뒤의 코드때문에 이렇게 보이지는 않음
      h1Tag.textContent = '제목'
      console.log(h1Tag) // <h1>제목</h1>
      
      // 추가
      const divTag = document.querySelector('div')
      divTag.appendChild(h1Tag)
      console.log(divTag)
      
      // 삭제
      const pTag = document.querySelector('p')
      divTag.removeChild(pTag)
      
    </script>
    ```
    

## style 조작

1. style property
    - 해당 요소의 모든 style 속성 목록을 포함하는 속성
2. 실습
    
    ```jsx
    <script>
      const pTag = document.querySelector('p')
    
      console.log(pTag.style)
      pTag.style.color = 'crimson'
      pTag.style.fontSize = '3rem'
      pTag.style.border = '2px solid black'
    
    </script>
    ```
    

## 참고

## DOM 속성 확인 Tip

1. 개발자 도구 - Elements - Properties
    - 선택한 요소의 모든 DOM 속성 확인 가능

## 용어 정리

1. Node
    - DOM의 기본 구성 단위
    - DOM 트리의 각 부분은 Node라는 객체로 표현됨
        - Document Node : HTML 문서 전체를 나타내는 노드
        - Element Node : HTML 요소를 나타내는 노드 (EX) <p>
        - Text Node : HTML 텍스트 (Element Node 내의 텍스트 컨텐츠를 나타냄)
        - Attribute Node : HTML 요소의 속성을 나타내는 노드
2. NodeList
    - DOM 메서드를 사용해 선택한 Node의 목록
    - 배열과 유사한 구조를 가짐
    - Index로만 각 항목에 접근 가능
    - JavaScript의 배열 메서드 사용 가능
    - querySelectorAll()에 의해 반환되는 NodeList는 DOM의 변경사항을 실시간으로 반영하지 않음
        - DOM이 나중에 변경되더라도 이전에 이미 선택한 NodeList 값은 변하지 않음
3. Element
    - Node의 하위 유형
    - Element는 DOM 트리에서 HTML 요소를 나타내는 특별한 유형의 Node
        - <p>, <div>, <span>, <body> 등의 HTML 태그들이 Element 노드를 생성
    - Node의 속성과 메서드를 모두 가지고 있으며 추가적으로 요소 특화된 기능을 가지고 있음
    - 모든 Element는 Node이지만, 모든 Node가 Element인 것은 아님
4. Parsing
    - 구문 분석, 해석
    - 브라우저가 문자열을 해석하여 DOM Tree를 만드는 과정
5. 세미콜론
    - 자바스크립트는 문장 마지막 세미콜론을 선택적으로 사용 사능
    - 세미콜론이 없으면 ASI에 의해 자동으로 세미콜론이 삽입됨
        - ASI (Authomatic Semicolon Insertion)
    - JavaScirpt를 만든 Brendan Eich 또한 세미콜론 작성을 반대