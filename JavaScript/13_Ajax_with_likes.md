# Ajax with likes

1. Ajax 좋아요 적용 시 유의사항
    - 전반적인 Ajax 적용은 팔로우 구현 과정과 모두 동일
    - 단, 팔로우와 달리 좋아요 버튼은 한 페이지에 여러 개가 존재
        - 그렇다면 모든 좋아요 버튼에 이벤트 리스너를 할당해야 할까? NO! 버블링을 이용하면 됨
2. [복습] 버블링
    - 한 요소에 이벤트가 발생하면, 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작하는 현상
    - 가상 최상단의 조상 요소(document)를 만날 때까지 이 과정이 반복되면서 요소 각각에 할당된 핸들러가 동작
    - 버블링이 필요한 이유
        - 만약 각자 다른 동작을 수행하는 버튼이 여러 개가 있다고 가정
        - 그렇다면 각 버튼마다 이벤트 핸들러를 달아야 할까?
        - NO, 각 버튼의 공통 조상 요소에 이벤트 핸들러 하나만 할당하기
            - 이벤트 감지는 해결
            - 그런데 어떤 게 이벤트를 발생시켰는자 확인하는 건?
                - target으로 확인
3. 버블링을 활용하지 않은 경우
    - querySelectorAll()을 사용해 전체 좋아요 버튼을 선택
    - forEach()를 사용해 전체 좋아요 버튼을 순회하면서 진행
    - 실습
        - querySelectorAll() 선택을 위한 class 적용
            
            ```jsx
            {% for article in articles %}
            	...
            	<form class="like-forms" data-article-id="{{ article.pk }}">
            		{% csrf_token %}
            		{% if request.user in article.like_users.all %}
            			<input type="submit" value="좋아요 취소" id="like-{{ article.pk }}">
            		{% else %}
            			<input type="submit" value="좋아요" id="like-{{ article.pk }}">
            		{% endif %}
            	</form>
            {% endfor %}
            ```
            
        - forEach()를 사용해 전체 좋아요 버튼을 순회하면서 진행
            
            ```jsx
            formTags.forEach((formTag) => {
            	formTag.addEventListener('submit', function (event) {
            		event.preventDefault()
            		
            		const articleId = formTag.dataset.articleId
            		
            		axios({
            			method: 'post',
            			url: `/articles/${articleId}/likes/`,
            			headers: {'X-CSRFToken': csrftoken},
            		})
            			.then((response) => {
            				const isLiked = reponse.data.is_liked
            				const likeBtn = document.querySelector(`#like-${articleId}`)
            				if (isLiked === true) {
            					likeBtn.value = '좋아요 취소'
            				} else {
            					likeBtn.value = '좋아요'
            				}
            		})
            	}
            })
            ```
            

## 실습

1. 모든 좋아요 form 요소를 포함하는 최상위 요소 작성
    
    ```jsx
    <article class="article-container">
      {% for article in articles %}
        ...
      {% endfor %}
    </article>
    ```
    
2. 최상위 요소 선택
3. 이벤트 핸들러 할당
4. 하위 요소들의 submit 이벤트를 감지하고 submit 기본 이벤트를 취소
    
    ```jsx
    // 2. 게시글을 모두 포함하는 최상위 요소를 선택
    const articleContainer = document.querySelector('.article-container')
    
    // 3. 선택한 최상위 요소에 이벤트 핸들러 부착
    articleContainer.addEventListener('submit', function (event) {
      // 4. submit 이벤트 기본 동작 취소
      event.preventDefault()
      ...
    })
    ```
    
5. axios 코드 작성
    - 각 좋아요 form에 article.pk를 부여
    
    ```html
    {% comment %} 9. JS에 게시글 id를 전달하기 위한 data 속성 세팅 {% endcomment %}
    <form data-article-id="{{ article.pk }}">
      ...
    </form>
    ```
    
    - HTML의 article.pk 값을 JavaScrpit에서 참조하기
        - [복습] currentTarget & target
        - currentTarget 속성
            - 현재 요소
            - 항상 이벤트 핸들러가 연결된 요소만을 참조하는 속성
            - this와 같음
        - target 속성
            - 이벤트가 발생한 가장 안쪽의 요소(target)를 참조하는 속성
            - 실제 이벤트가 시작된 요소
            - 버블링이 진행되어도 변하지 않음
    
    ```jsx
    // 10. HTML에서 전달한 게시글 id 받기
    const articleId = event.target.dataset.articleId
    ```
    
    - url 작성에 필요한 article pk 받기
    
    ```jsx
    axios({
      method:'post',
      url: `/articles/${articleId}/likes/`,
      // 8. 선택한 csrf token 값을 요청 headers에 세팅
      headers: {'X-CSRFToken': csrftoken},
    })
    ```
    
6. 좋아요 버튼 토글
    - 토글하기 위해 필요한 것 : 현재 사용자가 좋아요를 누른 상태인지 좋아요를 누르지 않은 상태인지에 대한 상태 확인
        - 해결 :
            - Django의 view 함수에서 좋아요 여부를 파악할 수 있는 변수 추가 생성
            - JSON 타입으로 응답하기
        
        ```python
        def likes(request, article_pk):
            article = Article.objects.get(pk=article_pk)
        
            # 12. 좋아요 상태여부를 JS에 응답할 데이터 세팅
            if request.user in article.like_users.all():
                article.like_users.remove(request.user)
                is_liked = False
            else:
                article.like_users.add(request.user)
                is_liked = True
            # 13. 세팅한 데이터를 JSON 형식으로 응답
            context = {
                'is_liked': is_liked,
            }
            return JsonResponse(context)
        ```
        
    - 응답 데이터 is_like를 받아 isLiked 변수에 할당
        
        ```jsx
        .then((response) => {
          console.log(response)
          // 14. django한테 응답받은 좋아요 상태 정보 저장
          const isLiked = response.data.is_liked
        })
        ```
        
    - 어떤 게시글의 좋아요 버튼을 선택 했는지 구분이 필요
        - 문자와 article의 pk 값을 혼합하여 각 버튼에 id 속성 값을 설정
        
        ```jsx
        <form data-article-id="{{ article.pk }}">
          {% csrf_token %}
          {% if request.user in article.like_users.all %}
          {% comment %} 17. 각 좋아요 버튼을 구별할 수 있는 id 속성 추가 {% endcomment %}
            <input type="submit" value="좋아요 취소" id="like-{{ article.pk }}">
          {% else %}
            <input type="submit" value="좋아요" id="like-{{ article.pk }}">
          {% endif %}
        </form>
        ```
        
    - 각 좋아요 버튼을 선택 후 isLiked에 따라 버튼을 토글
        
        ```jsx
        .then((response) => {
          console.log(response)
          // 14. django한테 응답받은 좋아요 상태 정보 저장
          const isLiked = response.data.is_liked
          //16. 좋아요 버튼 선택
          const likeBtn = document.querySelector(`#like-${articleId}`)
          // 15. 좋아요 상태 정보에 따라 버튼 변경
          if (isLiked === true) {
            likeBtn.value = '좋아요 취소'
          } else {
            likeBtn.value = '좋아요'
          }
        })
        ```