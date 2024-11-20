# Event Handling

## v-on

1. 설명 : DOM 요소에 이벤트 리스너를 연결 및 수신
2. 구성 :
    
    ```jsx
    v-on:submit.pervent="onSubmit"
    ```
    
    - handler 종류
        - Inline handlers : 이벤트가 트리거 될 때 실행 될 JavaScript 코드
        - Method handlers : 컴포넌트에 정의된 메서드 이름
    - v-on shorthand `@`
3. Inline handlers
    - Inline handlers는 주로 간단한 상황에 사용
        - 예시 :
            
            ```jsx
            const count = ref(0)
            ```
            
            ```html
            <button @click="count++">Add 1</button>
            <p>Count: {{ count }}</p>
            ```
            
    - 메서드 호출
        - 메서드 이름에 직접 바인딩하는 대신 Inline Handlers에서 메서드를 호출할 수 있음
        - 이렇게 하면 기본 이벤트 대신 사용자 지정 인자를 전달할 수 있음
        - 예시 :
            
            ```jsx
            const greeting = function (message) {
            	console.log(message)
            }
            ```
            
            ```html
            <button @click="greeting('hello')">Say hello</button>
            <button @click="greeting('bye')">Say bye</button>
            ```
            
    - event 인자에 접근하기
        - Inline Handlers에서 원래 DOM 이벤트에 접근하기
        - $event 변수를 사용하여 메서드에 전달
        - 예시 :
            
            ```jsx
            const warning = funciton (message, event) {
            	console.log(message)
            	console.log(event)
            }
            ```
            
            ```html
            <button @click="warining('경고입니다.', $event)">Submit</button>
            ```
            
4. Method Handlers
    - 설명 :
        - Inline handlers로는 불가능한 대부분의 상황에서 사용
        - Method Handlers는 이를 틔거하는 기본 DOM Event 객체를 자동으로 수신

## Modifiers

1. 설명 :
    - Event Modifiers를 활용해 event.preventDefault()와 같은 구문을 메서드에 작성하지 않도록 함
    - stop, prevent, self 등 다양한 modifiers 제공
    - 메서드는 DOM 이벤트에 대한 처리보다 데이터에 관한 논리를 작성하는 것에 집중할 것
    - 예시 :
        
        ```html
        <form @submit.prevent="onSubmit">...<.form>
        <a @click.stop.prevent=”onLink”>...</a>
        ```
        
        - Modifiers는 chained 되게끔 작성할 수 있으며 이때는 작성된 순서로 실행되기 때문에 작성 순서에 유의
2. Key Modifiers
    - 설명 : 키보드 이벤트를 수신할 때 특정 키에 관한 별도 modifiers를 사용할 수 있음
    - 예시 :
        
        ```html
        <input @keyup.enter="onSubmit">
        ```
        
        - key가 Enter일 때만 onSubmit 이벤트를 호출하기