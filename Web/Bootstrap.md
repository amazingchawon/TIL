# [Bootstrap](https://getbootstrap.com/)

CSS 프론트엔드 프레임워크(Toolkit) → 미리 만들어진 다양한 디자인 요소들을 제공하여 웹 사이트를 빠르고 쉽게 개발할 수 있도록 함

1. CDN Content Delivery Network : 지리적 제약 없이 빠르고 안전하게 콘텐츠를 전송할 수 있는 전송 기술
    - 서버와 사용자 사이의 물리적인 거리를 줄여 콘텐츠 로딩에 소요되는 시간을 최소화 (웹 페이지 로드 속도를 높임)
    - 지리적으로 사용자와 가까운 CDN 서버에 콘텐츠를 저장해서 사용자에게 전달

## Bootstrap 사용 가이드

1. 사용법 :
    - Bootstrap에는 특정한 규칙이 있는 클래스 이름으로 스타일 및 레이아웃이 미리 작성되어 있음
2. Bootstrap에서 클래스 이름으로 Spacing을 표현하는 방법
    
    
    | Property |  | Sides |  | size |  |  |
    | --- | --- | --- | --- | --- | --- | --- |
    | m | margin | t | top | 0 | 0r em | 0px |
    | p | padding | b | bottom | 1 | 0.25 rem | 4px |
    |  |  | s | left | 2 | 0.5 rem | 8px |
    |  |  | e | right | 3 | 1 rem | 16px |
    |  |  | y | top, bottom | 4 | 1.5 rem | 24 px |
    |  |  | x | left, right | 5 | 3 rem | 48px |
    |  |  | blank | 4 sides | auto | auto | auto |

# Reset CSS

1. 정의 : 모든 HTML 요소 스타일을 일관된 기준으로 재설정하는 간결하고 압축된 규칙 세트
    - HTML Element, Table, List 등의 요소들에 일관성 있게 스타일을 적용 시키는 기본 단계
2. 사용 배경
    - 모든 브라우저는 각자의 user agent stylesheet를 가지고 있음
        - user agent stylesheets : 모든 문서에 기본 스타일을 제공하는 스타일 시트
        - 용도 : 웹 사이트를 보다 읽기 편하게 하기 위해
        - 문제점 발생 → 이 설정이 브라우저마다 상이
    - 모든 브라우저에서 웹사이트를 동일하게 보여야 함 → 모두 똑같은 스타일 상태로 만들고 스타일 개발을 시작
3. Normalize CSS
    - Reset CSS 방법 중 대표적인 방법
    - 웹 표준 기준으로 브라우저 중 하나가 불일치 한다면 차이가 있는 브라우저를 수정하는 방법
        - 경우에 따라 IE 또는 EDGE 브라우저는 표준에 따라 수정할 수 없음, 이 경우 IE 또는 EDGE의 스타일을 나머지 브라우저에 적용시킴
4. Bootstrap에서의 Reset CSS
    - bootstrap-reboot.css 파일 : normalize.css를 자체적으로 커스텀하여 사용

<br></br>

# Bootstrap 활용

## Typography

제목, 본문 텍스트, 목록 등

## Colors

Bootstrap이 지정하고 제공하는 색상 시스템

## Component

Bootstrap에서 제공하는 UI 관련 요소(버튼, 네비게이션 바, 카드, 폼, 드롭다운 등)

1. 주의 사항 : 
    - carousel id 속성 값과 각 버튼의 data-bs-target 속성 값이 각각 올바르게 일치하는지 확인
        - carousel 뿐만 아니라, 움직이는 컴포넌트!
    - Modal에서 버튼과 모달의 위치 확인
        - 버튼과 모달 본체가 함께 다닐 필요 없음
        - 모달 본체는 버튼을 눌러야만 활성화되는 코드이기 때문에 코드 최하단에 모아두는 것을 권장
        - 모달을 레이아웃 깊은 곳에 넣어두었다가 딤처리 된 화면 뒤에 생기는 경우 존재 → 모달을 끄지 못함
2. 이점 : 일관된 디자인을 제공하여 웹 사이트의 구성 요소를 구축하는데 유용하게 활용

# Semantic Web

웹 데이터를 의미론적으로 구조화된 형태로 표현하는 방식

1. HTML → 구조 + 의미
2. 이 요소가 시작적으로 어떻게 보여질까 → 이 요소가 가진 목적과 역할은 무엇일까

## Semantic in HTML

1. HTML 요소가 의미를 가진다는 것
    
    ```html
    <!--의미 X-->
    <p style="font-size: 30px;">Heading</p>
    <!--의미 O-->
    <h1>Heading</h1> 
    ```
    
2. HTML Semantic Element
    - 기본적인 모양과 기능 이외에 의미를 가지는 HTML 요소
        - 검색엔진 및 개발자가 웹 페이지 콘텐츠를 이해하기 쉽도록
    - 대표적인 Semantic Element
        - header
        - nave
        - main
        - article
        - section
        - aside
        - footer

## Semantic in CSS

1. CSS 방법론 : CSS를 효율적이고 유지 보수가 용이하게 작성하기 위한 일련의 가이드라인
2. OOCSS Object Oriented CSS : 객체 지향적 접근법을 적용하여 CSS를 구성하는 방법론
    - 기본 원칙
        - 구조와 스킨을 분리
            
            ```css
            /*bad*/
            
            .blue-button {
            	border : none;
            	font-size : 1em;
            	padding: 10px 20px;
            	background-color: blue;
            	colot: white;
            }
            ```
            
            ```css
            /*good*/
            .button {
            	border: none;
            	font-size: 1em;
            	padding: 10px 20px;
            }
            
            .button-blue {
            	background-color: blue;
            	color: white;
            }
            ```
            
        - 컨테이너와 콘텐츠를 분리
            - 객체에 직접 적용하는 대신 객체를 둘러싸는 컨테이너에 스타일을 적용
            - 스타일을 정의할 때 위치에 의존적인 스타일을 사용하지 않도록 함
            - 콘텐츠를 다른 컨테이너로 이동시키거나 재배치할 때 스타일이 깨지는 것을 방지

# 참고

1. Bootstrap을 사용하는 이유
    - 가장 많이 사용되는 CSS 프레임워크
    - 사전에 디자인된 다양한 컴포넌트 및 기능
        - 빠른 개발과 유지보수
    - 손쉬운 반응형 웹 디자인 구현
    - 커스터마이징(customizing)이 용이
    - 크로스 브라우징(cross browsing) 지원
        - 모든 주요 브라우저에서 작동하도록 설계되어 있음
2. CDN 없이 사용하기
    - Bootstrap 코드 파일 다운로드
    - bootstrap.css 와 bootstrap.bundle.js만 선택
    - CSS 파일은 HTML head 태그에 가져와서 사용
    - JS 파일은 HTML body 태그에 가져와서 사용 (바디 태그 닫기 전 바로 위에)

1. 의미론적인 마크업이 필요한 이유
    - 검색엔진 최적화 SEO
        - 검색 엔진이 해당 웹 사이트를 분석하기 쉽게 만들어 검색 순위에 영향을 줌
    - 웹 접근성 Web Accessibility
        - 웹 사이트, 도구, 기술이 고령자나 장애를 가진 사용자들이 사용할 수 있도록 설계/개발 하는 것
        - naver - nuli (널리)