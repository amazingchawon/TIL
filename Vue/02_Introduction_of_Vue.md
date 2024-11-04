# Introduction of Vue

1. 정의 : 사용자 인터페이스를 구축하기 위한 JavaScript 프레임워크
2. 설명 :
    - 2014 : Evan You에 의해 발표
        - Angular 개발팀 출신
    - 최신 버전 : Vue 3
        - Vue 2 문서에 접속하지 않도록 주의
3. Vue를 학습하는 이유
    - 낮은 학습 곡선
        - 간결하고 직관적인 문법
        - 잘 정리된 문서
    - 확장성과 생태계
        - 다양한 플러그인과 라이브러리를 제공하는 높은 확장성
        - 전세계적으로 활성화된 커뮤니티
    - 유연성 및 성능
    - 주목받는 Client-Side framework
4. Vue의 주요 특징 정리
    - 반응형 데이터 바인딩
        - 데이터 변경 시 자동 UI 업데이트
    - 컴포넌트 기반 아키텍쳐
        - 재사용 가능한 UI 조각
    - 간결한 문법과 직관적인 API
    - 유연한 스케일링
        - 작은 프로젝트부터 대규모 애플리케이션까지 적합

## Vue의 2가지 핵심 기능

1. 선언적 렌더링 Delcarative Rendering
    - 표준 HTML을 확장하는 Vue 템플릿 구문을 사용하여 JavaScript 상태(데이터)를 기반으로 화면에 출력될 HTML을 선언적으로 작성
    - 이전에 하던 DOM 요소를 선택하는 명령형 렌더링과 차이
2. 반응성 Reactivity
    - JavaScript 상태 변경을 추적
    - 변경사항이 발생하면 자동으로 DOM을 업데이트
3. 실습
    
    ```html
    <div id="app">
      <h1>{{ message }}</h1>
      <button v-on:click="countNumber++">
        카운트 넘버: {{ countNumber }}
      </button>
    </div>
    
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script>
      const { createApp, ref } = Vue
      
      const app = createApp({
        setup() {
          const message = ref('Hello vue!')
          const countNumber = ref(0)
          return {
            message,
            countNumber
          }
        }
      })
    
      app.mount('#app')
    </script>
    ```
    

## Component

1. 정의 : 재사용 가능한 코드 블록
2. 특징 :
    - UI를 독립적이고 재사용 가능한 일부분으로 분할하고 각 부분을 개별적으로 다룰 수 있음
    - 자연스럽게 애플리케이션은 중첩된 Component의 트리 형태로 구성됨

## Vue Application

1. Vue를 사용하는 방법
    - CDN 방식
    - NPM 설치 방식
2. Vue Application 생성
    
    ```html
    <div id="app"></div>
    
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <script>
      const { createApp } = Vue
      const app = createApp({})
      
      app.mount('#app')
    </script>
    ```
    
    - CDN 작성
    - Application instance
        - `const { createApp } =Vue`
            - CDN에서 Vue를 사용하는 경우 전역 Vue 객체를 불러오게 됨
            - 구조분해할당 문법으로 Vue 객체의 creatApp 함수를 할당
        - `const app = createApp({})`
        - 모든 Vue 애플리케이션은 createApp 함수로 새 Application instance를 생성하는 것으로 시작함
    - Root Component
        - `({})`
        - createApp 함수에는 객체(컴포넌트)가 전달됨
        - 모든 App에는 다른 컴포넌트들을 하위 컴포넌트로 포함할 수 있는 Root(최상위) 컴포넌트가 필요 (현재는 단일 컴포넌트)
    - Mounting the App (앱 연결)
        - `app.mount('#app')`
        - HTML 요소에 Vue Application instance를 탑재 (연결)
        - 각 앱 인스턴스에 대해 mount()는 한 번만 호출 할 수 있음

## 반응형 상태

### `ref()`

1. 정의 :
    - 반응형 상태(데이터)를 선언하는 함수
    - 반응형을 가지는 참조 변수를 만드는 것
    - ref === reactive reference
2. 예시 :
    
    ```html
    <script>
      const { createApp } = Vue
    
      const app = createApp({
        setup() {
          const message = ref('hello')
          console.log(message) // ref 객체
          console.log(message.value) // 'hello'
          return {
    	      message
          }
        }
      })
    </script>
    ```
    
3. 설명 :
    - `.value` 속성이 있는 ref 객체로 래핑(wrapping)하여 반환하는 함수
    - ref로 선언된 변수의 값이 변경되면, 해당 값을 사용하는 템플릿에서 자동으로 업데이트
    - 인자는 어떤 타입도 가능
    - 템플릿의 참조에 접근하려면 setup 함수에서 선언 및 반환 필요
    - 편의상 템플릿에서 ref를 사용할 때는 .value를 작성할 필요 없음 (automatically unwrapped)
4. ref 객체가 필요한 이유
    - 일반적인 변수가 아닌 객체 데이터 타입으로 사용하는 이유는?
    - Vue는 템플릿에서 ref를 사용하고 나중에 ref의 값을 변경하면 자동으로 변경 사항을 감지하고 그에 따라 DOM을 업데이트 함 (의존성 추적 기반의 반응형 시스템)
    - Vue는 렌더링 중에 사용된 모든 ref를 추적하며 나중에 ref가 변경되면 이를 추적하는 구성 요소에 대해 다시 렌더링
    - 이를 위해서 참조 자료형의 객체 타입으로 구현한 것
        - JavaScript에서는 일반 변수의 접근 또는 변형을 감지할 방법이 없기 때문
5. ref Unwrap 주의사항
    - 템플릿에서 unwrap은 ref가 최상위 속성인 경우에만 적용가능
    - 예시
        
        ```html
        const object = { id: ref(0) }
        {{ object.id + 1 }} // [object Object]1
        ```
        
        ```html
        const object = { id: ref(0)
        const { id } = object
        {{ id + 1 }}
        ```
        
    - 단, ref가 {{ }}의 최종 평가 값인 경우는 unwrap 가능
        
        ```html
        {{ object.id }} // object.id.value랑 동일
        ```
        

## Vue 기본 구조

1. Vue 컴포넌트
    
    ```jsx
    const app = createApp({
    	setup() {
    		const message = ref('Hello vue!')
    		return {
    			message
    		}
    	}
    })
    ```
    
    - createApp()에 전달되는 객체는 Vue 컴포넌트
    - 컴포넌트의 상태는 setup() 함수 내에서 선언되어야 하며 객체를 반환해야 함
2. 템플릿 렌더링
    
    ```html
    <div id="app">
    	<h1>{{ message }}</h1>
    </div>
    ```
    
    - 반환된 객체의 속성은 템플릿에서 사용할 수 있음
    - Mustache syntax를 사용하여 메서지 값을 기반으로 동적 텍스트를 렌더링
    - 콘텐츠 식별자나 경로에만 국한되지 않으며 유효한 JavaScript 표현식을 사용할 수 있음
        
        ```html
        <h1>{{ measage.split(''),reverse().join('') }}</h1>
        ```
        
3. Event Listeners in Vue
    
    ```html
    <div id="app">
    	<button v-on:click="increment">{{ count }}</button>
    </div>
    ```
    
    ```jsx
    const { createApp, ref } = Vue
    
    const app = createApp({
    	setup() {
    		const count = ref(0)
    		const increment = function () {
    			count.value++
    		}
    		return {
    			count,
    			increment
    		}
    	}
    })
    ```
    
    - `v-on` directive를 사용하여 DOM 이벤트를 수신할 수 있음
    - 함수 내에서 반응형 변수를 변경하여 구성 요소 상태를 업데이트