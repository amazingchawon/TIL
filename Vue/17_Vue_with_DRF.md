# Vue with DRF 1

## 프로젝트 개요

1. Vue with DRF 1 : Vue와 DRF 간 기본적인 요청과 응답
2. Vue with DRF 2 : Vue와 DRF에서의 인증 시스템

## 메인 페이지 구현

### 게시글 목록 출력

1. 개요
    - ArticleView 컴포넌트에 ArticleList 컴포넌트와 ArticleListItem 컴포넌트 등록 및 출력하기
    - ArticleList와 ArticleListItem은 각각 게시글 출력을 담당
2. ArticleView의 route 관련 코드 작성
3. App 컴포넌트에서 ArticleView 컴포넌트로 이동하는 RouterLink 작성
4. ArticleView 컴포넌트에 ArticleList 컴포넌트 등록
5. store에 임시 데이터 articles 배열 작성하기
    - 추후 변경
6. ArticleList 컴포넌트에서 게시글 목록 출력
    - store의 articles 데잍 참조
    - v-for을 활용하여 하위 컴포넌트에서 사용할 article 단일 객체 정보를 props로 전달
7. ArticleListItem 컴포넌트는 내려 받은 props를 정의 후 출력

### DRF와의 요청과 응답

1. 개요 :
    - [게시글 목록 출력]에서 임시 데이터 만든 것을 DRF 서버에 요청해서 데이터 응답 받는 것으로 변경
2. DRF 서버로 AJAX 요청을 위한 azios 설치 및 관련 코드 작성
    
    ```bash
    npm install axios
    ```
    
    ```jsx
    // store/counter.js
    
    import axios from 'axios'
    
    export const useCounterStore = defineStore('counter', () => {
      const articles = ref([])
      const API_URL = 'http://127.0.0.1:8000'
    }, { persist: true })
    ```
    
3. DRF 서버로 요청을 보내고 응답 데이터를 처리하는 getArticles 함수 작성
    
    ```jsx
    // store/counter.js
    
    import axios from 'axios'
    
    export const useCounterStore = defineStore('counter', () => {
      const articles = ref([])
      const API_URL = 'http://127.0.0.1:8000'
    
      // DRF로 전체 게시글 요청을 보내고 응답을 받아 articles에 저장하는 함수
      const getArticles = function () {
        axios({
          method: 'get',
          url: `${API_URL}/api/v1/articles/`
        })
          .then((response) => {
            articles.value = response.data
          })
          .catch((error) => {
            console.log(error);
          })
      }
    
      return { articles, API_URL, getArticles }
    }, { persist: true })
    ```
    
4. ArticleView 컴포넌트가 마운트 될 때 getArticles 함수가 실행되도록 함
    
    ```jsx
    <script setup>
    // views/AritlceView.vue
    import ArticleList from '@/components/ArticleList.vue'
    import { onMounted } from 'vue'
    import { useCounterStore } from '@/stores/counter'
    import { RouterLink } from 'vue-router'
    
    const store = useCounterStore()
    onMounted(() => {
      // mount 되기 전에 store에 있는 전체 게시글 요청 함수를 호출
      // 이때 해야지 화면에 그리기 전에 데이터를 먼저 채울 수 있음
      store.getArticles()
    })
    </script>
    ```
    
5. DRF 서버 측에서는 문제없이 응답 (200OK) 그러나 브라우저 측에서 거절
    - 거절한 이유 :  XMLHttpReqest에 대한 접근이 CORS policy에 의해 차단되었음

### CORS Policy

1. SOP Same-origin policy :
    - 동일 출처 정책
    - 어떤 출처에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호 작용하는 것을 제한하는 보안 방식
    - 다른 곳에서 가져온 자료는 일단 막음
2. Origin (출처)
    - URL의 Protocol, Host, Port를 모두 포함하여 출처라고 부름
    - Same Origin 예시
        - 아래 세 영역이 일치하는 경우에만 동일 출처로 인정
        - Scheme/Protocol : http
        - Host : localhost
        - Port : 3000
3. CORS policy의 등장
    - 기본적으로 웹 브라우저는 같은 출처에서만 요청하는 것을 허용
    - 다른 출처의 요청은 보안상의 이유로 차단 → SOP에 의해서
    - 하지만 현대 웹 애플리케이션 → 다양한 출처로부터 리소스를 요청하는 경우가 많음
    - 웹 서버가 리소스에 대한 서로 다른 출처 간 접근을 허용하도록 선택할 수 있는 기능 제공
4. CORS
    - 교차 출처 리소스 공유
    - 특정 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
5. CORS Policy
    - 교차 출처 리소스 공유 정책
    - 다른 출처에서 온 리소스를 공유하는 것에 대한 정책
    - 서버에서 설정됨
    - 브라우저가 해당 정책을 확인하여 요청이 허용되는지 여부를 결정
    - 다른 출처의 리소스를 불러오려면 그 다른 출처에서 올바른 CORS header를 포함한 응답을 반환해야 함
6. CORS Headers 설정
    - Django → django-cors-headers 라이브러리 활용
        
        ```jsx
        pip install django-cors-headers
        ```
        
    - settings.py
        
        ```python
        INSTALLED_APPS = [
        	...
        	'corsheaders',
        	...
        ]
        
        MiDDELWARE = [
        	...
        	'corsheaders.middleware.CorsMiddleware',
        	...
        ]
        
        CORS_ALLOWED_ORIGINS = [
        	# CORS를 허용할 Vue 프로젝트의 Domain 등록
        	'http://localhost:5173'
        ]
        ```
        

### Article CR 구현

1. 단일 게시글 데이터 출력
    - DetailVue 관련 route 작성
    - ArticleListItem에 DetailView 컴포넌트로 가기 위한 RouterLink 추가
    - DetailView가 마운트 될 때 특정 게시글을 조회하는 AJAX 요청 진행
    - 응답 데이터 저장 후 출력
2. 게시글 작성
    - CreateView 관련 route 작성
    - ArticleView에서 CreateView 컴포넌트로 가기 위한 RouterLink 추가
    - v-model을 사용해 사용자 입력 데이터를 양방향 바인딩
        - v-model의 trim 수식어를 사용해 사용자 입력 데이터의 공백을 제거
    - 게시글 생성 요청을 담당하는 createArticle 함수 작성
        - 게시글 생성 요청을 담당하는 createArticle 함수 작성
        - 게시글 생성이 성공한다면 ArticleView 컴포넌트로 이동
        - submit 이벤트가 발생하면 createArticle 함수 호출
        - v-on prevent 수식어를 사용해 submit 이벤트의 기본 동작 취소
        
        ```jsx
        <template>
          <div>
            <h1>게시글 작성</h1>
            <form @submit.prevent="createArticle">
              <!-- csrf 토큰 보내지 않아도 됨 -->
              <div>
                <label for="title">제목 : </label>
                <input type="text" id="title" v-model="title">
              </div>
              <div>
                <label for="content">내용 : </label>
                <textarea type="text" id="content" v-model="content"></textarea>
              </div>
              <button>제출</button>
            </form>
          </div>
        </template>
        
        <script setup>
        import { ref } from 'vue'
        import axios from 'axios'
        import { useCounterStore } from '@/stores/counter'
        import { useRouter } from 'vue-router'
        
        const title = ref(null)
        const content = ref(null)
        const store = useCounterStore()
        const router = useRouter()
        
        // DRF로 게시글 생성 요청을 보내는 함수
        const createArticle = function () {
          axios({
            method: 'post',
            url: `${store.API_URL}/api/v1/articles/`,
            data: {
              title: title.value,
              content: content.value
            }
          })
          .then((response) => {
            console.log('게시글 작성 성공')
            router.push({ name: 'ArticleView'})
          })
          .catch((error) => {
            console.log(error);
          })
        }
        </script>
        
        ```