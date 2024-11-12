# State Management

1. 설명 :
    - Vue 컴포넌트는 이미 반응형 상태를 관리하고 있음
        - 상태 === 데이터
2. 컴포넌트 구조의 단순화
    - 상태 State : 앱 구동에 필요한 기본 데이터
    - 뷰 View : 상태를 선언적으로 매핑하여 시각화
    - 기능 Actions : 뷰에서 사용자 입력에 대해 반응적으로 상태를 변경할 수 있게 정의된 동작
    - → 단방향 데이터 흐름의 간단한 표현
    - 상태 관리의 단순성이 무너지는 시점 : 여러 컴포넌트가 상태를 공유할 때
        - 여러 뷰가 동일한 상태에 종속되는 경우
        - 서로 다른 뷰의 기능이 동일한 상태를 변경시켜야 하는 경우
3. 해결책 : 각 컴포넌트의 공유 상태를 추출하여, 전역에서 참조할 수 있는 저장소에서 관리
    - 컴포넌트 트리 → 하나의 큰 View, 모든 컴포넌트는 트리 계층 구조에 관계 없이 상태에 접근하거나 기능을 사용할 수 있음

## Pinia

1. 설명 : Vue의 공식 상태 관리 라이브러리
2. 설치 : Vite 프로젝트 빌드 시 Pinia 라이브러리 추가
3. Vue 프로젝트 구조 변화
    - stores 폴더 신규 생성
4. Pinia 활용 시점 :
    - Pinia를 사용한다고 해서 모든 데이터를 state에 넣어야 하는 것은 아님
        - pass props, emit event도 상황에 따라 적절히 사용해야 함
    - Pinia → 공유된 상태를 관리하는 데 유용, 하지만 애플리케이션이 단순하다면 없는게 더 효율적일지도

### Pinia 구성요소

1. store
    - 중앙 저장소
    - 모든 컴포넌트가 공유하는 상태, 기능 등이 작성됨
    - defineStore()의 반환 값 이름은 user와 store 사용 권장 (user@@@Store)
    - defineStore()의 첫번째 인자는 애플리케이션 전체에 걸쳐 사용하는 store의 고유 ID
    - 여러 store 작성 가능
    
    ```jsx
    // stores/counter.js
    
    import { ref, computed } from 'vue'
    import { defineStore } from 'pinia'
    
    export const useCounterStore = defineStore('counter', () => {
      const count = ref(0)
      const doubleCount = computed(() => count.value * 2)
      function increment() {
        count.value++
      }
    
      return { count, doubleCount, increment }
    })
    
    ```
    
    - Setup Stores의 반환 값
        - pinia의 상태들을 사용하려면 반드시 반환해야 함
        - store에서는 공유하지 않는 private한 상태 속성을 가지지 않음 → 작성하지 않음
2. state `const count = ref(0)`
    - 반응형 상태(데이터)
    - ref() === state
3. getters `const doubleCount = computed(() => count.value * 2)`
    - 계산된 값
    - computed() === getters
4. actions `function increment()`
    - 메서드
    - function() === actions
5. plugin
    - 애플리케이션의 상태 관리에 필요한 추가 기능을 제공하거나 확장하는 도구나 모듈
    - 애플리케이션의 상태 관리를 더욱 간편하고 유연하게 만들어주며 패키지 설치 이후 별도 설정을 통해 추가 됨

### 활용

1. state 사용 :
    - 각 컴포넌트 깊이에 관계 없이 store 인스턴스로 state에 접근하여 직접 읽고 쓸 수 있음
    - 만약 store에서 state를 정의하지 않았다면 컴포넌트에서 새로 추가할 수 없음
    
    ```jsx
    // App.vue
    
    <script setup>
    import HelloWorld from './components/HelloWorld.vue'
    import TheWelcome from './components/TheWelcome.vue'
    // 1. 중앙 저장소 가져오기
    import { useCounterStore } from './stores/counter'
    
    // 2. 중앙 저장소를 활용하여 인스턴스 생성
    const store = useCounterStore()
    
    // 3. 중앙 저장소에 상태를 참조
    console.log(store.count)
    
    // cf. 각 컴포넌트에서는 중앙저장소에 새로운 상태 생성 불가능
    store.ssafy = 'ssafy'
    </script>
    
    ```
    
2. getters 사용 :
    - store의 모든 getters 또한 state처럼 직접 접근 가능
3. Actions 사용 :
    - store의 모든 actions 또한 직접 접근 및 호출 가능
    - getters와 달리 state 조작, 비동기, API 호출이나 다른 로직 진행 가능

## Pinia 실습 : Todo 프로젝트

1. 컴포넌트 구성
    
    
    | App |  |
    | --- | --- |
    | TodoForm | TodoList |
    |  | TodoListItem |
2. 사전 준비
    - 초기 생성된 컴포넌트 모두 삭제
    - src/assets 내부 파일 모두 삭제
    - main.js css 코드 삭제
3. App.vue
    
    ```jsx
    <template>
      <div>
        <h1>Todo Project</h1>
        <h2>완료된 Todo 개수: {{ store.doneTodosCount }}</h2>
        <TodoList />
        <TodoForm />
      </div>
    </template>
    
    <script setup>
    import TodoList from '@/components/TodoList.vue'
    import TodoForm from '@/components/TodoForm.vue'
    import { useCounterStore } from './stores/counter';
    
    const store = useCounterStore()
    </script>
    
    <style scoped>
    
    </style>
    ```
    
4. TodoForm.vue
    
    ```jsx
    <template>
      <div>
        <form @submit.prevent="createTodo(todoText)" ref="formElem">
          <input type="text" v-model="todoText">
          <input type="submit">
        </form>
      </div>
    </template>
    
    <script setup>
    import { ref } from 'vue'
    import { useCounterStore } from '@/stores/counter'
    
    const todoText = ref('')
    
    // ref로 form 태그 선택 -> todo 등록하고 입력된거 지우기 위해
    // 여기서 null은 의미 없음
    const formElem = ref(null)
    
    const store = useCounterStore()
    // store에 있는 addTodo를 호출할 함수를 정의
    // DOM에서 바로 호출하면 추가로직을 작성할 수 없음
    const createTodo = function (todoText) {
      store.addTodo(todoText)
      // form 초기화
      formElem.value.reset()
    }
    
    </script>
    
    <style scoped>
    
    </style>
    ```
    
5. TodoList.vue
    
    ```jsx
    <template>
      <div>
        <TodoListItem
          v-for="todo in store.todos"
          :key="todo.id"
          :todo="todo" // 하위 컴포넌트 TodoListItem을 반복하면서 개별 todo를 props로 전달
        />
      </div>
    </template>
    
    <script setup>
    import TodoListItem from '@/components/TodoListItem.vue'
    import { useCounterStore } from '@/stores/counter'
    
    const store = useCounterStore()
    
    </script>
    
    <style scoped>
    
    </style>
    ```
    
6. TodoListItem.vue
    
    ```jsx
    <template>
      <div>
        <span
          @click="store.updateTodo(todo.id)"
          :class="{ 'is-done': todo.isDone }"
        >
          {{ todo.text }}
        </span>
        <button @click="store.deleteTodo(todo.id)"> X </button>
      </div>
    </template>
    
    <script setup>
    import { useCounterStore } from '@/stores/counter'
    
    defineProps({
      todo: Object
    })
    
    const store = useCounterStore()
    
    </script>
    
    <style scoped>
    .is-done {
      text-decoration: line-through;
    }
    </style>
    ```
    
7. counter.js
    
    ```jsx
    import { ref, computed } from 'vue'
    import { defineStore } from 'pinia'
    
    export const useCounterStore = defineStore('counter', () => {
      let id = 0
      const todos = ref([])
    
      const addTodo = function (todoText) {
        todos.value.push({
          id: id++,
          text: todoText,
          isDone: false,
        })
      }
    
      const deleteTodo = function (selectedId) {
        const index = todos.value.findIndex((todo) => todo.id === selectedId)
        todos.value.splice(index, 1)
      }
    
      const updateTodo = function (selectedId) {
        todos.value = todos.value.map((todo) => {
          if (todo.id === selectedId) {
            todo.isDone = !todo.isDone
          }
          return todo
        })
      }
    
      const doneTodosCount = computed(() => {
        const doneTodos = todos.value.filter((todo) => todo.isDone)
        return doneTodos.length
      })
    
      return { todos, addTodo, deleteTodo, updateTodo, doneTodosCount }
    }, { persist: true })
    
    ```
    

### Local Storage

1. 설명 : 브라우저 내에 key-value 쌍을 저장하는 웹 스토리지 객체
2. 특징 :
    - 페이지를 새로 고침하고 브라우저를 다시 실행해도 데이터가 유지
    - 쿠키와 다르게 네트워크 요청 시 서버로 전송되지 않음
    - 여러 탭이나 창 간에 데이터를 공유 할 수 있음
3. 사용 목적 :
    - 사용자 설정, 상태 정보, 캐시 데이터 등을 클라이언트 측에서 보관
    - → 웹 사이트 성능 향상, 사용자 경험 개선
4. pinia-plugin-persistedstate
    - Pinia의 플러그인 중 하나
    - 웹 애플리케이션의 상태(state)를 브라우저의 local storage나 session storage에 영구적으로 저장하고 복원하는 기능을 제공
5. 설치 및 등록
    - 공식 문서 https://prazdevs.github.io/pinia-plugin-persistedstate/guide/
    - 설치
        
        ```bash
        npm i pinia-plugin-persistedstate
        ```
        
    - 등록 main.js
        
        ```jsx
        // main.js
        
        import { createApp } from 'vue'
        import { createPinia } from 'pinia'
        import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
        import App from './App.vue'
        
        const app = createApp(App)
        const pinia = createPinia()
        pinia.use(piniaPluginPersistedstate)
        
        // app.use(createPinia())
        app.use(pinia)
        
        app.mount('#app')
        ```
        
    - counter.js : defineStore()의 3번째 인자로 관련 객체 추가
        
        ```bash
        export const useCounterStore = defineStore('counter', () => {
         ...
          return { todos, addTodo, deleteTodo, updateTodo, doneTodosCount }
        }, { persist: true })
        
        ```