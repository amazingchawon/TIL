# 웹 스타일링

## CSS

1. CSS : Cascading Style Sheet, 웹 페이지의 디자인과 레이아웃을 구성하는 언어
2. 구문
    
    ```css
    h1 {
    	color : red;
    	font-size: 30px;
    }
    ```
    
    - 선택자 Selector : `h1`
    - 속성 Property : `font-size`
    - 값 Value : `30px`
    - 선언 Declaration : `{}`
3. 적용 방법
    - 인라인 Inline 스타일
        - HTML 요소 안에 style 속성 값으로 작성
        - 사용 빈도 낮음
    - 내부 Internal 스타일 시트
        - `head` 태그 안 `style` 태그에 작성
    - 외부 External 스타일 시트
        - 별도 CSS 파일 생성 후 HTML `head`에서 `link` 태그를 사용해 불러오기

## Selectors 선택자

1. 정의 : HTML 요소를 선택하여 스타일을 적용할 수 있도록 하는 선택자
2. 종류:
    - 기본 선택자
        - 전체 선택자 `*` : HTML 모든 요소를 선택
        - 요소(tag) 선택자 : 지정한 태그의 모든 요소 선택
        - 클래스(class) 선택자 `.` : 주어진 클래스 속성을 가진 모든 요소를 선택
            
            ```css
            .classname {color : green;}
            ```
            
        - 아이디(id) 선택자 `#` :
            - 주어진 아이디 속성을 가진 요소 선택
            - 문서에는 주어진 아이디를 가진 요소가 **하나만** 있어야 함
        - 속성(attr) 선택자
    - 결합자 Combinators
        - 자손 결합자 `" "` (space)
            - 첫 번째 요소의 전체 자손 요소들 선택
        - 자식 결합자 `>`
            - 첫 번째 요소의 직계 자식만 선택

## Specificity 명시도

1. 명시도 : 결과적으로 요소에 적용할 CSS 선언을 결정하기 위한 알고리즘
    - CSS Selector에 가중치를 계산하여 어떤 스타일을 적용할지 결정
    - 동일한 요소를 가리키는 2개 이상의 CSS 규칙이 있는 경우, 가장 높은 명시도를 가진 Seletor가 승리하여 스타일이 적용됨
    - Cascade : 폭포
2. 명시도가 높은 순
    - Importance `!important` :
        - 다른 우선순위 규칙보다 우선하여 적용하는 키워드
        - Cascade의 구조를 무시하고 강제로 스타일을 적용하는 방식이므로 사용을 권장하지 않음
    - Inline 스타일
    - 선택자  : id > class > 요소 → 주로 class 사용
    - 소스 코드 선언 순서 (나중에 선언한 것이 더 명시도가 높음)

## 상속

1. CSS 속성 분류
    - 상속되는 속성
        - Text 관련 요소 (font, color, text-align), opacity, visibility 등
    - 상속 되지 않는 속성
        - Box model 관련 요소 (width, height, border, box-sizing, ..)
        - position 관련 요소 (position, top/right/bottom/left, z-index)
2. CSS 상속 여부 확인 : MDN의 각 속성별 문서 하단에서 상속 여부를 확인할 수 있음

## CSS Box Model

웹 페이지의 모든 HTML 요소를 감싸는 사각형 상자 모델

1. 박스 표기(Display) 타입
    - Outer display type
        - Block box
        - Inline box
    - Inner display type
        - Flexbox

### Outer display type

1. 박스 타입
    - Block box : 아래로
        - 항상 새로운 행으로 나뉨
        - width와 height 속성 사용 가능
            - width 속성을 지정하지 않으면 박스는 inline 방향으로 사용 가능한 공간을 모두 차지함 → 상위 컨테이너 너비 100%로 채우는 것
        - padding, margin, border로 인해 다른 요소를 상자로부터 밀어냄
        - 대표적인 block 타입 태그 : `h1~6`, `p`, `div`
    - Inline box : 오른쪽으로
        - 새로운 행으로 넘어가지 않음
        - width와 height 속성 사용 불가
        - 수직방향
            - padding, margin, border가 적용되지만 다른 요소를 밀어낼 수는 없음
        - 수평방향
            - padding, margin, borders가 적용되어 다른 요소를 밀어낼 수 있음
        - 대표적인 inline 타입 태그 : `a`, `img`, `span`, `strong`, `em`
    - cf ) 박스 타입에 따라 페이지에서의 배치 흐름 및 다른 박스와 관련하여 박스가 동작하는 방식이 달라짐