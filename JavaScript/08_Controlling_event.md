# Controlling event

1. 웹에서의 이벤트
    - 화면을 스크롤하는 것
    - 버튼을 클릭했을 때 팝업 창이 출력되는 것
    - 마우스 커서의 위치에 따라 드래그 앤 드롭 하는 것
    - 사용자의 키보드 입력에 따라 새로운 요소를 생성하는 것
    - → 웹에서 모든 동작은 이벤트 발생과 함께 한다

## event 객체

1. 설명 :
    - DOM에서 이벤트가 발생했을 때 생성되는 객체
    - DOM 요소에서 event가 발생하면, 해당 event는 연결된 이벤트 처리기(event handler)에 의해 처리 됨
2. 이벤트 종류
    - mouse, input, keyboard, touch …

## event handler

1. 설명 :
    - 특정 이벤트가 발생했을 때 실행되는 함수
    - 사용자의 행동에 어떻게 반응할지를 JavaScript 코드로 표현한 것

### `.addEventListener()` :

1. 설명 :
    - 대표적인 이벤트 핸들러 중 하나
    - 특정 이벤트를 DOM 요소가 수신할 때마다 콜백 함수를 호출
2. 구조
    
    ```jsx
    EventTarget.addEventListener(type, handler)
    ```
    
    - EventTarget : DOM 요소
    - type : 수신할 이벤트
        - 문자열로 작성 (이미 정해진 예약어가 존재)
    - handler : 콜백 함수
        - 발생한 이벤트 객체를 수신하는 콜백 함수
        - 이벤트 핸들러는 자동으로 event 객체를 매개변수로 받음
    - 대상에 특정 Event가 발생하면, 지정한 이벤트를 받아 할 일을 등록한다.
3. 활용
    - 버튼을 클릭하면 버튼 요소 출력하기
        - 버튼에 이벤트 처리기를 부착하여 클릭 이벤트가 발생하면 이벤트가 발생한 버튼 정보를 출력
        
        ```jsx
        // 1. 버튼 선택
        const btn = document.querySelector('#btn')
        
        // 2. 콜백 함수
        const detectClick = function (event) {
          console.log(event) // PointerEvent
          console.log(event.currentTarget) // <button id='btn>버튼</button>
          console.log(this) // <button id='btn>버튼</button>
        }
        
        // 3. 버튼에 이벤트 핸들러를 부착
        btn.addEventListener('click', detectClick)
        ```
        
        - 이벤트 핸들러 내부의 this는 이벤트 리스너에 연결된 요소 currentTarget를 가리킴
        - 이벤트가 발생하면 event 객체가 생성되어 첫 번째 인자로 전달
            - event 객체가 필요 없는 경우 생략 가능
        - 반환 값 없음

### 버블링

1. 실습 :
    
    ```jsx
    const formElement = document.querySelector('#form')
    const divElement = document.querySelector('#div')
    const pElement = document.querySelector('#p')
    
    const clickHandler1 = function (event) {
      console.log('form이 클릭되었습니다.')
    }
    const clickHandler2 = function (event) {
      console.log('div가 클릭되었습니다.')
    }
    const clickHandler3 = function (event) {
      console.log('p가 클릭되었습니다.')
    }
    
    formElement.addEventListener('click', clickHandler1)
    divElement.addEventListener('click', clickHandler2)
    pElement.addEventListener('click', clickHandler3)
    ```
    
    - <p> 요소만 클릭했는데도 불구하고 모든 핸들러가 동작함
2. 설명 :
    - 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작하는 현상
    - 가장 최상단의 조상 요소(document)를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작
    - 이벤트가 제일 깊은 곳에 있는 요소에서 시작해 부모 요소를 거슬러 올라가며 발생하는 것이 마치 물속 거품과 닮았기 때문
    - 최하위 <p> 요소를 클릭하면 p → div → form 순서로 3개의 이벤트 핸들러가 모두 순차적으로 동작했던 것
3. `event.currentTarget` & `event.target` :
    - currentTarget
        - 현재 요소
        - 항상 이벤트 핸들러가 연결된 요소만을 참조하는 속성
        - this와 같음
    - target
        - 이벤트가 발생한 가장 안쪽의 요소(target)를 참조하는 속성
        - 실제 이벤트가 시작된 요소
        - 버블링이 진행되어도 변하지 않음
    - 실습
        
        ```jsx
        const outerOuterElement = document.querySelector('#outerouter')
        const outerElement = document.querySelector('#outer')
        const innerElement = document.querySelector('#inner')
        
        const clickHandler = function (event) {
          console.log('currentTarget:', event.currentTarget.id)
          console.log('target:', event.target.id)
        }
        
        outerOuterElement.addEventListener('click', clickHandler)
        ```
        
        - currentTarget : 핸들러가 연결된 outerouter 요소만을 가리킴
        - target : 실제 이벤트가 발생하는 요소를 가리킴
        - 핸들러는 outerouter에만 연결되어 있지만, 하위 요소 outer, inner를 클릭해도 해당 핸들러가 동작함
            - 클릭 이벤트가 어디에서 발생했든 상관없이 outerouter까지 이벤트가 버블링되어 핸들러를 실행시키기 때문
4. 버블링이 필요한 이유
    - 만약 다음과 같이 각자 다른 동작을 수행하는 버튼이 여러 개가 있다고 가정
        - 그렇다면 각 버튼마다 다른 이벤트를 핸들러를 할당해야 할까?
            - 각 버튼의 공통 조상인 요소에 이벤트 핸들러 단 하나만 할당하기
    - 요소의 공통 조상에 이벤트 핸들러를 단 하나만 할당하면, 여러 버튼 요소에서 발생하는 이벤트를 한꺼번에 다룰 수 있음
    - 공통 조상에 할당한 핸들러에서 event.target을 이용하면 실제 어떤 버튼에서 이벤트가 발생했는지 알 수 있기 때문

### 캡처링과 버블링

1. 캡처링 설명 : 이벤트가 하위 요소로 전파되는 단계 (버블링과 반대)
2. 특징 :
    - table의 하위 요소를 클릭하면 이벤트는 먼저 최상위 요소부터 아래로 전파됨 (캡처링)
    - 실제 이벤트가 발생한 지점(event.target)에서 실행된 후 다시 위로 전파(버블링)
        - 이 전파 과정에서 상위 요소에 할당된 이벤트 핸들러들이 호출되는 것
    - 캡처링은 실제 개발자가 다루는 경우가 없으므로 버블링에 집중하기

### event handler 활용 실습

1. click : 버튼을 클릭하면 숫자를 1씩 증가해서 출력하기
    
    ```jsx
    // 1. 초기값
    let countNumber = 0
    
    // 2. 버튼 요소 선택
    const btn = document.querySelector('#btn')
    
    // 3. 이벤트 핸들러의 콜백 함수
    const clickHandler = function () {
      // 3.1 초기값을 +1 증가
      countNumber += 1
    
      // 3.2 숫자를 콘텐츠로 가지고 있는 span 태그를 선택
      const spanTag = document.querySelector('#counter')
    
      // 3.3 span 태그의 콘텐츠 값을 countNumber 값으로 변경
      spanTag.textContent = countNumber
    }
    
    // 4. 선택한 버튼에 이벤트 핸들러 부착
    btn.addEventListener('click', clickHandler)
    ```
    
2. input 이벤트 실습 : 사용자의 입력 값을 실시간으로 출력하기
    
    ```jsx
    // 1. input 요소를 선택 (이벤트가 발생하는 지점)
    const inputTag = document.querySelector('#text-input')
    // 2. p 요소 선택
    const pTag = document.querySelector('p')
    
    // 3. 콜백 함수 (input 요소에 input 이벤트가 발생할 때마다 실행될 코드)
    const inputHandler = function (event) {
      // 3.1 이벤트 객체에서 사용자가 입력한 값을 찾아 저장
      // console.log(event)
      // console.log(event.currentTarget)
      // console.log(this)
      // console.log(event.currentTarget.value)
      // const inputData = this.value
      const inputData = event.currentTarget.value
    
      // 3.2 선택한 p요소의 텍스트 콘텐츠에 할당
      pTag.textContent = inputData
    }
    
    // 4. 선택한 input 요소에 이벤트 핸들러를 부착
    inputTag.addEventListener('input', inputHandler)
    
    ```
    
    - console.log()로 event 객체를 출력할 경우 currentTarget 키 값은 null을 가짐
    - currentTarget은 이벤트가 처리되는 동안에만 사용할 수 있기 때문
    - 대신 console.log(event.currentTarget)을 사용하여 콘솔에서 확인가능
    - currentTarget 이후의 속성 값들은 target을 참고해서 사용하기
3. click & input 이벤트 실습
    - 사용자의 입력 값을 실시간으로 출력
        - + 버튼을 클릭하면 출력되는 값의 CSS 스타일을 변경하기
        
        ```jsx
        // 1. input 구현
        // 1-1. input & h1 요소 선택
        const inputTag = document.querySelector('#text-input')
        const h1Tag = document.querySelector('h1')
        
        // 1-2. 콜백 함수
        const inputHandler = function (event) {
          // 1-2-1. 사용자 입력 데이터 추출
          const inputData = event.currentTarget.value
          // 1-2-2. 추출한 데이타를 h1 요소의 콘텐츠로 할당
          h1Tag.textContent = inputData
        }
        // 1-3. input 요소에 이벤트 핸들러 부착
        inputTag.addEventListener('input', inputHandler)
        
        // 2. 버튼 기능 구현
        // 2-1. 버튼 요소 선택
        const btn = document.querySelector('#btn')
        
        // 2-2. 콜백 함수
        const clickHandler = function (event) {
          // 2-2-1. h1 요소의 클래스 목록에 blue 문자열 추가
          h1Tag.classList.add('blue')
          // 혹은 toggle 메서드로도 구현 가능
          // h1Tag.classList.toggle('blue')
        }
        
        // 2-3. 버튼에 이벤트 핸들러 부착
        btn.addEventListener('click', clickHandler)
        ```
        
    - todo 실습
        
        ```jsx
        // 1. 필요한 요소들 선택
        const inputTag = document.querySelector('.input-text')
        const btn = document.querySelector('#btn')
        const ulTag = document.querySelector('ul')
        
        // 2. 콜백 함수 (실제 todo 데이터를 생성 후 추가하는 로직)
        const addTodo = function (event) {
          // 2-1. 사용자 입력 데이터 저장
          const inputData = inputTag.value
        
          // 2-2. li 태그를 생성
          const liTag = document.createElement('li')
        
          // 2-3. li 태그의 텍스트 콘텐츠로 사용자 입력데이터 할당
          liTag.textContent = inputData
        
          // 2-4. ul 태그의 자식태그로 완성된 li 태그를 자식으로 추가
          ulTag.appendChild(liTag)
        
          // 2-5. todo 추가 후에 input에 작성한 데이터를 초기화
          inputTag.value= ''
        }
        
        // 3. 버튼에 이벤트 핸들러 부착
        btn.addEventListener('click', addTodo)
        ```
        
        - todo 추가 기능 구현
            - 빈 문자열 입력 방지
            - 입력이 없을 경우 경고 대화상자를 띄움
            
            ```jsx
            const addTodo = function (event) {
            	const inputData = inputTag.value
            	if (inputData.trim()) {
            		const liTag = document.createElement('li')
            		liTag.textContent = inputData
            		ulTag.appendChild(liTag)
            		inputTag.value = ''
            	} else {
            		alert('todo를 입력해주세요')
            	}
            }
            ```
            
    - 로또 번호 생성기 실습
        
        ```jsx
        <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
          <script>
            // 1. 필요한 모든 요소를 선택
            const btn = document.querySelector('#btn')
            const divTag = document.querySelector('div')
        
            // 2. 로또 번호를 생성하는 함수
            const getNumbers = function () {
              // 2.1 1부터 45까지의 배열을 생성
              const numbers = _.range(1, 46)
        
              // 2.2 45개의 요소 중 랜덤으로 6개를 추출
              const sixNumbers = _.sampleSize(numbers, 6)
        
              // 2.3 추출한 번호 배열을 정렬
              // const sortedNumbers = _.sortBy(sixNumbers)
              return sixNumbers
            }
        
            // 3. 로또 번호를 화면에 출력하는 함수 (이벤트 핸들러의 콜백 함수)
            const getLottery = function (event) {
              // 3.1 추출한 6개 로또 번호 할당
              const numbers = getNumbers()
        
              // 3.2 6개의 li 요소를 담을 ul 태그를 생성
              const ulTag = document.createElement('ul')
        
              // 3.3 추출한 6개 로또 번호를 반복하면서 li 태그를 생성
              numbers.forEach((number) => {
                // 3.4 번호를 담을 li 태그 생성
                const liTag = document.createElement('li')
                // 3.5 반복을 통해 나온 번호를 li 태그의 값으로 할당
                liTag.textContent = number
                // 3.6 완성된 li 태그를 부모 ul 태그의 자식으로 추가
                ulTag.appendChild(liTag)
              })
              // 3.7 완성된 ul 태그를 div 태그의 자식으로 추가
              divTag.appendChild(ulTag)
            }
        
            // 4. 버튼에 이벤트 핸들러 부착
            btn.addEventListener('click', getLottery)
        ```