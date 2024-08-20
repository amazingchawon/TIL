# CSS Layout

각 요소의 위치와 크기를 조정하여 웹 페이지의 디자인을 결정하는 것

- Display, Position, Flexbox 등

## Box Model

웹 페이지의 모든 HTML 요소를 감싸는 사각형 상자 모델

1. 박스 타입 : 박스 타입에 따라 페이지의 배치 흐름 및 다른 박스와 관련하여 박스가 동작하는 방식이 달라짐
    - Block box : 아래로 → 컨텐츠의 너비와 관계 없이 가로 전체 공간 차지
    - Inline : 옆으로 → 컨텐츠의 너비만큼 공간 차지
2. 박스 표기(Display) 타입
    - Outer display type
        - Block box
        - Inline box
    - Inner display type
        - Flexbox
3. 구성요소:
    - 내용 content
        - 실제 콘텐츠가 표시되는 영역의 크기
        - `width` / `height` 속성을 사용하여 크기 조정
    - 안쪽 여백 padding
        - 콘텐츠 주위의 공백
        - `padding` 관련 속성을 사용하여 크기 조정
    - 테두리 border
        - 콘텐츠와 패딩을 래핑
        - `border` 속성을 사용하여 크기 조정
    - 외부 간격 margin box
        - 콘텐츠, 패딩, 테두리를 개핑
        - 박스와 **다른 요소** 사이의 공백
        - `margin` 속성을 사용하여 크기 조정
4. shorthand 속성 (축약표기법)
    - `border`
        - width, style, color을 한번에 설정하기 위한 속성
        - 순서는 상관 X
    - `margin` / `padding`
        - 4방향의 속성을 각각 지정하지 않고 한번에 지정할 수 있는 속성
        - (EX) margin : 10px 20px 30px 40px → 상/우/하/좌
        - (EX) magint : 10px 20px 40px → 상/좌우/하
        - (EX) magint : 10px 20px → 상하/좌우
        - (EX) magint : 10px → 상하좌우 공통
5. `box-sizing` 속성
    - `content box` : 기존 상태
    - `border box` : border 기준 상태
        
        ```css
        * {
        	box-sizing: border-box;
        }
        ```
        

### 마진 상쇄

1. 설명 : 두 block 타입 요소의 margin top과 bottom이 만나 더 큰 margin으로 결합되는 현상
2. 이유 : 일관성, 단순화
    - 복잡한 레이아웃에서 요소 간 간격을 일관되게 유지하기 위함
    - 요소 간의 간격을 더 예측 가능하고 관리하기 쉽게 만듦

### Display 속성

박스 모델의 타입을 설정하는 속성

1. display type
    
    ```css
    .box {
    	display: inline-block;
    }
    ```
    
    - Block 속성
        - 항상 새로운 행으로 나뉨
        - width와 height 속성 사용 가능
            - width 속성을 지정하지 않으면 박스는 inline 방향으로 사용 가능한 공간을 모두 차지함 → 상위 컨테이너 너비 100%로 채우는 것
        - padding, margin, border로 인해 다른 요소를 상자로부터 밀어냄
        - 대표적인 block 타입 태그 : `h1~6`, `p`, `div`
    - Inline 속성
        - 새로운 행으로 넘어가지 않음
        - width와 height 속성 사용 불가
        - 수직방향
            - padding, margin, border가 적용되지만 다른 요소를 밀어낼 수는 없음
        - 수평방향
            - padding, margin, borders가 적용되어 다른 요소를 밀어낼 수 있음
        - 대표적인 inline 타입 태그 : `a`, `img`, `span`, `strong`, `em`
    - inline-block 속성
        - inline과 block 요소 사이의 중간 지점을 제공하는 display 값
        - width와 height 속성 사용 가능
        - padding, margin 및 border로 인해 다른 요소가 상자에서 밀려남
        - 새로운 행으로 넘어가지 않음
        
        → 요소가 줄 바꿈 되는 것을 원하지 않으면서 너비와 높이를 적용하고 싶을 때 사용
        
    - None 속성
        - 요소를 화면에 표시하지 않고, 공간조차 부여되지 않음
    
2. Normal flow : 일반적인 흐름 또는 레이아웃을 변경하지 않은 경우 웹 페이지 요소가 배치되는 방식

## Position

1. 정의 : 요소를 Normal Flow에서 제거하여 다른 위치로 배치하는 것
    - 다른 요소 위에 올리기, 화면의 특정 위치에 고정시키기 등
    - 이동 방향
        - top / left / right / bottom
        - z
2. Position 유형
    - `static`
        - 요소를 Normal Flow에 따라 배치
        - top, right, bottom, left 속성이 적용되지 않음
        - 기본 값
    - `relative` (상대 위치)
        - 요소를 Normal Flow에 따라 배치
        - 자신의 원래 위치`static`를 기준으로 이동
        - top, right, bottom, left 속성으로 위치를 조정
        - 다른 요소의 레이아웃에 영향을 주지 않음 (요소가 차지하는 공간은 `static` 일 때와 같음)
    - `absolute` (절대 위치)
        - 요소를 Normal Flow에서 제거
        - 가장 가까운 `relative` 부모 요소를 기준으로 이동
            - 만족하는 부모 요소가 없다면, body 태그를 기준으로 함
        - top, right, bottom, left 속성으로 위치를 조정
        - 문서에서 요소가 차지하는 공간이 없어짐
    - `fixed`
        - 요소를 Normal Flow에서 제거
        - 현재 화면영역(viewport)을 기준으로 이동
        - 스크롤해도 항상 같은 위치에 유지됨
        - top, right, bottom, left 속성으로 위치를 조정
        - 문서에서 요소가 차지하는 공간이 없어짐
        - 플로팅 버튼을 구현할 때 많이 쓰임
    - `sticky`
        - `relative`와 `fixed`의 특성을 결합한 속성
        - 스크롤 위치가 임계점에 도달하기 전에는 `relative`처럼 동작
        - 스크롤 위치가 특정 임계점에 도달하면 `fixed`처럼 동작하여 화면에 고정됨
        - 만약 다음 sticky 요소가 나오면 다음 sticky 요소가 이전 sticky 요소의 자리를 대체
            - 이전 sticky 요소가 고정되어 있던 위치와 다음 sticky 요소가 고정되야 할 위치가 겹치게 되기 때문
    - `z-index`
        - 설명
            - 요소의 쌓임 순서(stack order)를 정의하는 속성
            - 정수 값을 사용해 Z축 순서를 지정
            - 값이 클수록 요소가 위에 쌓이게 됨
            - `static`이 아닌 요소에만 적용됨
        - 특징
            - 기본 값 : auto → 나중에 쓴 것이 위로 올라감
            - 부모 요소의 `z-index` 값에 영향을 받음
            - 같은 부모 내에서만 `z-index` 비교
            - 부모의 `z-index`가 낮으면 자식의 `z-index`가 아무리 높아도 부모보다 위로 올라갈 수 없음
            - `z-index` 값이 같으면 HTML 문서 순서대로 쌓임
3. Position의 목적
    
    전체 페이지에 대한 레이아웃을 구성하는 것보다는 **페이지 특정 항목의 위치를 조정**하는 것
    

## Flexbox

요소를 행과 열 형태로 배치하는 1차원 레이아웃 방식 → 공간 배열 & 정렬 (웹 페이지 큰 그림)

1. 구성요소
    - Flex Container
        - `display : flex;` 혹은 `display: inline-flex;` 가 설정된 부모 요소
        - 이 컨테이너의 1차 자식 요소들이 Flex item이 됨 (Flex item을 지정하는 설정은 없음)
        - flexbox 속성 값들을 사용하여 자식 요소 Flex ltem들을 **배치하는 주체**
    - Flex item : Flex Container 내부에 레이아웃 되는 항목
    - main axis : 주축, flex item들이 배치되는 기본 축
        - main start → main end : (기본 값) 왼쪽 → 오른쪽
    - cross axis : 교차축, main axis에 수직인 축
        - cross start → cross end : (기본 값) 위쪽 → 아래쪽
2. 속성
    - Flex Container 관련
        - `display` : Flex Container 지정
        - `flex-direction` : flex item이 나열되는 방향을 지정
            - row, column, row-reverse, column-reverse
            - column으로 지정할 경우 주 축이 변경됨
            - ‘-reverse’로 지정하면 flex item 배치의 시작 선과 끝 선이 서로 바뀜
        - `flex-wrap` : flex item 목록이 flex container의 한 행에 들어가지 않은 경우, 다른 행에 배치할지 여부 설정
            - nowrap(기본), wrap
        - `justify-content` : 주 축을 따라 flex item과 주위에 공간을 분배
            - flex-start, center, flex-end
        - `align-content` : 교차 축을 따라 flex item과 주위에 공간을 분배
            - flex-wrap이 wrap 또는 wrap-reverse로 설정된 여러 행에만 적용됨
            - 한 줄짜리 행에는 효과 X (flex-wrap이 nowrap으로 설정된 경우)
        - `align-items` : 교차 축을 따라 flex item 행을 정렬, 한 행씩
    - Flex Item 관련 속성
        - `align-self` : 교차 축을 따라 **개별 flex item**을 정렬, 한 개씩
        - `flex-grow` : 남는 행 여백을 비율에 따라 각 flex item에 분배
            - 아이템이 컨테이너 내에서 확장하는 비율을 지정
                - flex-grow 값이 1인 item의 2배가 2인 item의 크기가 아니다!!
                - 남는 여백을 균등하게 나누어서 그 몫을 flex-grow 값에 따라 주는 것
            - ↔ `flex-shrink`
        - `flex-basis` : flex item의 초기 크기 값을 지정
            - flex-basis와 width 값을 동시에 적용한 경우 flex-basis가 우선
3. 속성 - 기능 분류
    - 배치
        - flex-direction
        - flex-wrap
    - 공간 분배
        - justify-content
        - align-content
    - 정렬
        - align-items
        - align-self
4. `flex-wrap` 응용 
    - 반응형 레이아웃 : 다양한 디바이스와 화면 크기에 자동으로 적응하여 콘텐츠를 최적으로 표시하는 웹 레이아웃 방식