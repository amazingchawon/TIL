# Passing Props

## Props

1. 설명 : 부모 컴포넌트로부터 자식 컴포넌트로 데이터를 전달하는데 사용되는 속성
2. 특징 :
    - 부모 속성이 업데이트 → 자식으로 전달
        - 자식이 업데이트 →부모로 전달 X ⇒ 이때는 Emit event
        - 자식 컴포넌트 내부에서 props를 변경하려고 시도하면 X
    - 부모 컴포넌트가 업데이트 될 때마다 이를 사용하는 자식 컴포넌트의 모든 props가 최신 값으로 업데이트 됨
    - ⇒ 부모 컴포넌트에서만 변경하고 이를 내려 받는 자식 컴포넌트는 자연스럽게 갱신
3. One-way Data Flow → 이렇게 부모에서 자식으로만 전달되는 걸 의미
    - 모든 props는 자식 속성과 부모 속성 사이에 하향식 단방향 바인딩을 형성
    - 단방향인 이유 :
        - 하위 컴포넌트가 실수로 상위 컴포넌트의 상태를 변경하여 앱에서의 데이터 흐름을 이해하기 어렵게 만드는 것을 방지하기 위함
        - 데이터 흐름의 일관성 및 단순화

### Props 선언

1. 설명 :
    - 부모 컴포넌트에서 내려보낸 props를 사용하기 위해서는 자식 컴포넌트에서 명시적인 props 선언이 필요
2. 사전 준비
    - vue 프로젝트 생성
    - 초기 생성된 컴포넌트 모두 삭제(App.vue 제외)
    - src/asstes 내부 파일 모두 삭제
    - main.js 해당 코드 삭제
    - App > Parent > ParentChild 컴포넌트 관계 작성
3. Props 작성
    - 부모 컴포넌트에서 Parent에서 자식 컴포넌트 ParentChild에 보낼 props 작성
        
        ```html
        <template>
          <div>
            <ParentChild my-msg="message" />
          </div>
        </template>
        ```
        
4. Props 선언
    - defineProps()를 사용하여 props를 선언
    - defineProps()에 작성하는 인자의 데이터 타입에 따라 선언 방식이 나뉨
        - 문자열 배열을 사용한 선언
            
            ```html
            <script setup>
            	defineProps(['myMsg'])
            </script>
            ```
            
            - 배열의 문자열 요소로 props로 선언
            - 문자열 요소의 이름은 전달된 Props의 이름
        - 객체를 이용한 선언
            
            ```html
            <script setup>
            	defineProps({
            		myMsg: String
            	})
            </script>
            ```
            
            - 각 객체의 속성은 키가 전달받은 props 이름이 되며, 객체 속성의 값은 값이 될 데이터의 타입에 해당하는 생성자 함수(Number, String…) 여야 함
            - 객체 선언 문법 사용 권장
                - 컴포넌트를 가독성이 좋게 문서화하는 데 도움이 됨
                - 다른 개발자가 잘못된 유형을 전달할 때에 브라우저 콘솔에 경고를 출력하도록 함
5. Props 데이터 사용
    - props 선언 후 템플릿에서 반응형 변수와 같은 방식으로 활용
        
        ```html
        <div>
        	<p>{{ myMsg }}</p>
        </div>
        ```
        
    - props를 객체로 반환하므로 필요한 경우 JavaScript에서 접근 가능
        
        ```html
        <script setup>
        	const props = defineProps({ myMsg: String })
        	console.log(props) // {myMsg:'message'}
        	console.log(props.myMsg) // 'message'
        </script>
        ```
        

### Props 세부사항

1. Props Name Casing (Props 이름 컨벤션)
    - 자식 컴포넌트로 전달시 → kebab-case
    - 선언 및 템플릿 참조시 → camelCase
2. Static Props와 Dynamic Props
    - Static Props : 지금까지 작성한 Props는 모두 정적
    - Dynamic Props :
        - v-bind를 사용하여 동적으로 할당된 props를 사용할 수 있음

### Dynamic Props

1. 정의 :
    
    ```jsx
    // Parent.vue
    import { ref } from 'vue'
    
    const name = ref('Alice')
    ```
    
    ```html
    <ParentChild my-msg="message" :dynamic-props="name" />
    ```
    
2. 선언 및 출력
    
    ```jsx
    // ParentChild.vue
    
    defineProps({
    	myMsg: String,
    	dynamicProps: String,
    })
    ```
    
    ```html
    <p>{{ dynamicProps }}</p>
    ```
    

### 정적/동적 props 주의사항

1. 차이 : v-bind 가 붙어있는지 아닌지 차이

### Props 활용

1. 다른 디렉티브와 함께 사용
    - ParentItem.vue
        
        ```html
        <template>
          <div>
            <p>{{ myProp.id }}</p>
            <p>{{ myProp.name }}</p>
          </div>
        </template>
        
        <script setup>
        defineProps({
          myProp: Object,
        })
        </script>
        ```
        
    - Parent.vue
        
        ```html
        <template>
          <div>
            ...
            <ParentItem
              v-for="item in items"
              :key="item.id"
              :my-prop="item"
            />
          </div>
        </template>
        
        <script setup>
        import ParentItem from '@/components/ParentItem.vue'
        import { ref } from 'vue'
        
        const items = ref([
          { id: 1, name: '사과'},
          { id: 2, name: '바나나'},
          { id: 3, name: '딸기'}
        ])
        </script>
        ```