# 인증 with DRF

1. 인증과 권한
    - Authentication
        - 수신된 요청을 해당 요청의 사용자 또는 자격 증명과 연결하는 메커니즘
            - 누구인지를 확인하는 과정
    - Permissions
        - 요청에 대한 접근 허용 또는 거부 여부를 결정
    - 설명 :
        - 순서상 인증이 먼저 진행
            - 수신 요청을 해당 요청의 사용자 or 해당 요청이 서명된 토큰과 같은 자격 증명 자료와 연결
        - 권한 및 제한 정책 → 인증이 완료된 해당 자격 증명을 사용하여 요청을 허용해야 하는 지를 결정
2. DRF에서의 인증
    - 인증 실행 시기 : view 함수 시작, 권한 및 제한 확인이 발생하기 전, 다른 코드의 진행이 허용되지 전에 실행됨
    - 인증 자체로는 들어오는 요청을 허용하거나 거부할 수 없으며, 단순히 요청에 사용된 자격 증명만 식별한다는 점에 유의
3. 승인되지 않은 응답 및 금지된 응답
    - 인증되지 않은 요청의 권한을 거부하는 경우 해당되는 두가지 오류 코드를 응답
    - HTTP 401 Unauthorized
        - 요청된 리소스에 대한 유효한 인증 자격 증명이 없기 때문에 클라이언트 요청이 완료되지 않았음을 나타냄 (누구인지를 증명할 자료가 없는 상황)
    - HTTP 403 Forbidden (Permission Denied)
        - 서버에 요청이 전달되었지만, 권한 때문에 거절되었다는 것을 의미
        - 401과 다른 점 : 서버가 클라이언트가 누구인지 알고 있음
4. 인증 정책 설정 방법
    - 전역 설정
        - 프로젝트 전체에 적용되는 기본 인증 방식을 정의
        - DEFAULT_AUTHENTICATION_CLASSES를 사용
        - 기본 값 : SessionAuthentication과 BasicAuthentication
        - 사용 예시 :
            
            ```python
            REST_FRAMEWORK = {
                # Authentication
                'DEFAULT_AUTHENTICATION_CLASSES': [
                    'rest_framework.authentication.BasicAuthentication',
                    'rest_framework.authentication.SessionAuthentication',
                ],
            }
            ```
            
    - View 함수 별 설정
        - authentication_classes 데코레이터를 사용
        - 개별 view에 지정하여 재정의
        - 사용 예시 :
            
            ```python
            from rest_framework.decorators import authentication_classes
            from rest_framework.authentication impor TokenAuthentication, BasicAuthentication
            
            @api_view(['GET', 'POST'])
            @authentication_classed([TokenAuthentication, BasicAutentication])
            def article_list(requst):
            	pass
            ```
            
5. DRF가 제공하는 인증 체계
    - BasicAuthentication
    - TokenAuthentication
        - token 기반 HTTP 인증 체계
        - 기본 데스크톱 및 모바일 클라이언트와 같은 클라이언트-서버 설정에 적합
        - 서버가 인증된 사용자에게 토큰을 발급하고 사용자는 매 요청마다 발급받은 토큰을 요청과 함께 보내 인증 과정을 거침
    - SessionAuthentication
    - RemoteUserAuthentication

## Token 인증 설정

1. 과정
    - 인증 클래스 설정
        - 전역 인증 정책을 Token 방식으로 설정
            
            ```python
            REST_FRAMEWORK = {
                # Authentication
                'DEFAULT_AUTHENTICATION_CLASSES': [
                    'rest_framework.authentication.TokenAuthentication',
                ],
            }
            ```
            
    - INSTALLED_APPS 추가
        
        ```python
        INSTALLED_APPS = [
        	...
        	'rest_framework.authtoken',
        	...
        ]
        ```
        
    - Migrate 진행
    - 토큰 생성 코드 작성
        - signals  : 이벤트 알림 시스템
            - 조건이 만족되면 자동으로 실행되는 파일
            - 인증된 사용자에게 자동으로 토큰을 생성해주는 역할
2. 토큰 인증 방식 과정
    - 브라우저 → 사용자 로그인 → 장고
    - 장고 → 사용자 확인 → DB
    - 장고 : 토큰 발급
    - 브라우저 ← 응답 (+ Token) ← 장고
    - 브라우저 → 데이터 요청 (+ Token) → 장고
    - 브라우저 → 응답 (+ 요청데이터) ← 장고

## Dj-Rest-Auth 라이브러리

1. 설명 :
    - 회원가입, 인증, 비밀번호 재설정, 사용자 세부 정보 검색, 회원 정보 수정 등 다양한 인증 관련 기능을 제공하는 라이브러리
2. 설치 및 적용
    - 설치
        
        ```jsx
        pip install dj-rest-auth
        ```
        
    - INSTALLED_APPS에 추가
    - urls.py에 추가
        
        ```python
        path('accounts/', include('dj_rest_auth.urls'))
        ```
        
    - Registration(등록) 기능 추가
        - 패키지 추가 설치
            
            ```python
            pip install 'dj-rest-auth[with-social]'
            ```
            
        - 추가 App 등록
            - https://dj-rest-auth.readthedocs.io/en/latest/installation.html#registration-optional
        - 관련 설정 코드 주석 해제
            
            ```python
            MIDDLEWARE = [
            	'allauth.account.middleware.AccountMiddleware',
            ]
            ```
            
        - 추가 URL 등록
        - Migrate

## Token 발급 및 활용

1. 발급
    - Dj-Rest-Auth 라이브러리 설치로 인해 생긴 url로 회원가입
        - token 발급 됨
    - Dj-Rest-Auth 라이브러리 설치로 인해 생긴 url로 회원가입
        - token 발급 됨
    - 이제 Vue에서 발급된 token 정보를 저장해야 함
2. 활용
    - Postman을 활용해 게시글 작성
    - header에 Token 정보 포함
        - Key : Authorization
        - Value : Token {토큰 값}
    - body에 title, content 정보 포함

# 권한 with DRF

1. 권한 설정 방법
    - 전역 설정
        - 프로젝트 전체에 적용되는 기본 권한 방식을 정의
        - DEFAULT_PERMISSION_CLASSES를 사용
        - 기본 값 : rest_framework.permissions.AllowAny
        - 사용 예시
            
            ```jsx
            REST_FRAMEWORK = {
                # permission
                'DEFAULT_PERMISSION_CLASSES': [
                    'rest_framework.permissions.AllowAny',
                ],
            }
            ```
            
    - View 함수 별 설정
        - permission_classes 데코레이터를 사용
        - 개별 view에 지정하여 재정의
        - 사용 예시
            
            ```python
            # permission Decorators
            from rest_framework.decorators import permission_classes
            from rest_framework.permissions import IsAuthenticated
            
            @api_view(['GET', 'POST'])
            @permission_classes([IsAuthenticated])
            def article_list(request):
            	pass
            ```
            
2. DRF가 제공하는 권한 정책
    - IsAuthenticated
        - 인증되지 않은 사용자 → 권한 거부
        - 인증된 사용자 → 권한 허용
        - 등록된 사용자만 API에 액세스 할 수 있도록 하려는 경우에 적합
    - IsAdminUser
    - IsAuthenticatedOrReadOnly
    - 등등
3. IsAuthenticated 권한 설정
    - 개요
        - 기본적으로 모든 View 함수에 대한 접근을 허용
        - 전체 게시글 조회 및 생성 시에만 인증된 사용자만 진행 할 수 있도록 권한 설정
    - 전역 설정
        - allowany로 허용
    - views.py → IsAuthenticated 데코레이터 추가

# 인증 with Vue

## 회원가입

1. stores의 signUp 함수가 해야 할 일
    - 사용자 입력 데이터를 받기
    - 서버로 회원가입 요청을 보냄
    - 구조
        
        ```jsx
        // stores/counter.js
        
        export const useCounterStore = defineStore('counter, () => {
        	const signUp = function () {
        		...
        	}
        	return { ... signUp }
        }
        ```
        
2. 컴포넌트
    - 컴포넌트에서 사용자 입력 데이터를 저장 후 store의 signUp 함수를 호출하는 함수 작성
        
        ```jsx
        // views/SignUpView.vue
        import { useCounterStore } from '@/stores/counter'
        
        const store = userCounterStore()
        
        const signUp = function () {
        	const payload = {
        		username: username.value,
        		password1: password1.value,
        		password2: password2.value
        	}
        	store.signUp(payload)
        }
        ```
        
3. 실제 회원가입 요청을 보내는 store의 signUp 함수 작성
    
    ```jsx
    // stores/counter.js
    
    const signUp = function (payload) {
    	const { username, password1, password2 } = payload
    	
    	axios({
    		method: 'post',
    		url: `${API_URL}/accounts/signup/`,
    		data: {
    			username, password1, password2
    		}
    	})
    		.then(res => {
    			console.log('회원가입이 완료되었습니다.')
    		})
    		.catch(err => console.log(err))
    }
    ```
    

## 로그인

1. 로그인 요청을 보내기 위한 logIn 함수가 해야 할 일
    - 사용자 입력 데이터를 받기
    - 서버로 로그인 요청 및 응답 받은 토큰 저장
    - 구조
        
        ```jsx
        // stores/counter.js
        
        export const useCounterStore = defineStore('counter, () => {
        	const logIn= function () {
        		...
        	}
        	return { ... logIn }
        }
        ```
        
2. 컴포넌트
    - 컴포넌트에 사용자 입력 데이터를 저장 후 store의 logIn 함수를 호출하는 함수 작성
    
    ```jsx
    // views/LoginView.vue
    
    import { useCounterStore } from '@/stores/counter'
    
    const store = useCounterStore()
    
    const logIn = function () {
    	const payload = {
    		username: username.value,
    		password: password.value
    	}
    	store.logIn(payload)
    }
    ```
    
3. 실제 로그인 요청을 보내는 store의 logIn 함수 작성
    
    ```jsx
    // stores/counter.js
    
    const logIn= function () {
    	const { username, password } = payload
    	axios({
    		method: 'post',
    		url: `${API_URL}/accounts/login`,
    		data: {
    			username, password
    		}
    	})
    		.then(res => {
    			console.log('로그인이 완료되었습니다.')
    		})
    		.catch(err => console.log(err))
    }
    ```
    
    - post인 이유
        - 서버의 데이터를 상태를 변경하는 것이기 때문

## 토큰 저장

1. 토큰 저장 로직 구현 : 로그인 시 토큰 저장
    
    ```jsx
    // stores/counter.js
    
    export const useCounterStore = defineStore('counter, () => {
    	
    	const token = ref(null)
    	
    	const logIn= function () {
    		const { username, password } = payload
    		axios({
    			method: 'post',
    			url: `${API_URL}/accounts/login`,
    			data: {
    				username, password
    			}
    		})
    			.then(res => {
    				token.value = res.data.key
    			})
    			.catch(err => console.log(err))
    	}
    	return { ... logIn, token }
    }
    ```
    
2. 토큰이 필요한 요청
    - 게시글 전체 목록 조회 시
        - 게시글 전체 목록 조회 요청 함수 getArticles에 token 추가
            
            ```jsx
            // DRF로 전체 게시글 요청을 보내고 응답을 받아 articles에 저장하는 함수
            const getArticles = function () {
              axios({
                method: 'get',
                url: `${API_URL}/api/v1/articles/`,
                headers: {
                  Authorization: `Token ${token.value}`
                }
              })
                .then((res) => {
                  articles.value = res.data
                })
                .catch((err) => {
                  console.log(err)
                })
            }
            ```
            
    - 게시글 작성 시
        - 게시글 생성 요청 함수 createArticle에 token 추가
            
            ```jsx
            // DRF로 게시글 생성 요청을 보내는 함수
            const createArticle = function () {
              axios({
                method: 'post',
                url: `${store.API_URL}/api/v1/articles/`,
                data: {
                  title: title.value,
                  content: content.value
                },
                header: {
                  Authorization: `Token ${store.token}`
                }
              })
                .then((res) => {
                  // console.log('게시글 작성 성공!')
                  router.push({ name: 'ArticleView' })
                })
                .catch((err) => {
                  console.log(err)
                })
            }
            ```
            

## 인증 여부 확인

1. 사용자의 인증(로그인) 여부에 따른 추가 기능 구현
    - 인증 되지 않은 사용자 : 메인 페이지 접근 제한
    - 인증 된 사용자 : 회원가입 및 로그인 페이지에 접근 제한
2. 인증 상태 여부를 나타낼 속성 값 지정
    - token 소유 여부에 따라 로그인 상태를 나타낼 isLogin 변수를 작성
        - computed를 활용 → token 값이 변할 때만 상태를 계산하도록 함
        
        ```jsx
        // stores/counter.js
        
        const isLogin = computed(() => {
          if (token.value === null) {
            return false
          } else {
            return true
          }
        })
        ```
        

### 인증 되지 않은 사용자 : 메인 페이지 접근 제한

1. 방법 :
    - 전역 네비게이션 가드 beforeEach를 활용
    - 다른 주소에서 메인 페이지로 이동시, 인증 되지 않는 사용자라면 로그인 페이지로 이동시키기
2. 코드 :
    
    ```jsx
    // router/index.js
    
    router.beforeEach((to, from) => {
      const store = useCounterStore()
      if (to.name === 'ArticleView' && !store.isLogin) {
        window.alert('로그인이 필요합니다.')
        return { name: 'LogInView'}
      }
    })
    ```
    

### 인증된 사용자 : 회원가입과 로그인 페이지에 접근 제한

1. 방법 :
    - 다른 주소에서 회원가입 또는 로그인 페이지로 이동 시, 이미 인증 된 사용자라면 메인 페이지로 이동시키기
2. 코드 :
    
    ```jsx
    // router/index.js
    
    router.beforeEach((to, from) => {
      const store = useCounterStore()
      if (to.name === 'ArticleView' && !store.isLogin) {
        window.alert('로그인이 필요합니다.')
        return { name: 'LogInView'}
      }
      if ((to.name === 'SignUpView' || to.name === 'LogInView') && (store.isLogin)) {
        window.alert('이미 로그인이 되어있습니다.')
        return { name: 'ArticleView'}
      }
    })
    ```