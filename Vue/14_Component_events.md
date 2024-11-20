# Component Events

## $emit()

1. 설명 : 자식 컴포넌트가 이벤트를 발생시켜 부모 컴포넌트로 데이터를 전달하는 역할의 메서드
    - $ 표기는 Vue 인스턴스 내부 변수들을 가리킴
    - Life cycle hooks, 인스턴스 메서드 등 내부 특정 속성에 접근할 때 사용
2. 구조 :
    
    ```html
    $emit(event, ...args)
    ```
    
    - event : 커스텀 이벤트 이름
    - args : 추가 인자

### 이벤트 발신 및 수신

1. 발신 : $emit을 사용하여 템플릿 표현식에 직접 사용자 정의 이벤트를 발신
    
    ```html
    <button @click="$emit('someEvent')">클릭</button>
    ```
    
    - JavaScript → CamelCase
2. 수신 : 부모는 v-on을 사용하여 수신할 수 있음
    
    ```html
    <ParentComp @some-event="someCallback" />
    ```
    
    - HTML → kabab-case
3. 실습 : ParentChild에서 버튼을 누르면 Parent에서 감지
    - ParentChild
        
        ```html
        <template>
          <div>
            ...
            <button @click="$emit('someEvent')">클릭</button>
          </div>
        </template>
        ```
        
    - Parent
        
        ```html
        <template>
          <div>
            <ParentChild
              ...
              @some-event="someCallback"
            />
          </div>
        </template>
        
        <script setup>
        const someCallback = function () {
          console.log('자식 컴포넌트에서 발생한 이벤트를 수신했어요.')
        }
        </script>
        
        <style scoped>
        
        </style>
        ```
        

### emit 이벤트 선언

1. defineEmits() 사용
    - props와 마찬가지로 defineEmits()에 작성하는 인자의 데이터 타입에 따라 선언 방식이 나뉨
        - 배열 or 객체
    - defineEmits()는 $emit 대신 사용할 수 있는 동등한 함수를 반환
        - script에서는 $emit 메서드를 접근할 수 없기 때문
2. 실습 : 위 실습과 결과는 동일
    - ParentChild
        
        ```html
        <template>
          <div>
            ...
             <button @click="buttonClick">클릭</button>
             <button @click="emitArgs">어쩌구</button>
          </div>
        </template>
        
        <script setup>
        ...
        // emit 이벤트 선언
        const emit = defineEmits(['someEvent'])
        
        const buttonClick = function () {
          emit('someEvent')
        }
        </script>
        ```
        

### emit 이벤트 전달

1. 이벤트 인자 Event Arguments
    - 이벤트 발신 시 추가 인자를 전달하여 값을 제공할 수 있음
2. 실습 :
    - ParentChild
        
        ```html
        <template>
          <div>
            ...
             <button @click="buttonClick">출력</button>
             <button @click="emitArgs">추가 인자 전달</button>
          </div>
        </template>
        
        <script setup>
        import ParentGrandChild from '@/components/ParentGrandChild.vue'
        
        const emit = defineEmits(['someEvent', 'emitArgs'])
        
        ...
        const emitArgs = function () {
          emit('emitArgs', 1, 2, 3)
        }
        
        </script>
        ```
        
    - Parent
        
        ```html
        <template>
          <div>
            <ParentChild
              ...
              @emit-args="getNumbers"
            />
          </div>
        </template>
        
        <script setup>
        	...
        	const getNumbers = function (...args) {
        	  console.log(args) // [1, 2, 3]
        	}
        </script>
        
        <style scoped>
        
        </style>
        ```
        

### 이벤트 세부사항 : Event Name Casing

1. 선언 및 발산시 → camelCase
2. 부모 컴포넌트에서 수신 시 → kebab-case

## emit 이벤트 실습

1. 실습 내용 : 최하단 컴포넌트 ParentGrandChild에서 Parent 컴포넌트의 name 변수 변경 요청하기
    - emit은 단계를 뛰어넘을 수 없음
    - Parent ← emit ← ParentChild ← emit ← ParentGrandChild
2. ParentGrandChild
    
    ```html
    <template>
      <div>
        <p>{{ myMsg }}</p>
        <button @click="updateName">이름 변경!</button>
      </div>
    </template>
    
    <script setup>
      defineProps({
        myMsg: String
      })
    
      const emit = defineEmits(['updateName'])
    
      const updateName = function () {
        emit('updateName')
      }
    </script>
    ```
    
3. ParentChild
    
    ```html
    <template>
      <div>
        <p>{{ myMsg }}</p>
        <p>{{ dynamicProps }}</p>
        <ParentGrandChild 
          :my-msg="myMsg"
          @update-name="updateName"
        />
        ...
      </div>
    </template>
    
    <script setup>
    	...
    	const updateName = function () {
    	  emit('updateName')
    	}
    </script>
    ```
    
    - emit 이름이 같을 필요는 X
4. Parent
    
    ```html
    <template>
      <div>
        <ParentChild
          my-msg="message"
          :dynamic-props="name"
          @some-event="someCallback"
          @emit-args="getNumbers"
          @update-name="updateName"
        />
      </div>
    </template>
    
    <script setup>
    	...
    	const updateName = function () {
    	  name.value = 'Bella'
    	}
    </script>
    ```