# List Rendering

## v-for

1. 설명 : 소스 데이터(Array, Object, Number, String, Iterable)를 기반으로 요소 또는 템플릿 블록을 여러번 렌더링
2. 구조 :
    - alias in expression 형식의 특수 구문을 사용
        
        ```html
        <div v-for="item in items">
        	{{ item.text }}
        </div>
        ```
        
    - 인덱스(객체에서는 key)에 대한 별칭을 지정할 수 있음
        
        ```html
        <div v-for="(item, index) in arr"></div>
        
        <div v-for="value in object"></div>
        <div v-for="(value, key) in object"></div>
        <div v-for="(value, key, index) in object"></div>
        ```
        
        - v-for 태그가 붙은 div가 myArr의 길이만큼 생기는 것
3. 활용:
    - v-for on <templates> : HTML template 요소에 v-for를 사용하거나 하나 이상의 요소에 대해 반복 렌더링 할 수 있음
        
        ```html
        <!-- v-for on <template> -->
        <ul>
          <template v-for="item in myArr">
            <li>{{ item.name }}</li>
            <li>{{ item.age }}</li>
            <hr>
          </template>
        </ul>
        ```
        
    - nested v-for : 각 v-for의 하위 영역은 상위 영역에 접근 할 수 있음
        
        ```html
         <!-- nested v-for -->
        <ul v-for="item in myInfo">
          <li v-for="friend in item.friends">
            {{ item.name }} - {{ friend }}
          </li>
        </ul>
        ```
        
4. 주의 : v-for와 배열을 함께 사용 시, 배열의 메서드를 주의해서 사용
    - 변화 매서드 사용 X
        - 호출하는 원본 배열을 변경
        - push(), pop(), shift(), unshift(), splice(), sort(), reverse()
    - 배열 교체
        - 원본 배열을 수정하지 않고 항상 새 배열을 반환
        - filter(), concat(), slice()
    - v-for와 배열을 사용해 필터링/정렬 활용하기
        - 원본데이터를 수정하거나 교체하지 않고 필터링하거나 정렬된 새로운 데이터를 표시하는 방법
        - computed 활용 : 원본 기반으로 필터링 된 새로운 결과를 생성
            
            ```jsx
            const numbers = ref([1, 2, 3, 4, 5])
            
            const evenNumbers = computed(() => {
            	return numbers.value.filter((number))
            })
            ```
            
            ```html
            <li v-for="num in evenNumbers(numbers)">{{ num }}</li>
            ```
            
        - method 활용 : computed가 불가능한 중첩된 v-for에 경우
            
            ```jsx
            const numberSets = ref([
            	[1, 2, 3, 4, 5],
            	[6, 7, 8, 9, 10]
            ])
            
            const evenNumbers = function (numbers) {
            	return numbers.filter((number) => number % 2 === 0)
            }
            ```
            
            ```html
            <ul v-for="numbers in numberSets">
            	<li v-for="num in evenNumbers(numbers)">{{ num }}</li>
            </ul>
            ```
            

### v-for with key

> 반드시 v-for와 key를 함께 사용한다
> 
> 
> → 내부 컴포넌트의 상태를 일관되게 하여 데이터의 예측 가능한 행동을 유지하기 위함
> 
1. 사용 예시
    
    ```html
    <div v-for="item in items" :key="item.id">
      {{ item.name }}
    </div>
    ```
    
2. 내장 특수 속성 key
    - 고유한 값을 나타낼 수 있는 식별자여야 함
        - 권장
            - 데이터베이스의 고유 ID
            - 항목의 고유한 식별자
            - 변경되지 않는 속성 값
        - 피해야 할 사항
            - 배열의 인덱스를 key로 사용하는 것
            - 객체를 key로 사용하는 것
    - number 혹은 string으로만 사용해야 함
    - Vue의 내부 가상 DOM 알고리즘이 이전 목록과 새 노드 목록을 비교할 때 각 node를 식별하는 용도로 사용
    - 에러가 발생하지 않더라도, Vue 내부 동작 관련된 부분이기에 최대한 작성하려고 노력할 것

### v-for with v-if

> 동일 요소에 v-for과 v-if를 함께 사용하지 않는다
> 
> 
> → 동일 요소에서 v-if 가 v-for보다 우선순위가 더 높기 때문
> 
> → v-if에서의 조건은 v-for 범위의 변수에 접근할 수 있음
> 
1. 예시 :
    - 동일 요소에 v-for와 v-if 사용 → 에러 발생
        
        ```html
        <!-- [Bad] v-for with v-if -->
        <!-- 동일 요소에 v-for와 v-if를 함께 사용하지 않는다. -->
        <ul>
          <li v-for="todo in todos" v-if="!todo.isComplete" :key="todo.id">
            {{ todo.name }}
          </li>
        </ul>
        ```
        
    - 해결책 1. computed를 활용해 이미 필터링 된 목록을 반환하여 반복
        
        ```jsx
        const app = createApp({
          setup() {
            let id = 0
        
            const todos = ref([
              { id: id++, name: '복습', isComplete: true },
              { id: id++, name: '예습', isComplete: false },
              { id: id++, name: '저녁식사', isComplete: true },
              { id: id++, name: '노래방', isComplete: false }
            ])
        
            const completeTodos = computed(() => {
              return todos.value.filter((todo) => !todo.isComplete)
            })
        
            return {
              todos,
              completeTodos
            }
          }
        })
        ```
        
        ```html
        <!-- [Good] computed 활용 -->
        <!-- 해결책 1. computed를 활용해 이미 필터링 된 목록을 반환하여 반복 -->
        <ul>
          <li v-for="todo in completeTodos">
            {{ todo }}
          </li>
        </ul>
        ```
        
    - 해결책 2. template 요소를 사용하여 v-for와 v-if의 위치를 분리
        
        ```html
        <!-- [Good] template 활용 -->
        <!-- 해결책 2. template 요소를 사용하여 v-for와 v-if의 위치를 분리 -->
        <ul>
          <template v-for="todo in todos" :key="todo.id">
            <li v-if="!todo.isComplete">
              {{ todo.name }}
            </li>
          </template>
        </ul>
        ```