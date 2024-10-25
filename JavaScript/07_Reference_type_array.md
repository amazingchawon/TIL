# Reference Type : Array

1. 정의 : 순서가 있는 데이터 집합을 저장하는 자료구조
2. 구조 :
    
    ```jsx
    const names = ['Alice', 'Bella', 'Cathy']
    
    consol.log(names[0]) // Alice
    consol.log(names.length) // 3
    ```
    
    - 대괄호를 이용해 작성
    - 요소의 자료형은 제약 없음
    - length 속성을 사용해 배열에 담긴 요소 개수 확인 가능
3. 수정
    
    ```jsx
    names[1] = 'Dan'
    consol.log(names) // ['Alice', 'Dan', 'Cathy']
    ```
    
4. 배열은 객체다
    - 배열도 키와 속성들을 담고 있는 참조 타입의 객체
    - 배열의 요소를 대괄호 접근법을 사용해 접근하는 건 객체 문법과 같음
        - 배열의 키는 숫자
    - 숫자형 키를 사용함으로써 배열은 객체 기본 기능 이외에도 순서가 있는 컬렌션을 제어해주는 특별한 메소드를 제공하는 것

## 배열 메서드

1. `push()` , `pop()` : 배열 끝 요소를 추가 / 제거
    
    ```jsx
    const names = ['Alice', 'Bella', 'Cathy']
    
    // pop
    console.log(names.pop()) // Cathy
    console.log(names) // ['Alice', 'Bella']
    
    // push
    names.push('Dan')
    console.log(names) // ['Alice', 'Bella', 'Dan']
    ```
    
    - `pop()` 은 반환 값 존재
2. `shift()` , `unshift()` : 배열 앞 요소를 제거 / 추가
    
    ```jsx
    // shift
    console.log(names.shift()) // Alice
    console.log(names) // ['Bella', 'Dan']
    
    // unshift
    names.unshift('Eric')
    console.log(names) // ['Eric', 'Bella', 'Dan']
    ```
    
    - `shift()` 은 반환 값 있음

## Array Helper Methods

1. 정의 : 배열 조작을 보다 쉽게 수행할 수 있는 특별한 메서드 모음
2. 특징 :
    - ES6에 도입
    - 배열의 각 요소를 **순회**하며 각 요소에 대해 **함수(콜백함수)**를 호출
    - 대표 메서드 : `forEach()`, `map()`, `filter()`, `every()`, `some()`, `reduce()` 등
    - 메서드 호출 시 인자로 함수(콜백함수)를 받는 것이 특징

### 콜백 함수 Callback function

1. 설명 :
    - 다른 함수에 인자로 전달되는 함수
    - 외부 함수 내에서 호출되어 일종의 루틴이나 특정 작업을 진행
2. 사용법/동작법 :
    
    ```jsx
    const numbers1 = [1, 2, 3]
    numbers1.forEach(function (number) {
      console.log(number) // 1 2 3
    })
    ```
    
    ```jsx
    const numbers2 = [1, 2, 3]
    const callBackFunc = function (number) {
      console.log(number)
    })
    
    numbers2.forEach(callBackFunc)
    ```
    
    | 메서드 | 역할 |
    | --- | --- |
    | `forEach` | - 배열 내의 모든 요소에 각각에 대해 함수(콜백함수)를 호출<br>- 반환 값 없음 |
    | `map` | - 배열 내의 모든 요소에 각각에 대해 함수(콜백함수)를 호출<br>- 반환 값 있음 |
3. 콜백 함수의 이점 :
    - 함수의 재사용성 측면
        - 함수를 호출하는 코드에서 콜백 함수의 동작을 자유롭게 변경할 수 있음
        - 예를 들어, map 함수는 콜백 함수를 인자로 받아 배열의 각 요소를 순회하며 콜백 함수를 실행
        - 이때, 콜백 함수는 각 요소를 변환하는 로직을 담당하므로, map 함수를 호출하는 코드는 간결하고 가독성 높아짐
    - 비동기적 처리 측면
        - 동기 : 순차적 처리
        - 비동기 : 병렬적 처리
        - 예시
            
            ```jsx
            console.log('a')
            
            setTimeout(() => {
            	console.log('b')
            }, 3000)
            
            console.log('c')
            
            // a c b 순으로 출력
            ```
            
            - setTimeout 함수는 콜백 함수를 인자로 받아 일정 시간이 지난 후에 실행됨
            - 이때, setTimeout 함수는 비동기적으로 콜백 함수를 실행하므로, 다른 코드의 실행을 방해하지 않음 (비동기 JavaScript에서 자세히)

### `forEach()`

1. 설명 :
2. 구조 :
    
    ```jsx
     arr.forEach(callback(item[, index[, array]]))
    ```
    
    - item : 처리할 배열의 요소
    - index : 처리할 배열 요소의 인덱스 (선택 인자)
    - array : forEach를 호출한 배열 (선택 인자)
    - 반환값 : Undefined
3. 예시
    
    ```jsx
    const names = ['Alice', 'Bella', 'Cathy']
    
    // 일반 함수 표기
    names.forEach(function (name) {
    	console.log(name)
    })
    
    // 화살표 함수 표기
    namses.forEach((name) => {
    	console.log(name)
    })
    
    // 활용
    names.forEach((name, index, array) => {
    	console.log(name, index, array)
    }
    ```
    

### `map()`

1. 설명 :
    - 배열의 모든 요소에 대해 함수를 호출하고, 반환된 후에 호출한 결과 값을 모아 새로운 배열을 반환
2. 구조 :
    
    ```jsx
    arr.map(callback(item[, index[, array]])
    ```
    
    - forEach의 매개 변수와 동일
    - 반환 값
        - 배열의 각 요소에 대해 실행한 ‘callback의 결과를 모운 새로운 배열’
        - forEach 동작 원리와 같지만 forEach와 달리 새로운 배열을 반환함
3. 예시 :
    - 배열을 순회하며 각 객체의 name 속성 값을 추출하기 (for … of와 비교)
    
    ```jsx
    // 1. for...of 와 비교
    const persons = [
      { name: 'Alice', age: 20 },
      { name: 'Bella', age: 21 }
    ]
    
    // 1.1 for...of
    let result1 = []
    for (cosnt person of persons) {
    	result.push(person.name)
    }
    console.log(result1) // ['Alice', 'Bella']
    
    // 1.2 map
    const result2 = persons.map(function (person) {
    	return person.name
    })
    console.log(result2) // ['Alice', 'Bella']
    ```
    
    - 화살표 함수 표기
    
    ```jsx
    // 2. 화살표 함수 표기
    const names = ['Alice', 'Bella', 'Cathy']
    
    const result3 = names.map(function (name) {
      return name.length
    })
    
    const result4 = names.map((name) => {
    	return name.length
    })
    
    console.log(result3) // [5, 5, 5]
    console.log(result4) // [5, 5, 5]
    ```
    
    - 커스텀 콜백 함수
    
    ```jsx
    // 3. 커스텀 콜백 함수
    const numbers = [1, 2, 3]
    
    const myCallBackFunc = function (number) {
    	return number * 2
    }
    
    const doubleNumber = numbers.map(myCallBackFunc)
    
    console.log(doubleNumber) // [2, 4, 6]
    ```
    

### 배열 순회 종합

| 방식 | 특징 | 비고 |
| --- | --- | --- |
| for loop | - 배열의 인덱스를 이용하여 각 요소에 접근<br>- break, continue 사용 가능 |  |
| for of | - 배열 요소에 바로 접근 가능<br>-  break, continue 사용 가능 |  |
| forEach() | - 간결하고 가독성이 높음<br>- callback 함수를 이용하여 각 요소를 조작하기 용이<br>- break, continue 사용 불가 | 사용 권장 |

### 기타 Array Helper Methods

| 메서드 | 역할 |
| --- | --- |
| filter | 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환 |
| find | 콜백 함수의 반환 값이 참이면 해당 요소를 반환 |
| some | 배열의 요소 중 적어도 하나라면 콜백 함수를 통과하면 true를 반환하며 즉시 배열 순회 중지, 반면에 모두 통과하지 않으면 false를 반환 |
| every | 배열의 모든 요소가 콜백 함수를 통과하면 true를 반환, 반면에 하나라도 통과하지 못하면 즉시 false를 반환하고 배열 순회 중지 |
1. some 예시 :
    
    ```jsx
    const array = [1, 2, 3, 4, 5]
    
    // some
    // - 배열의 요소 중 적어도 하나라도 콜백 함수를 통과하는지 테스트
    // - 콜백 함수가 배열 요소 적어도 하나라도 참이면 true를 반환하고 순회 중지
    // - 그렇지 않으면 false를 반환
    const isEvenNumber = array.some(function (number) {
      return number % 2 === 0
    })
    
    console.log(isEvenNumber) // true
    ```
    
2. every 예시 :
    
    ```jsx
    // every
    // - 배열의 모든 요소가 콜백 함수를 통과하는지 테스트
    // - 콜백 함수가 모든 배열 요소에 대해 참이면 true를 반환
    // - 그렇지 않으면 false를 반환하고 순회 중지
    
    const isAllEvenNumber = array.every(function (number) {
      return number % 2 === 0
    })
    
    console.log(isAllEvenNumber) // false
    ```
    
3. forEach를 break하는 대안
    - some과 every의 특징을 활용해 마치 break를 사용하는 것처럼 활용 할 수 있음
    
    ```jsx
    // [forEach를 break 하는 대안]
    // some과 every의 특징을 이용하여 마치 forEach에서 break를 사용하는 것처럼 구현할 수 있음
    const names = ['Alice', 'Bella', 'Cathy']
    
    // 1. some
    // - 콜백 함수가 true를 반환하면 some 메서드는 즉시 중단하고 true를 반환
    names.some(function (name) {
      if (name === 'Bella') {
        return true
      }
      return false
    })
    
    // 2. every
    // - 콜백 함수가 false를 반환하면 every 메서드는 즉시 중단하고 false를 반환
    names.every(function (name) {
      if (name === 'Bella') {
        return false
      }
      return true
    })
    ```
    

## 배열 with 전개구문

1. 설명 : 
    - 배열 복사
2. 예시 :
    
    ```jsx
    // 배열 복사 (with 전개 구문)
    let parts = ['어깨', '무릎']
    let lyrics = ['머리', ...parts, '발]
    
    console.log(lyrics) // [ '머리', '어깨', '무릎', '발' ]
    ```