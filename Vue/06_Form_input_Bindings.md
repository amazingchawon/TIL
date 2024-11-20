# Form Input Bindings

1. 설명 :
    - form을 처리할 때 사용자가 input에 입력하는 값을 실시간으로 JavaScript 상태에 동기화해야 하는 경우(양방향 바인딩)
2. 방법
    - `v-bind`와 `v-on`을 함께 사용
    - `v-model` 사용

## v-bind with v-on

1. 방법 :
    - v-bind를 사용해서 input 요소의 value 속성 값을 입력 값으로 사용
    - v-on을 사용해서 input 이벤트가 발생할 때마다 input 요소의 value 값을 별도 반응형 변수에 저장하는 핸들러 호출
2. 코드 :
    
    ```jsx
    const inputText1 = ref('')
    const onInput = function (event) {
    	inputText1.value = event.currentTarget.value
    }
    ```
    
    ```html
    <p>{{ inputText1 }}</p>
    <input :value="inputText1" @input="onInput">
    ```
    

## v-model

1. 설명 : form input 요소 또는 컴포넌트에서 양방향 바인딩을 만듦
2. 코드 :
    
    ```jsx
    const inputText2 = ref('')
    ```
    
    ```html
    <p>{{ inputText2 }}</p>
    <input v-model="inputText2">
    ```
    
3. 사용 :
    - 사용자 입력 데이터와 반응형 변수를 실시간 동기화
    - IME가 필요한 언어(한국어, 중국어, 일본어)으 경우 v-model이 제대로 업데이트되지 않음 → v-bind와 v-on 방법을 사용 해야 함

### 활용

1. 설명 : Checkbox, Radio. Select 등 다양한 타입의 사용자 입력 방식과 함께 사용 가능
2. Checkbox 활용
    - 단일 checkbox와 boolean 값 활용
    
    ```jsx
    const checked = ref(false)
    ```
    
    ```html
    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>
    ```
    
- 다중 체크박스와 배열 활용
    
    ```jsx
    const checkedNames = ref([])
    ```
    
    ```html
    <div>Checked names: {{ checkedNames }}</div>
    
    <input type="checkbox" id="alice" value="Alice" v-model="checkedNames">
    <label for="alice">Alice</label>
    
    <input type="checkbox" id="bella" value="Bella" v-model="checkedNames">
    <label for="bella">Bella</label>
    ```
    
1. Select 활용
    - select에서 v-model 표현식의 초기 값이 어떤 option과도 일치하지 않는 경우 select 요소는 “unselected”상태로 렌더링 됨
    
    ```jsx
    	const selected = ref('')
    ```
    
    ```html
    <div>Selected: {{ selected }}</div>
    
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option>Alice</option>
          <option>Bella</option>
          <option>Cathy</option>
        </select>
    ```