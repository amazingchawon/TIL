# Template Syntax

1. 설명 : DOM을 기본 구성 요소 인스턴스의 데이터에 선언적으로 바인딩(Vue Instance와 DOM을 연결)할 수 있는 HTML 기반 템플릿 구문(확장된 문법 제공)을 사용

## 종류

1. Text Interpolation
    
    ```html
    <p>Message: {{ msg }}</p>
    ```
    
    - 데이터 바인딩의 가장 기본적인 형태
    - 이중 중괄호 구문(콧수염 구문)을 사용
    - 콧수염 구문은 해당 구성 요소 인스턴스의 msg 속성 값으로 대체
    - msg 속성이 변경될 때마다 업데이트 됨
2. Raw HTML
    
    ```html
    <div v-html="rawHtml"></div>
    
    const rawHtml = ref('<span style="color:red">This should be red</span>
    ```
    
    - 콧수염 구문은 데이터를 일반 텍스트로 해석하기 때문에 실제 HTML을 출력하려면 v-html을 사용해야 함
3. Attribute Bindings
    
    ```html
    <div v-bind:id="dynamicId"></div>
    
    const dynamicId = ref('my-id')
    ```
    
    - 콧수염 구문은 HTML 속성 내에서 사용할 수 없기 때문에 v-bind를 사용
    - HTML의 id 속성 값을 vue의 dynamicId 속성과 동기화 되도록 함
    - 바인딩 값이 null이나 undefined인 경우 렌더링 요소에서 제거됨
4. JavaScript Expressions
    
    ```html
    {{ number + 1 }}
    {{ ok ? 'YES' : 'NO' }}
    {{ message.split('').reverse.join('') }}
    
    <div v-bind:id="list-${id}`"></div>
    ```
    
    - Vue는 모든 데이터 바인딩 내에서 JavaScript 표현식의 모든 기능을 지원
    - Vue 템플릿에서 JavaScript 표현식을 사용할 수 있는 위치
        - 콧수염 구문 내부
        - 모든 directive 속성 값( `v-`로 시작한 특수 속성)
    - 주의사항
        - 각 바인딩에는 하나의 단일 표현식만 포함될 수 있음
            - 표현식 값으로 평가할 수 있는 코드 조각(return 뒤에 사용할 수 있는 코드여야 함)
        - 작동하지 않는 경우
            - 표현식이 아닌 선언식
            - 제어문은 삼항 표현식으로

## Directive

1. 설명 : `v-` 접두사가 있는 특수 속성
2. 특징 :
    - Directive의 속성 값은 **단일 JavaScript 표현식**이어야 함
        - `v-for` , `v-on` 제외
    - 표현식 값이 편경될 때 DOM에 반응적으로 업데이트를 적용
    - 예시 :
        
        ```html
        <p v-if="seen">Hi There</p>
        ```
        
3. 전체 구문:
    
    ```html
    v-on:submit.pervent="onSubmit"
    ```
    
    - `v-on` : Name, Starts with v-, May be omitted when using shorthands
    - `submit` : Argument, Follows tje colon or shorthand symbol
        - 일부 directive는 diretive 뒤에 콜론으로 표시되는 인자를 사용할 수 있음
        - 예시: href는 HTML <a> 요소의 href 속성 값을 myUrl 값에 바인딩하도록 하는 v-bind 인자
            
            ```html
            <a v-bind:href="myUrl">
            ```
            
        - 예시 : click은 이벤트 수신할 이벤트 이름을 작성하는 v-on 인자
            
            ```html
            <button v-on:click="doSomething">
            ```
            
    - `prevent` : Modifiers, Denoted by the leading dot
        - `.(dot)` 으로 표현되는 특수 접미사로, diretive가 특별한 방식으로 바인딩되어야 함을 나타냄
        - 예시: .prevent는 발생한 이벤트에서 event.preventDefault()를 호출하도록 v-on에 지시하는 modifier
            
            ```html
            <form v-on:submit.prevent="onSubmit">
            	<input type="submit">
            </form>
            ```
            

5. Built-in Directives

- https://vuejs.org/api/built-in-directives.html#built-in-directives