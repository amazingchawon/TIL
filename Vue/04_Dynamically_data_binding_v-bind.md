# Dynamically data binding : v-bind

1. 설명 : 하나 이상의 속성 또는 컴포넌트 데이터를 표현식에 동적으로 바인딩
2. 사용처 :
    - Attribute Bindings
    - Class and Style bindings

## 속성 바인딩 Attribute Bindings

1. HTML의 속성 값을 Vue의 상태 속성 값과 동기화 되도록 함
    
    ```html
    <img v-bind:src="imageSrc">
    <a v-bind:href="myUrl">Move to url</a>
    ```
    
2. v-bind shorthand `:`
    
    ```html
    <img :src="imageSrc">
    <a :href:"myUrl">Move to url</a>
    ```
    
3. 동적 인자 이름 Dynamic attribute name
    - 대괄호로 감싸서 directive argument에 JavaScript 표현식을 사용할 수 있음
    - 표현식에 따라 동적으로 평가된 값이 최종 argument 값으로 사용됨
    - 대괄호 안에 작성하는 이름은 반드시 소문자로만 구성 가능(브라우저가 속성 이름을 소문자로 강제 변환하기 때문
    
    ```html
    <button :[key]="myValue"></button>
    ```
    

## 클래스와 스타일 바인딩 ****Class and Style Bindings

1. 설명 : 
    - class와 style은 모두 HTML 속성 → 다른 속성과 마찬가지로 v-bind를 사용하여 동적으로 문자열을 할당할 수 있음
    - Vue는 class 및 style 속성 값을 v-bind로 사용할 때, **객체 또는 배열을 활용**하여 작성할 수 있도록 함
        - 단순히 문자열 연결을 사용하여 이러한 값을 생성하는 것은 번거롭고 오류 발생이 쉽기 때문
2. Class and Style bindings가 가능한 경우
    - Binding **HTML** classess
    - Binding Inline **Styles**
3. Binding **HTML** classess
    - Binding to Objects :
        - 객체를 `:class` 에 전달하여 클래스를 동적으로 전환할 수 있음
            - 예시 :
                
                ```html
                const isActive = ref(false)
                
                <div :class="{ active: isActive }">Text</div>
                ```
                
                - isActive의 Boolean 값에 의해 active 클래스의 존재가 결정 됨
        - 객체에 더 많은 필드를 포함하여 여러 클래스를 전환할 수 있음
            - 예시 :
                
                ```html
                const isActive = ref(false)
                const hasInfo = ref(true)
                
                <dic class="static" :class="{ active: isActive, 'text-primary': hasInfo }">Text</div>
                
                <div class="static text-primary">Text</div>
                ```
                
                - :class directive를 일반 클래스 속성과 함께 사용 가능
        - 반응형 변수를 활용해 객체를 한번에 작성하는 방법 (반드시 inline 방식으로 작성하지 않아도 됨)
            - 예시 :
                
                ```jsx
                const isActive = ref(false)
                const hasInfo = ref(true)
                
                // ref는 반응 객체의 속성으로 엑세스되거나 변경될 때 자동으로 unwrap
                const classObj = ref({
                	active: isActive,
                	'text-primary': hasInfo
                })
                ```
                
                ```html
                <div class="static" :class="classObj">Text</div>
                ```
                
    - Binding to Arrays
        - `:class` 를 배열에 바인딩하여 클래스 목록을 적용 가능
            - 예시 :
                
                ```jsx
                const activeClass = ref('active')
                const infoClass = ref('text-primary')
                ```
                
                ```html
                <div :class="[activeClass, infoClass]">Text</div>
                
                <div class="active text-primary">Text</div>
                ```
                
        - 배열 구문 내에서 객체 구문 사용 가능
            - 예시 :
                
                ```jsx
                <div :class="[{ active: isActive }, infoClass]">Text</div>
                ```
                
4. Binding Inline **Styles**
    - Binding to Objects
        - `:style` 은 JavaScript 객체 값에 대한 바인딩을 지원
            - 예시 :
                
                ```jsx
                const activeColor = ref('crimson')
                const fontSize = ref(50)
                ```
                
                ```html
                <div :style="{ color: activeColor, fontSize: fontSize + 'px'}">Text</div>
                <!--kebab-cased 형태-->
                <div :style="{ color: activeColor, font-size: fontSize + 'px'}">Text</div>
                <div style="color: crimson; font-size: 50px;">Text</div>
                ```
                
                - 실제 CSS에서 사용하는 것처럼 `:style` 은 kebab-cased 키 문자열도 지원
                    - 단, camelCase 작성을 권장
        - 반응형 변수를 활용해 객체를 한번에 작성하는 방법(반드시 inline 방식으로 작성하지 않아도 됨)
            - 예시 :
                
                ```jsx
                const styleObj = ref({
                	color: activeColor,
                	fontSize: fontSize.value + 'px'
                })
                ```
                
                ```html
                <div style="color:crimson; font-size: 50px;">Text</div>
                ```
                
    - Binding to Arrays
        - 여러 스타일 객체를 배열에 작성해서 `:style` 바인딩 가능
            - 예시 :
                
                ```jsx
                const stytleObj2 = ref({
                	color: 'blue',
                	border: '1px solid black'
                })
                ```
                
                ```html
                <div :style="[styleObj, styleObj2]">Text</div>
                
                <div style="color: blue; font-size: 50px; border: 1px solid black;">
                ```