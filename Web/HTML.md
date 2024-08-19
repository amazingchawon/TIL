# 웹 - HTML

1. World Wide Web : 인터넷으로 연결된 컴퓨터들이 정보를 공유하는 거대한 정보 공간
2. Web : Web site, Web application 등을 통해 사용자들이 정보를 검색하고 상호 작용하는 기술
3. Web site : 인터넷에서 여러 개의 Web page가 모인 것으로, 사용자들이 정보나 서비스를 제공하는 공간
4. Web page: HTML, CSS 등의 웹 기술을 이용하여 만들어진, Web site를 구성하는 하나의 요소
    - 구성요소
        - Structure : HTML
        - Styling : CSS
        - Behavior : Javascript

# 웹 구조화

## HTML

1. HTML : HyperText Markup Language, 웹 페이지의 의미와 구조를 정의하는 언어
2. Hypertext : 웹 페이지를 다른 페이지로 연결하는 링크, 참조를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
    - 특징 : 비선형성, 상호연결성, 사용자 주도적 탐색
3. Markup Language : 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어
    - (EX) HTML, Markdown

## Structure of HTML

1. 구조
    
    ```html
    <!DOCTYPE html>
    <html land="en">
    <head>
    	<title>My page</title>
    </head>
    <body></body>
    </html>
    ```
    
    - `<!DOCTYPE html>` : 해당 문서가 html로 작성된 문서라는 것을 나타냄
    - `<html></html>` : 전체 페이지의 콘텐츠를 포함
    - `<title></title>` : 브라우저 탭 및 즐겨찾기 시 표시되는 제목으로 사용
    - `<head></head>` :
        - HTML 문서에 관련된 설명, 설정 등 컴퓨터가 식별하는 메타데이터를 작성
        - 사용자에게 보이지 않음
    - `<body></body>` :
        - HTML 문서의 내용을 나타냄
        - 페이지에 표시되는 모든 콘텐츠를 작성
        - 한 문서에 하나의 body 요소만 존재
2. 요소 Element
    - 하나의 요소는 여는  태그와 닫는 태그, 그리고 그 안의 내용으로 구성됨
    - 닫는 태그는 태그 이름 앞에 슬래시(/)가 포함됨
    - 닫는 태그가 없는 태그도 존재
        - content가 필요 없는 태그면 닫는 태그가 없는게 보통
        - `<p>My cat is very qrumpy</p>` → My cat ~ 부분이 content
3. 속성 Attributes
    - 사용자가 원하는 기준에 맞도록 요소를 설정하거나 다양한 방식으로 요소의 동작을 조절하기 위한 값
    - 목적
        - 나타내고 싶지 않지만 추가적인 기능, 내용을 담고 싶을 때 사용
        - CSS 에서 스타일 적용을 위해 해당 요소를 선택하기 위한 값으로 활용됨
        - `<p class="editor-note">My cat is very grumpy</p>` → class 부분이 attribute
    - 작성 규칙
        - 속성은 요소 이름과 속성 사이에 공백이 있어야 함
        - 하나 이상의 속성들이 있는 경우엔 속성 사이에 **공백**으로 구분함
        - 속성 값은 열고 닫는 따옴표(쌍따옴표 권장)로 감싸야 함
4. 태그 종류
    - `<a href="url">링크 이름</a>` : 하이퍼텍스트 생성 태그
    - `<img src="소스" alt"대체 텍스트">` : 이미지 태그
        - 닫는 태그 X
        - alt는 필수는  X

## Text Structure

1. 설명 : HTML의 주요 목적 중 하나는 텍스트 구조와 **의미**를 제공하는 것
    - (EX) `<h1></h1>` : 단순히 텍스트를 크게만 만드는 것이 아닌, 현재 문서의 최상위 제목이라는 의미를 부여하는 것
2. 대표적인 HTML Text structure
    
    ```html
    <body>
    	<h1>Main Heading</h1>
    	<h2>Sub Heading</h2>
    	<p>This is my page</p>
    	<p>This is <em>emphasis</em></p>
    	<p>Hi <strong>my name</strong> is Air</p>
    	<ol>
    		<li>파이썬</li>
    		<li>알고리즘</li>
    		<li>웹</li>
    	</ol>
    </body>	
    ```
    
    - Heading & Paragraphs
        - `h1~6`
        - `p`
    - Lists
        - `ol` : ordered list
        - `ul` : unordered list
        - `li` : 자식
    - Emphasis & Importance
        - `em` : 기울어짐
        - `strong` : 볼드

## HTML 스타일 가이드

1. 대소문자 구분
    - HTML은 대소문자를 구분하지 않지만, 소문자 사용을 강력히 권장
    - 태그명과 속성명 모두 소문자로 작성
2. 속성 따옴표 : 큰 따옴표를 사용하는 것이 원칙
3. 공백 처리
    - 연속된 공백을 하나로 처리
    - Enter키로 줄 바꿈을 해도 브라우저에서 인식하지 않음 → 줄바꿈 태그를 사용해야 함
4. 에러 출력 없음
5. 코드 구조와 포맷팅
    - 일관된 들여쓰기를 사용
    - 선택자와 속성은 각각 새 줄에 작성
    - 중괄호 앞에 공백 넣기
    - 속성 뒤에는 콜론(:)과 공백 넣기
    - 마지막 속성 뒤에는 세미콜론(;) 넣기