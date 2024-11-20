# Vue Router

## Routing

1. 설명 :
    - 네트워크에서 경로를 선택하는 프로세스
    - 웹 애플리케이션에서 다른 페이지 간의 전환과 경로를 관리하는 기술
2. SSR에서의 Routing
    - 서버 측에서 수행
    - 서버가 사용자가 방문한 URL 경로를 기반으로 응답을 전송
    - 링크를 클릭하면 브라우저는 서버로부터 HTML 응답을 수신하고 새 HTML로 전체 페이지를 다시 로드
3. CSR에서의 Routing
    - 클라이언트 측에서 수행
    - 클라이언트 측 JavaScript가 새 데이터를 동적으로 가져와 전체 페이지를 다시 로드하지 않음
4. SPA에서 Routing이 없다면
    - 유저가 URL을 통한 페이지의 변화를 감지할 수 없음
    - 페이지가 무엇을 렌더링 중인지에 대한 상태를 알 수 없음
        - URL이 1개이기 때문에 새로 고침 시 처음 페이지로 돌아감
        - 링크를 공유할 시 첫 페이지만 공유 가능
    - 브라우저의 뒤로 가기 기능을 사용할 수 없음
    - → 페이지는 1개이지만, 주소에 따라 여러 컴포넌트를 새로 렌더링하여 마치 여러 페이지를 사용하는 것처럼 보이도록 해야 함

## Vue Router

1. 설명 : Vue 공식 라우터
2. 사전 준비 :
    - Vite로 프로젝트 생성 시 Router 추가
    - Home, About 링크에 따라 변경되는 URL과 새로 렌더링 되는 화면
3. Vue 프로젝트 구조 변화
    - App.vue 코드 변화
    - router 폴더 신규 생성
    - views 폴더 신규 생성

### App.vue 코드 변화

1. RouterLink : 
    - 페이지를 다시 로드하지 않고 URL을 변경하여 URL 생성 및 관련 로직을 처리
    - HTML의 <a> 태그를 렌더링
2. RouterView :
    - RouterLink URL에 해당하는 컴포넌트를 표시
    - 원하는 곳에 배치하여 컴포넌트를 레이아웃에 표시할 수 있음

### router 폴더 신규 생성

1. router/index.js
    - 라우팅에 관련된 정보 및 설정이 작성되는 곳
    - router에 URL과 컴포넌트를 맵핑

### views 폴더 신규 생성

1. 설명 : 
    - RouterView 위치에 렌더링 할 컴포넌트를 배치
    - 기존 components 폴더와 기능적으로 다른 것은 없음
        - 단순 분류를 위해
        - 일반 컴포넌트와 구분하기 위해 컴포넌트 이름을 View로 끝나도록 작성하는 것을 권장

## Basic Routing

1. 라우팅 기본
    - index.js에 라우터 관련 설정 작성(주소, 이름, 컴포넌트)
        
        ```jsx
        // index.js
        
        const router = createRouter({
          routes: [
            {
              path: '/',
              name: 'home',
              component: HomeView,
            },
          ]
        })
        ```
        
    - RouterLink의 to 속성으로 index.js에서 정의한 주소 값(path)을 사용
        
        ```jsx
        <!-- App.vue -->
        
        <RouterLink to="/">Home</RouterLink>
        ```
        
    - RouterLink 클릭 시 경로와 일치하는 컴포넌트가 RouterView에서 렌더링 됨
        
        ```jsx
        <!-- App.vue -->
        
        <RouterView />
        ```
        

### Named Routes

1. 설명 : 경로에 이름을 지정하는 라우팅
2. 예시 :
    - name 속성 값에 경로에 대한 이름을 지정
    - 경로에 연결하려면 RouterLink에 v-bind를 사용해 ‘to’ props 객체로 전달
    
    ```jsx
    // index.js
    
    const router = createRouter({
    	routes : [{
    			path = '/',
    			name = 'home',
    			component: HomeView
    		},
    		...
    	]
    })
    ```
    
    ```jsx
    <RouterLink :to="{ name: 'home' }">Home</RouterLink>
    ```
    
3. 장점 :
    - 하드 코딩된 URL을 사용하지 않아도 됨
    - URL 입력시 오타 방지

### Dynamic Route Matching

1. 설명 : 
    - URL의 일부를 변수로 사용하여(매개변수를 사용한) 동적 경로 매칭
2. 활용
    - views 폴더 내 UserView 컴포넌트 작성
        
        ```jsx
        <!-- UserView.vue -->
        
        <template>
        	<div>
        		<h1>UserView</h1>
        	</div>
        </template>
        ```
        
    - index.js 작성
        - 매개변수는 콜론으로 표기
        
        ```jsx
        import UserView from '@/views/UserView.vue'
        
        const router = createRouter({
          history: createWebHistory(import.meta.env.BASE_URL),
          routes: [
            {
              path: '/user/:id',
              name: 'user',
              component: UserView
            }
          ],
        })
        ```
        
    - App.vue 작성
        
        ```jsx
        <script setup>
        import { ref } from 'vue'
        const userId = ref(1)
        </script>
        
        <template>
          <nav>
            <RouterLink to="/">Home</RouterLink>
            <RouterLink to="/about">About</RouterLink>
            <RouterLink :to="{ name: 'user', params: { id: userId }}">User</RouterLink>
          </nav>
        ```
        
        - 매개변수는 객체의 params 속성의 객체 타입으로 전달
        - 단, 객체의 key와 index.js에서 지정한 매개변수 이름이 같아야 함
    - 내부 변수
        
        ```jsx
        <template>
          <div>
            <h1>UserView</h1>
            <h2>{{ $route.params.id }} User 페이지</h2>
          </div>
        </template>
        ```
        
        - 경로가 일치하면 라우트의 매개변수는 컴포넌트에서 $route.params로 참조 가능
            - 이 방법은 권장하지 않음
        
        ```jsx
        <template>
          <div>
            <h1>UserView</h1>
            <h2>{{ userId }} User 페이지</h2>
          </div>
        </template>
        
        <script setup>
        import { useRoute, useRouter } from 'vue-router'
        import { ref } from 'vue'
        
        const route = useRoute()
        const userId = ref(route.params.id)
        </script>
        ```
        
        - userRoute() 함수를 사용해 스크립트 내에서 반응형 변수에 할당 후 템플릿에 출력하는 것을 권장
        - 템플릿에서 $route를 사용하는 것과 동일

## Nested Routes

1. 설명 :
    - 중첩된 라우팅
    - 예시 → 여기서 johnny는 변수
        
        https://velog.velcdn.com/images/limsungwoo/post/1b3fca87-0276-49b9-a622-90c6a2d74d44/image.png
        
2. 중첩된 라우팅 활용
    - 컴포넌트 생성
        - components 폴더에 UserProfile, UserPosts 컴포넌트 작성
    - 라우터 등록
        - index.js에 두 컴포넌트를 import
        
        ```jsx
        {
          path: '/user/:id',
          name: 'user',
          component: UserView,
          children: [
            { path: 'profile', name: 'user-profile', component: UserProfile},
            { path: 'posts', name: 'user-posts', component: UserPosts},
          ]
        }
        ```
        
        - 중첩된 Named Routes를 다룰 때는 일반적으로 “하위 경로에만 이름을 지정”
        - 이렇게 하면  /user/:id로 이동했을 때 항상 중첩된 경로가 표시 됨
        
        ```jsx
        {
          path: '/user/:id',
          name: 'user',
          component: UserView,
          children: [
        	  { path: '', name: 'user', component: UserProfile},
            { path: 'profile', name: 'user-profile', component: UserProfile},
            { path: 'posts', name: 'user-posts', component: UserPosts},
          ]
        }
        ```
        
    - 두 컴포넌트에 대한 RouterLink 및 RouterView 작성
        
        ```jsx
        <!-- UserView.vue -->
        
        <template>
          <div>
            <h1>UserView</h1>
            <RouterLink :to="{ name: 'user-profile'}">Profile</RouterLink>
            <RouterLink :to="{ name: 'user-posts'}">Posts</RouterLink>
            <h2>{{ userId }} User 페이지</h2>
            <RouterView />
          </div>
        </template>
        ```
        
3. 주의 
    - 컴포넌트 간 부모-자식 관계 관점이 아님
    - URL에서의 중첩된 관계를 표현하는 관점으로 바라보기

### Children

1. 설명 : children 옵션은 배열 형태로 필요한 만큼 중첩 관계를 표현할 수 있음

## Programmatic navigation

1. 설명 :
    - RouterLink 대신 JavaScript를 사용해 페이지를 이동하는 것
    - 프로그래밍으로 URL 이동하기
    - router의 인스턴스 메서드를 사용해 RouterLink로 <a> 태그를 만드는 것처럼 프로그래밍으로 네비게이션 관련 작업을 수행할 수 있음
2. 메서드 :
    - `router.push()` : 다른 위치로 이동하기
        - 새 항목을 history stack에 push하므로 사용자가 브라우저 뒤로 가기 버튼을 클릭하면 이전 URL로 이동할 수 있음
        - RouterLink를 클릭했을 때 내부적으로 호출되는 메서드이므로 RouterLink를 클릭하는 것은 router.push()를 호출하는 것과 같음
        - 선언적 표현 : `<RouterLink :to="...">`
        - 프로그래밍적 표현 : `router.push(...)`
        - 예시
            
            ```jsx
            <template>
              <div>
            		...
                <button @click="goHome">홈으로</button>
                <RouterView />
              </div>
            </template>
            
            <script setup>
            import { useRoute, RouterLink, RouterView, useRouter } from 'vue-router'
            ...
            const router = useRouter()
            
            const goHome = function () {
              router.push({ name: 'home'})
            }
            </script>
            ```
            
    - `router.replace()` : 현재 위치 바꾸기
        - push 메서드와 달리 history stack에 새로운 항목을 push 하지 않고 다른 URL로 이동 (=== 이동 전 URL로 뒤로 가기 불가)
        - 선언적 표현 : `<RouterLink : to=”….” replace>`
        - 프로그래밍적 표현: `router.replace()`
        - 예시 : UserView 컴포넌트에서 HomeView 컴포넌트로 이동하는 버튼만들기
            
            ```jsx
            const goHome = function () {
            	router.replace({ name: 'home'})
            }
            ```
            

## Navigation Guard

1. 설명 :
    - Vue router를 통해 특정 URL에 접근할 때 다른 URL로 redirect를 하거나 취소하여 내비게이션을 보호
    - 라우트 전환 전/후 자동으로 실행되는 Hook
2. 종류 :
    - Globally Guard (전역 가드) : 애플리케이션 전역에서 모든 라우트 전환에 적용되는 가드
    - Per-route Guard (라우터 가드) : 특정 라우트에만 적용되는 가드
    - In-component (컴포넌트 가드) : 컴포넌트 내에서만 적용되는 가드

### Globally Guard

1. 설명 :  애플리케이션 전역에서 모든 라우트 전환에 적용되는 가드
2. 작성위치 : index.js
3. 종류 :
    - `router.beforeEach()`
        - 설명 : 다른 URL로 이동하기 직전에 실행되는 함수
    - `router.beforeResolve()`
    - `router.afterEach()`
4. `router.beforeEach()` 구조 :
    
    ```jsx
    router.beforeEach (to, from) => {
    	...
    	return false 또는 return { name: 'About' }
    })
    ```
    
    - 모든 가드는 2개의 인자를 받음
        - to : 이동할 URL 정보가 담긴 Route 객체
        - from : 현재 URL 정보가 담긴 Route 객체
    - 선택적으로 다음 값 중 하나를 반환
        - false :
            - 현재 네비게이션을 취소
            - 브라우저의 URL이 변경된 경우(사용자가 수동으로 또는 뒤로가기 버튼을 통해) form 경로의 URL로 재설정
        - Route Location
            - router.push()를 호출하는 것처럼 경로 위치를 전달하여 다른 위치로 redirect
            - return이 없다면 자동으로 to URL Route 객체로 이동
5. 실습
    - 전역 가드 beforeEach 작성
    - HomeView에서 UserView로 이동했을 때 각 인자 값 출력 확인하기
        
        ```jsx
        // index.js
        
        router.beforeEach((to, from) => {
        	console.log(to) // 이동할 URL인 user 라우트에 대한 정보
        	console.log(from) // 현재 URL인 home 라우트 정보
        })
        ```
        
6. 활용 : 비로그인 사용자
    - LoginView 컴포넌트 작성 및 라우트 등록
        
        ```jsx
        // index.js
        
        import LoginView from '@/views/LoginView.vue'
        
        {
        	path: '/login',
        	name: 'login',
        	component: LoginView,
        }
        
        router.beforeEach((to, from) => {
          const isAuthenticated = false
        
          // 로그인이 되어있지 않고, 이동하고자 하는 페이지가 login이 아니라면
          if (!isAuthenticated && to.name !== 'login'){
            console.log('로그인이 필요합니다')
            return { name: 'login'}
          }
        })
        ```
        
        ```jsx
        // App.vue
        
        <RouterLink :to="{ name: 'login' }">Login</RouterLink>
        ```
        

### Per-route Guard

1. 설명 :
    - 특정 라우터에서만 동작하는 가드
    - 단순히 URL의 매개변수나 쿼리 값이 변경될 때는 실행되지 않고, 다른 URL에서 탐색해 올 때만 실행됨
2. 작성위치 : index.js의 각 routes 객체에서 정
3. 종류 :
    - `route.beforeEnter()` : 다른 URL로 이동하기 직전에 실행되는 함수
4. 구조 : 
    
    ```jsx
    {
    	path: '/user/id',
    	name: 'user',
    	component: UserView,
    	beforeEnter((to, from) => {
    		...
    		return false
    	}
    }
    ```
    
5. 활용 : 이미 로그인 한 상태라면 LoginView 진입을 막고 HomeView로 이동시키기
    
    ```jsx
    // index.js
    
    const isAuthenticated = true
    
    const router = createRouter({
    	routes: [
    	{
    		path: '/login',
    		name: 'login',
    		component: LoginView,
    		beforeEnter: (to, from) => {
    			if (isAuthenticated === true) {
    				console.log('이미 로그인 상태입니다.')
    				return { name: 'home'}
    			}
    		}
    	}
    ...
    ```
    

### In-component Guard

1. 설명 : 특정 컴포넌트 내에서만 동작하는 가드
2. 작성 위치 : 각 컴포넌트의 <script> 내부
3. 종류 :
    - `onBeforeRouteLeave()`
        - 현재 라우트에서 다른 라우트로 이동하기 전에 실행
        - 사용자가 현재 페이지를 떠나는 동작에 대한 로직을 처리
    - `onBeforeRouteUpdate()`
        - 이미 렌더링 된 컴포넌트가 같은 라우트 내에서 업데이트 되기 전에 실행
        - 라우트 업데이트 시 추가적으로 로직을 처리
4. `onBeforeRouteLeave()` 예시 : 사용자가 Userview를 떠날 시 팝업창 출력하기
    
    ```jsx
    <!-- UserView.vue -->
    import { onBeforeRouteLeave } from 'vue-router'
    
    onBeforerouteLeave((to, from) => {
    	const answer = window.confirm('정말 떠나실 건가요?)
    	if (answer === false) {
    		return false
    	}
    }
    ```
    
5. `onBeforeRouteUpdate()` 예시 : UserView 페이지에서 다른 id를 가진 User의 UserView 페이지로 이동하기
    
    ```jsx
    <!-- UserView.vue -->
    
    <button @click="routeUpdate">100번 유저 페이지</button>
    ```
    
    ```jsx
    <!-- UserView.vue -->
    
    import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'
    
    const routeUpdata = function () {
    	route.push({ name: 'user', params: { id : 100}})
    }
    
    onBeforeRouteUpdate((to, from) => {
    	userId.value = to.params.id
    })
    ```
    
    - 만약 onBeforeRouteUpdata에서 userId를 변경하지 않으면 userId는 갱신되지 않음 → 컴포넌트가 재사용 되었기 때문