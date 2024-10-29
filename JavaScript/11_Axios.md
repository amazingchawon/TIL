# Axios

1. 설명 :
    - 브라우저와  Node.js에서 사용할 수 있는 Promise 기반의 HTTP 클라이언트 라이브러리
    - 클라이언트 및 서버 사이에 HTTP 요청을 만들고 응답을 처리하는 데 사용되는 자바스크립트 라이브러리
    - 서버와 HTTP 요청과 응답을 간편하게 처리할 수 있도록 도와주는 도구
    - 브라우저를 위한 XHR 객체 생성
    - 간편한 API를 제공하며, Promise 기반의 비동기 요청을 처리
    - 주로 웹 어플리케이션에서 서버와 비동기적으로 통신할 때 사용
2. Ajax를 활용한  클라이언트 서버 간 동작
    - 클라이언트 : XHR 객체 생성 및 요청
    - 서버 : 응답 데이터 생성 → JSON 데이터 응답
    - 클라이언트 : Promise 객체 데이터를 활용해 DOM 조작 (웹 페이지의 일부분 만을 다시 로딩)

## Axios 실습

1. 설치
    - CDN 방식으로 사용하기

### Promise object

1. 설명 :
    - 자바 스크립트에서 비동기 작업을 처리하기 위한 객체
        - axios 객체의 리턴 값
    - 비동기 작업의 최종 완료(또는 실패)와 그 결과 값을 나타냄
2. 구조 :
    
    ```jsx
    // 1.
    const promiseObj = axios({
      method: 'get',
      url: 'https://api.thecatapi.com/v1/images/search'
    })
    
    console.log(promiseObj) // Promise object
    ```
    
3. 주요 메서드
    
    ```jsx
    //promiseObj.then().catch()
    
    promiseObj
    	.then((response) => {
    		console.log(response) // Response object
    		console.log(response.data) // Response data
    	})
    	.catch(error) => {
    		console.error(error)
    	})
    ```
    
    - `then(callback)` : 작업이 성공적으로 완료되었을 때 실행될 콜백 함수를 지정
        - 요청한 작업이 성공하면 callback을 실행
        - callback은 이전 작업의 성공 결과를 인자로 전달 받음
    - `catch(callback)` : 작업이 실패했을 때 실행될 콜백 함수를 지정
        - then()이 하나라도 실패하면 callback 실행 (남은 then은 중단)
        - callback은 이전 작업의 실패 객체를 인자로 전달받음
4. 비동기적으로 동작
    
    ```jsx
    promiseObj
      .then((response) => {
        console.log(response)
      })
      .catch((error) => {
        console.log(error)
      })
    
    console.log('안녕하세요')
    ```
    
    - 안녕하세요가 더 먼저 출력됨

### Axios 기본 구조

1. 구조 : 
    
    ```jsx
    // 2.
    axios({
      method: 'get',
      url: 'https://api.thecatapi.com/v1/images/search'
    })
    	.then((response) => {
    		console.log(response) // Response object
    		console.log(response.data) // Response data
    	})
    	.catch(error) => {
    		console.error(error)
    	})
    ```
    
    - axios 객체를 활용해 요청을 보낸 후 응답 데이터 promise 를 객체로 받음
2. 성공 처리
    - then 메서드를 사용해서 성공했을 때 수행할 로직을 작성
    - 서버로부터 받은 응답 데이터를 처리
3. 실패 처리
    - catch 메서드를 사용해서 실패했을 때 수행할 로직을 작성
    - 네트워크 오류나 서버 오류 등의 예외 상황을 처리

### Axios 활용

1. 고양이 사진 가져오기 실습 1
    
    ```jsx
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      const URL = 'https://api.thecatapi.com/v1/images/search'
      // 1. axios 요청 처리
      axios({
        method: 'get',
        url: URL
      })
        .then((response) => {
          console.log(response)  // 객체
          console.log(response.data)
          console.log(response.data[0])
          console.log(response.data[0].url)
        })
        .catch((error) => {
          console.log(error)
          console.log('실패했다옹')
        })
      console.log('야옹야옹')
    </script>
    ```
    
2. 고양이 사진 가져오기 실습 2
    - 내용
        - 버튼을 누르면
        - 고양이 이미지를 요청하고
        - 요청이 처리되어 응답이 오면
        - 응답 데이터에 있는 이미지 주소 값을 img 태그에 넣어 이미지 출력하기
    - 코드
    
    ```jsx
    const URL = 'https://api.thecatapi.com/v1/images/search'
    const btn = document.querySelector('button')
    
    const getCats = function () {
    	axios({
    		method: 'get',
    		url: URL
    	})
    		.then((response) => {
    			// 1. 이미지 주소 저장
    			const imgUrl = response.data[0].url
    			// 2. 이미지 태그 생성
    			const imgTag = document.createElement('img')
    			// 3. 이미지 태그의 src 값 설정
    			imgTag.setAttribute('src', imgUrl)
    			// 4. body의 자식 태그로 추가
    			document.body.appendChilr(imgTag)
    		})
    		.catch((error) => {
    			console.log(error)
    			console.log('실패했다옹')
    		})
    
    // 버튼에 이벤트 핸들러 부착
    btn.addEventListener('click', getCats)
    ```
    

## Ajax와 Axios

1. Ajax :
    - 하나의 특정한 기술을 의미하는 것이 아니라, 비동기적인 웹 애플리케이션 개발에 사용하는 기술들의 집합을 지칭
    - 개념, 접근 방식
2. Axios:
    - 클라이언트 및 서버 사이에 HTTP 요청을 만들고 응답을 처리하는 데 사용되는 자바 스크립트 라이브러리
    - Promise API를 기반으로 하여 비동기 처리를 더 쉽게 할 수 있음
    - ⇒ 프론트엔드에서 Axios를 활용해 DRF로 만든 API 서버로 요청을 보내고, 받아온 데이터를 비동기적으로 처리하는 로직을 작성하게 됨
    - Ajax를 실현하는 구체적인 도구

## Callback과 Promise

1. 비동기 처리의 특성과 관리
    - 특성 : 비동기 처리의 핵심은 작업이 시작되는 순서가 아니라 **완료되는 순서**에 따라 처리된다는 것
        - 어려움 :
            - 개발자 입장에서는 **코드의 실행 순서가 불명확하다**는 단점 존재
            - 이로 인해 실행 결과를 정확이 예측하며 코드를 작성하기 어려울 수 있음
    - 관리 방법 :
        - 비동기 콜백 :
            - 비동기 작업이 완료된 후 실행될 함수를 미리 정의
        - Promise :
            - 비동기 작업의 최종 완료 또는 실패를 나타내는 객체

### 비동기 콜백

1. 설명 :
    - 비동기적으로 처리되는 작업이 완료되었을 때 실행되는 함수
    - 연쇄적으로 발생하는 비동기 작업을 **순차적으로 동작**할 수 있게 함
        - 작업의 순서와 동작을 제어하거나 결과를 처리하는 데 사용
    - 코드
        
        ```jsx
        const asyncTask = function (callback) {
        	setTimout(function(){
        		console.log('비동기 작업 완료')
        		callback() // 작업 완료 후 콜백 호출
        	}, 2000) // 2초 후에 작업 완료
        }
        
        // 비동기 작업 수행 후 콜백 실행
        asyncTask(function () {
        	console.log('작업 완료 후 콜백 실행')
        })
        
        // 출력 순서
        // 비동기 작업 완료
        /// 작업 완료 후 콜백 실행
        ```
        
2. 한계 :
    - 비동기 콜백 함수는 보통 어떤 기능의 실행 결과를 받아서 다른 기능을 수행하기 위해 많이 사용 됨
    - 이 과정을 작성하다 보면 비슷한 패턴이 계속 발생
        - A를 처리해서 결과가 나오면, 첫번째 callback 함수를 실행하고…
        - 첫번째 callback 함수가 종료되면, 두번째 callback 함수를 실행하고…
        - 두번째 callback 함수가 종료되면, 세번째 callback 함수를…
    - 콜백 지옥, 파멸의 피라미드 (Pyramid of doom)이라고 부름
3. 정리 :
    - 콜백 함수는 비동기 작업을 순차적으로 실행할 수 있게 하는 반드시 필요한 로직
    - 비동기 코드를 작성하다 보면 콜백 함수로 인한 콜백 지옥은 빈번히 나타나는 문제이며 이는 코드의 가독성을 해치고 유지 보수가 어려워짐

### Promise

1. 설명 :
    - JavaScript에서 비동기 작업의 결과를 나타내는 객체
    - 비동기 작업이 완료되었을 때 결과 값을 반환하거나, 실패 시 에러를 처리할 수 있는 기능을 제공
    - 콜백 지옥 문제를 해결하기 위해 등장한 비동기 처리를 위한 객체
    - 작업이 끝나면 실행 시켜 줄게 라는 약속
    - Promise 기반의 HTTP 클라이언트 라이브러리가 바로 Axios
2. Then & catch의 chaining
    - axios로 처리한 비동기 로직은 항상 promise 객체를 반환
    - 즉, then과 catch는 모두 항상 promise 객체를 반환
        - 계속해서 chaining을 할 수 있음
    - then을 계속 이어 나가면서 작성할 수 있게 됨
3. chaining의 목적
    - 비동기 작업의 순차적인 처리 가능
    - 코드를 보다 직관적이고 가독성 좋게 작성할 수 있도록 도움
4. chaining의 장점
    - 가독성 : 비동기 작업의 순서와 의존 관계를 명확히 표현할 수 있어 코드의 가독성이 향상
    - 에러 처리 : 각각의 비동기 작업 단계에서 발생하는 에러를 분할에서 처리 가능
    - 유연성 : 각 단계마다 필요한 데이터를 가공하거나 다른 비동기 작업을 수행할 수 있어서 더 복잡한 비동기 흐름을 구성할 수 있음
    - 코드 관리 : 비동기 작업을 분리하여 구성하면 코드를 관리하기 용이
5. Promise가 제공하는 이점 (비동기 콜백과 비교)
    - 실행 순서의 보장
        - 콜백 함수 : JavaScript의 Event Loop가 현재 실행 중인 Call Stack을 완료하기 전에는 호출되지 않음
        - Promise : then/catch 메서드의 콜백 함수는 Event Queue에 배치되는 순서대로 엄격하게 호출됨
    - 유연한 비동기 처리
        - Promise는 비동기 작업이 완료된 후에도 then 메서드를 통해 콜백을 추가할 수 있음
    - 체이닝 Chaining 을 통한 연속적인 비동기 처리
        - then 메서드를 여러 번 연결하여 여러 개의 콜백 함수를 순차적으로 실행할 수 있음
        - 각 콜백은 주어진 순서대로 실행되며, 이전 Promise의 결과를 다음 then에서 사용할 수 있음
        - 복잡한 비동기 로직을 명확하게 표현할 수 있음
    - 에러 처리의 일원화
        - catch 메서드를 통해 Promise 체인 전체의 에러를 한 곳에서 처리할 수 있음
        - 전통적인 콜백 방식에서 각 콜백마다 에러 처리를 해야 하는 번거로움을 해소
    
    > Promise 정리
    > - Promise는 비동기 프로그래밍의 복잡성을 줄이고, 코드 가독성과 유지보수성을 높이는 강력한 도구
    > - 실행 순서 보장, 체이닝, 에러 처리 등의 특징을 통해 콜백 지옥을 피하고 더 체계적인 비동기 코드 작성을 가능하게 함