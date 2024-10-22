# 데이터 타입

1. 원시 자료형과 참조 자료형의 구분
    - 원시 자료형 : 변수에 값이 직접 저장되는 자료형 (불변, 값이 복사)
    - 참조 자료형 : 객체의 주소가 저장되는 자료형 (가변, 주소가 복사)

## 원시 자료형 Primitive type

1. 종류 :
    - Number
    - String
    - Boolean
    - null
    - undefined
2. 특징 :
    - 변수에 할당될 때 값이 복사됨 → 변수 간에 서로 영향을 미치지 않음
        
        ```jsx
        const a = 'bar'
        console.log(a) // bar
        
        a.toUpperCase() // 반환 값만 바꾸는 것, 원본은 바뀌지 않음
        console.log(a) // bar
        ```
        
        ```jsx
        let a = 10
        let b = a
        b = 20
        
        console.log(a) // 10
        console.log(b) // 20
        ```
        

### Number

1. 정의 : 정수 또는 실수형 숫자를 표현하는 자료형
2. 예시 :
    
    ```jsx
    const a = 13
    const b = -5
    const c = 3.14
    const d = Infinity
    const e = -Infinity
    const f = NaN // Not a Number를 나타내는 값
    ```
    
    - `NaN` : 주로 연산의 결과로 나타남
        - 숫자로서 읽을 수 없음
        - 결과가 허수인 수학 계산식
        - 피연산자가 NaN
        - 정의할 수 없는 계산식
        - 문자열을 포함하면서 덧셈이 아닌 계산식

### String

1. 정의 : 텍스트 데이터를 표현하는 자료형
2. 특징 :
    - `+` 연산자를 사용해 문자열끼리 결합
    - 뺄셈, 곱셉, 나눗셈 불가능
    
    ```jsx
    const firstName = 'Tony'
    const lastName = 'Stark'
    const fullName = firstName + lastName
    
    console.log(fullName) // TonyStark
    ```
    

### Template literals (템플릿 리터럴)

1. 설명 :
    - 내장된 표현식을 허용하는 문자열 작성 방식
    - Backtick(``)을 이용하며, 여러 줄에 걸쳐 문자열을 정의할 수도 있고 JavaScript의 변수를 문자열 안에 바로 연결할 수 있음
    - 표현식은 `$` 와 중괄호({expression})로 표기
    - ES6+ 부터 지원
2. 예시 :
    
    ```jsx
    const age = 100
    const message = `홍길동은 ${age}세 입니다.`
    console.log(message)
    ```
    

### null과 undefined

1. 사용법 :
    
    ```jsx
    let a = null
    console.log(a) // null
    
    let b // const는 이렇게 안됨
    console.log(b) // undefined
    ```
    
    - null : 프로그래머가 의도적으로 ‘값이 없음’을 나타낼 때 사용
    - undefined : 시스템이나 JavaScript 엔진이 ‘값이 할당되지 않음’을 나타낼 때 사용
2. 값이 없음에 대한 표현이 두가지인 이유
    - 역사적 맥락
        - JavaScript가 처음 만들어질 때 null은 객체가 없음을 나타내기 위해 도입
        - undefined는 나중에 추가되어 ‘값이 할당되지 않음’을 나타내게 됨
    - null의 타입이 object 인 이유
        - null 은 원시 자료형이라 object로 뜨면 안됨
        - 초기 버전에서 값의 타입을 나타내는 데 32비트 시스템을 사용
        - 타입 태그로 하위 3비트를 사용했는데, ‘000’은 객체를 나타냄
        - null은 모든 비트가 0인 특별한 값(null pointer)으로 표현되었고, 이로 인해 객체로 잘못 해석
    - ECMAScript의 표준화
        - ECMAScript 명세에서는 null을 원시 자료형으로 정의
        - 그러나 typeof null의 결과는 역사적인 이유로 object를 유지
        - ECMAScript 5 개발 중 이 문제를 수정하려는 시도가 있지만, 기존 웹 사이트들의 호환성 문제로 받아들여지지 않음

### Boolean

1. 설명 :
    - 값 : true / false
    - 조건문 또는 반복문에서 Boolean이 아닌 데이터 타입은 자동 형변환 규칙에 따라 true 또는 false로 변환됨
2. 자동 형변환
    
    
    | 데이터 타입 | false | true |
    | --- | --- | --- |
    | undefined | 항상 | X |
    | null | 항상  | X |
    | Number | 0, -0, NaN | 나머지 모든 경우 |
    | String | ‘’ (빈문자열) | 나머지 모든 경우 |

## 참조 자료형 Reference type

1. 종류 :
    - Object, Array, Function
2. 특징 :
    - 객체를 생성하면 객체의 메모리 주소를 변수에 할당 → 변수 간에 서로 영향을 미침
        
        ```jsx
        const obj1 = { name : 'Alice', age : 30 }
        const obj2 = obj1
        obj.age = 40
        
        console.log(obj1.age) // 40
        console.log(obj2.age) // 40
        ```
        
        ```jsx
        const arr1 = [1, 2, 3]
        const arr2 = arr1
        arr2.push(4)
        
        console.log(arr1) // [1, 2, 3, 4]
        console.log(arr2) // [1, 2, 3, 4]
        ```
        

## 연산자

### 할당 연산자

1. 설명 :
    - 오른쪽에 있는 피연산자의 평가 결과를 왼쪽에 피연산자에 할당하는 연산자
    - 단축 연산자 지원

### 증가 & 감소 연산자

1. 증가 연산자 `++`
    - 피연산자를 증가(1을 더함) 시키고 연산자의 위치에 따라 증가하기 전이나 후의 값을 반환
2. 감소 연산자 `--`
    - 피연산자를 감소(1을 뺌) 시키고 연산자의 위치에 따라 감소하기 전이나 후의 값을 반환
3. `+=` 혹은 `-=` 와 같이 더 명시적인 표현으로 작성하는 것을 권장
4. 예시 :
    
    ```jsx
    // "전위 연산자"
    // 피연산자에 1을 더한 값을 반환
    // a에 +1을 할당한 후의 값 4를 반환
    let a = 3
    const b = ++a
    console.log(a, b) // 4 4
    
    // "후위 연산자"
    // 피연산자에 1을 더하기 전의 값을 반환
    // x를 먼저 반환한 후 x에 +1을 할당
    let x = 3
    const y = x++
    console.log(x, y) // 4 3
    ```
    

### 비교 연산자

1. 피연산자들 (숫자, 문자, Boolean 등)을 비교하고 결과 값을 boolean으로 반환하는 연산자
2. 예시 :
    
    ```jsx
    3 > 3 // true
    
    'Z' < 'a' // true
    '가' < '나' // true
    ```
    

### 동등 연산자 (==)

1. 설명 :
    - 두 피연산자가 같은 값으로 평가되는지 비교 후 boolean 값을 변환
    - ‘암묵적 타입 변환’ 통해 타입을 일치시킨 후 같은 값인지 비교
        - 내부적으로 어떤 것을 변환 시키는지에 대한 순서가 정해져있지만, 무엇이 변환되는지는 중요치 않다
    - 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별
2. 예시 :
    
    ```jsx
    console.log(1 == 1) // true
    console.log('1' == 1) // true
    console.log(0 == false) // true
    ```
    

### 일치 연산자(===)

1. 설명 :
    - 두 피연산자의 값과 타입이 모두 같은 경우 true를 반환
    - 같은 객체를 가리키거나, 같은 타입이면서 같은 값인지를 비교
    - 엄격한 비교가 이뤄지며 암묵적 타입 변환이 발생하지 않음
    - 특수한 경우를 제외하고는 동등 연산자가 아닌 **일치 연산자 사용 권장**
2. 예시 :
    
    ```jsx
    console.log(1 === 1) // true
    console.log('1' === 1) // false
    ```
    

### 논리 연산자

1. and 연산 `&&`
2. or 연산 `||`
3. not 연산 `!`
4. 단축 평가 지원

### 삼항 연산자

1. 형태 :
    
    ```jsx
    condition ? expression 1: expression2
    ```
    
    - condition : 평가할 조건 (true 또는 false로 평가)
    - expression1 : 조건이 true일 경우에 반환할 값 또는 표현식
    - expression2 : 조건이 false일 경우에 반환한 값 또는 표현식
2. 사용법 :
    - 간단한 조건부 로직을 간결하게 표현할 때 유용
    - 복잡한 로직이나 대다수의 경우에는 가독성이 떨어질 수 있으므로 적절한 상황에서만 사용할 것
3. 예시 :
    
    ```jsx
    const age = 20
    const message = (age >= 18) ? '성인' : '미성년자'
    
    console.log(message) // '성인'
    ```
    

## 조건문

### if

1. 설명 : 조건 표현식의 결과값을 boolean 타입으로 변환 후 참/거짓을 반환
2. 예시 :
    
    ```jsx
    const name = 'customer'
    
    if (name === 'admin') {
    	console.log('관리자님 안녕')
    } else if {
    	console.log('고객님 환영해요')
    } else {
    	console.log(` 반갑습니다. ${name}님`)
    }
    ```
    

## 반복문

1. 종류 :
    - while
    - for
    - for … in
    - for … of

### while

1. 설명 : 조건문이 참이면 문장을 계속해서 수행
    
    ```jsx
    while (조건문) {
    	// do something
    }
    ```
    
2. 예시 :
    
    ```jsx
    let 1 = 0
    
    while (i < 6) {
    	console.log(i)
    	i += 1
    }
    ```
    

## for

1. 설명 : 특정한 조건이 거짓으로 판별될 때까지 반복
    
    ```jsx
    for ([초기문]; [조건문]; [증감문]) {
    	// do something
    }
    ```
    
2. 예시 :
    
    ```jsx
    for (let i = 0; i < 6; i++) {
    	console.log(i)
    }
    ```
    
3. 동작 원리 :
    - 반복문 진입 및 변수 i 선언
    - 조건문 평가 후 코드 블럭 실행
    - 코드 실행 이후 i 값 증가

### for … in

1. 설명 : 객체의 열거 가능한 속성(property)에 대해 반복
    ```jsx
    for (variable in object) {
    	statement
    }
    ```
    
2. 예시 :
    
    ```jsx
    const object = {
    	a: 'apple',
    	b: 'banana'
    }
    for (const property in object) {
    	console.log(property) // a, b
    	console.log(object[property]) // apple, banana
    ```
    

### for … of

1. 설명 : 반복 가능한 객체(배열, 문자열 등)에 대해 반복
    
    ```jsx
    for (variable of iterable) {
    	statement
    }
    ```
    
2. 예시 :
    
    ```jsx
    const = numbers = [0, 1, 2, 3]
    
    for (const number of numbers) {
    	console.log(number) // 0, 1, 2, 3
    }
    ```
    

### for … in 과 for .. of

1. 배열과 객체 비교
    
    ```jsx
    // for ... in
    // Array
    const arr = ['a', 'b', 'c']
    for (const elem in arr) {
    	console.log(elem) // 0, 1, 2
    }
    
    // Object
    const capitals = {
    	korea: '서울',
    	japan: '도쿄',
    	china: '베이징',
    }
    for (const capital in capitals) {
    	console.log(capital) // korea japan china
    }
    ```
    
    ```jsx
    // for ... of
    // Array
    const arr = ['a', 'b', 'c']
    for (const elem of arr) {
    	console.log(elem) // a b c
    }
    
    // Object
    const capitals = {
    	korea: '서울',
    	japan: '도쿄',
    	china: '베이징',
    }
    for (const capital of capitals) {
    	console.log(capital) // TypeError
    }
    ```
    
2. 배열 반복과 for … in
    - 객체 관점에서 배열의 인덱스는 “정수 이름을 가진 열거 가능한 속성”
    - for … in은 정수가 아닌 이름과 속성을 포함하여 열거 가능한 모든 속성을 반환
    - 내부적으로 for … in은 배열의 반복자가 아닌 속성 열거를 사용하기 때문에 특정 순서에 따라 인덱스를

### 반복문 사용시 const 사용 여부

1. for 문 :
    - `for (let i = 0; i < arr.length; i++) {}`
    - 최초 정의한 i를 재할당하면서 사용하기 때문에 const를 사용하면 에러 발생
2. for … in, for … of
    - 재할당이 아니라, 매 반복마다 다른 속성 이름이 변수에 지정되는 것이므로 const를 사용해도 에러가 발생하지 않음
    - 단, const 특징에 따라 블록 내부에서 수정할 수 없음

### 종합

| 키워드 | 특징 | 스코프 |
| --- | --- | --- |
| while | . | 블록스코프 |
| for | . | 블록스코프 |
| for … in | object 순회 | 블록스코프 |
| for … of | iterable 순회 | 블록스코프 |