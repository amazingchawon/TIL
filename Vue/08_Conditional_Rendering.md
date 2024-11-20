# Conditional Rendering

## v-if

1. 설명 : 표현식 값의 true/false를 기반으로 요소를 조건부로 렌더링
2. 활용 :
    - if else
        
        ```html
        <!-- if else -->
        <p v-if="isSeen">true일때 보여요</p>
        <p v-else>false일때 보여요</p>
        <button @click="isSeen = !isSeen">토글</button>
        ```
        
        - v-else directive를 사용하여 v-if에 대한 else 블록을 나타낼 수 있음
    - else if
        
        ```html
        <!-- else if -->
        <div v-if="name === Alice">Alice입니다</div>
        <div v-else-if="name === Bella">Bella입니다</div>
        <div v-else-if="name === Cathy">Cathy입니다</div>
        <div v-else>아무도 아닙니다.</div>
        ```
        
        - 화면에 그려지지 않음
    
    ```html
    <!-- v-if on <template> -->
    <template v-if="name === "Cathy">
      <div>Cathy입니다</div>
      <div>나이는 30살입니다</div>
    </template>
    ```
    
    - template은 최종 레이아웃에 출력되지 않음
        - 페이지가 로드될 때 렌더링 되지 않지만, JavaScirpt를 사용하여 나중에 문서에서 사용할 수 있도록 하는 HTML을 보유하기 위한 메커니즘
        - 보이지 않는 wrapper 역할
    
    ```html
    <!-- v-show -->
    <div v-show="isShow">v-show</div>
    ```
    
    - 화면에 그렸지만, css display none이 적용됨

## v-show

1. 설명 : 표현식 값의 true/false를 기반으로 요소의 가시성을 전환
2. 특징 :
    - v-show 요소는 항상 DOM에 렌더링 되어있음
    - CSS display 속성만 전환하기 때문

## v-if vs v-show

| v-if | v-show |
| --- | --- |
| cheap initial load, expensive toggle | expensive initial load, cheap toggle |
| - 초기 조건이 false인 경우 아무 작업도 수행하지 않음<br>- 토글 비용이 높음 (매번 지우고, 그리고 하니까) | - 초기 조건에 관계 없이 항상 렌더링<br>- 초기 렌더링 비용이 더 높음 |
| 콘텐츠를 자주 전환해야하는 경우 | 실행 중 조건이 변경되지 않는 경우 사용 |