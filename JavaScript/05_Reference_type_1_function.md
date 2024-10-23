# Reference type : Function
## 함수 정의

1. Function :
    - 참조 자료형에 속하며 모든 함수는 Function object
        - 참조 자료형의 특징 : 객체의 주소가 저장됨, 가변, 주소가 복사
2. 구조 :
    
    ```jsx
    function name ([param[, param,[..., param]]]) {
    	staments
    	return value
    }
    ```
    
    - `function` 키워드
    - 함수의 이름
    - 함수의 매개변수
    - 함수의 body를 구성하는 statements
    - return 값이 없다면 undefined를 반환
3. 함수 정의의 2가지 방법
    - 선언식 function declaration
    - 표현식 function expression

### 선언식 function declaration

1. 구조 :
    
    ```jsx
    function funcName () {
    	statements
    }
    ```
    
2. 예시
    
    ```jsx
    function add (num1, num2) {
    	return num1 + num2
    }
    
    add(1, 2) // 3
    ```
    
3. 특징
    - 호이스팅 됨
    - 코드의 구조와 가독성 면에서는 표현식에 비해 장점이 있음

### 표현식 function expression

1. 구조
    
    ```jsx
    const funcName = function ()
    	statements
    }
    ```
    
    - 함수 이름이 없음
2. 예시
    
    ```jsx
    const sub = function (num1, num2) {
    	return num1 - num2
    }
    
    sub(2, 1) // 1
    ```
    
3. 특징
    - 호이스팅 되지 않음
        - 변수 선언만 호이스팅 되고 함수 할당은 실행 시점에 이루어짐
    - 함수 이름이 없는 익명 함수를 사용할 수 있음
4. 함수 표현식 사용을 권장하는 이유
    - 예측 가능성
        - 호이스팅의 영향을 받지 않아 코드의 실행 흐름을 더 명확하게 예측할 수 있음
    - 유연성
        - 변수에 할당되므로 함수를 값으로 다루기 더 쉬움
    - 스코프 관리
        - 블록 스코프를 가지는 let이나 const와 함께 사용하여 더 엄격한 스코프 관리가 가능

## 매개변수

1. 매개변수 정의 방법
    - 기본 함수 매개변수
    - 나머지 매개변수
2. 기본 함수 매개변수 Default function parameter
    - 전달하는 인자가 없거나 undefined가 전달된 경우 이름 붙은 매개변수를 기본값으로 초기화
    - 예시
        
        ```jsx
        const greeting = function (name = 'Anonymous') {
        	return `Hi ${name}`
        }
        
        greeting() // Hi Anonymous
        ```
        
3. 나머지 매개변수 Rest parameters
    - 임의의 수의 인자를 **배열**로 허용하여 가변 인자를 나타내는 방법
    - 작성 규칙
        - 함수 정의 시 나머지 매개변수는 하나만 작성할 수 있음
        - 나머지 매개변수는 함수 정의에서 매개변수 마지막에 위치해야 함
    - 예시
        
        ```jsx
        const myFunc = function (param1, param2, ...restPrams) {
        	return [para1, param2, restPrams]
        }
        
        myFunc(1, 2, 3, 4, 5) // [1, 2, [3, 4, 5]]
        myFunc(1, 2) // [1, 2, []]
        ```
        
4. 매개변수와 인자 개수가 불일치 할 때
    - 매개변수 개수 > 인자 개수
        
        ```jsx
        const threeArgs = function (num1, num2, num3) {
        	return [num1, num2, num3]
        }
        
        console.log(threeArgs()) // [undefined, undefined, undefined]	
        ```
        
        - 누락된 인자는 undefined로 할당
    - 매개변수 개수 < 인자 개수
        
        ```jsx
        const noArgs = function () {
        	return 0
        }
        
        noArgs(1, 2, 3) // 0
        
        const twoArgs = function (param1, param2) {
        	return [param1, param2]
        }
        twoArgs(1, 2, 3) // [1, 2]
        ```
        
        - 초과 입력한 인자는 사용하지 않음

## 전개구문 `...` Spread syntax

1. 설명 :
    - 배열이나 문자열과 같이 반복 가능한 항목을 펼치는 것 (확장, 전개)
    - 전개 대상에 따라 역할이 다름
        - 배열이나 객체의 요소를 개별적인 값으로 분리하거나 다른 배열이나 객체의 요소를 현재 배열이나 객체에 추가하는 등
2. 활용처 :
    - 함수와의 사용
        - 함수 호출 시 인자 확장
        - 나머지 매개변수 압축
    - 객체와의 사용 (객체 파트에서 진행)
    - 배열과의 사용 (배열 파트에서 진행)
3. 함수와의 사용 : 함수 호출시 인자 확장
    
    ```jsx
    function myFunction(x, y, z) {
    	return x + y + z
    }
    
    let numbers = [1, 2, 3]
    
    console.log(myFunc(...numbers)) // 6
    ```
    
4. 함수와의 사용 : 나머지 매개변수 압축
    
    ```jsx
    const myFunc = function (param1, param2, ...restPrams) {
    	return [para1, param2, restPrams]
    }
    
    myFunc(1, 2, 3, 4, 5) // [1, 2, [3, 4, 5]]
    myFunc(1, 2) // [1, 2, []]
    ```
    

## 화살표 함수 표현식 Arrow Function expression

1. 설명 : 함수 표현식의 간결한 표현법
2. 화살표 함수로 변경 결과
    
    ```jsx
    const arrow = function (name) {
    	return `helllo ${name}`
    }
    
    const arrow = name => `hello, ${name}`
    ```
    

### 작성 과정

1. function 키워드 제거 후 매개변수와 중괄표 사이 화살표(`=>`) 작성
    
    ```jsx
    const arrow1 = function (name) {
    	return `hello ${name}`
    }
    
    // 1. function 키워드 삭제 후 화살표 작성
    cosnt arrow2 = (name) => { reutrn `hello, ${name}` }
    ```
    
2. 함수의 매개변수가 하나 뿐이라면, 매개변수의 ‘()’ 제거 가능
    - 단, 생략하지 않는 것을 권장
    
    ```jsx
    // 2. 인자의 소괄호 삭제 (인자가 1개일 경우에만 가능)
    const arrow3 = name => { return `hello, ${name}` }
    ```
    
3. 함수의 본문의 표현식이 한 줄이라면, `{}` 와 `return` 제거 가능
    
    ```jsx
    // 3. 중괄호와 return 삭제
    const arrow 4 = name => `hello, ${name}`
    ```
    

### 심화

```jsx
// 1. 인자가 없다면 () or _ 로 표시 가능
const noArgs1 = () => 'No args'
const noArges2 = _ => 'No args'

// 2-1. object를 return 한다면 return을 명시적으로 작성해야 함
const returnObject1 = () => { return { key : 'value' } }

// 2-2. return을 작성하지 않으려면 객체를 소괄호로 감싸야 함
const returnObject2 = () => ({ key : 'value '})
```