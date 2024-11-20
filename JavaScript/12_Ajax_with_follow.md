# Ajax with follow

1. 과정 설명 :
    - 기존 :
        - HTML의 form 태그를 사용해 POST 메서드로 데이터를 제출
    - 변경 :
        - axios를 사용해 POST 메서드로 데이터를 제출
        - form의 method 속성, action 속성이 불필요
        - 팔로우 버튼에 submit 이벤트가 발생하면(이벤트 리스너)
        - Django가 json 데이터를 응답
        - JS에서 응답받은 json 데이터를 활용해 팔로우 버튼을 조작 (DOM)

## 실습

1. Ajax 적용
    - 프로필 페이지에 axios CDN 작성
2. form 태그에서 method, action 삭제 후 id 추가
    - 요청이 axios로 대체되기 때문
3. form 요소에 이벤트 핸들러 할당
    - 팔로우 버튼 선택
    - 이벤트 리스너 부착 → submit 이벤트 감지
    - submit 이벤트의 기본 동작 취소하기
        - 버튼이 눌린 것만 감지하면 됨
        - submit 이벤트의 기본 동작 : 제출 → 이 동작 취소
    
    ```jsx
    const formTag = document.querySelector('#follow-form')
    
    formTag.addEventListener('submit', function (event) {
    	event.preventDefault()
    })
    ```
    
4. axios 요청 코드 작성
    - 문제 1 : url 작성에 필요한 user pk는 어떻게 가져올 수 있을까
        - 해결 1 :  HTML에서 JS로 전달
            - currentTaget이어야 하는 이유 : currentTarget은 addEventListener가 붙어있는 객체를 가리킴
        
        ```jsx
        <form id="follow-form" data-user-id="{{ person.pk }}">
        ...
        </form>
        
        <script>
        const userId = event.currentTarget.dataset.userId
        // const userId = this.dataset.userId
        // const userId = formTag.dataset.userId
        
        axios({
        	method:'post',
        	// 6. HTML에서 전달해서 할당한 PK 값으로 url 완성
        	url: `/account/${userId}/follow/`
        })
        </script>
        ```
        
    - 문제 2 : csrftoken은 어떻게 보내야 할까?
        - 해결 :
            - 문서상 input hidden 타입으로 존재하는 csrf token 데이터를 이제는 axios로 전송해야 함
            - csrf 값을 가진 input 요소를 직접 선택 후 axios에 작성하기
        
        ```jsx
        // 7. csrf token 선택
        const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value
        axios({
          method: 'post',
          // 6. HTML에서 전달해서 할당한 PK 값으로 url 완성
          url: `/accounts/${userId}/follow/`,
          // 8. 선택한 csrf token 값을 요청 headers에 세팅
          headers: {'X-CSRFToken': csrftoken},
        })
        ```
        
5. 팔로우 버튼 토글
    - 토글하기 위해 필요한 것 : 현재 팔로웅 상태인지 언팔로우 상태인지에 대한 상태 확인이 필요
        - 해결 : Django의 view 함수에서 팔로우 여부를 파악할 수 있는 변수를 추가로 생성해 JSON 타입으로 응답하기
        
        ```python
        from django.http import JsonResponse
        
        def follow(request, user_pk):
            User = get_user_model()
            you = User.objects.get(pk=user_pk)
            me = request.user
        
            if me != you:
                # 9. JS에게 팔로우 상태여부를 전달할 데이터 작성
                if me in you.followers.all():
                    you.followers.remove(me)
                    # me.followings.remove(you)
                    is_followed = False
                else:
                    you.followers.add(me)
                    # me.followings.add(you)
                    is_followed = True
                context = {
                    'is_followed': is_followed,
                }
                # 10. JSON 데이터로 응답
                return JsonResponse(context)
            return redirect('accounts:profile', you.username)
        ```
        
    - 응답 데이터 is_followed에 따라 팔로우 버튼을 조작하기
        
        ```jsx
        axios({
          method: 'post',
          // 6. HTML에서 전달해서 할당한 PK 값으로 url 완성
          url: `/accounts/${userId}/follow/`,
          // 8. 선택한 csrf token 값을 요청 headers에 세팅
          headers: {'X-CSRFToken': csrftoken},
        })
          .then((response) => {
            console.log(response)
            // 11. django로부터 응답받은 팔로우 상태 정보
            console.log(response.data)
            // 12. 팔로우 상태 정보 데이터에 따라 팔로우 버튼을 조작
            const isFollowed = response.data.is_followed
            const followBtn = document.querySelector('.follow-input')
            if (isFollowed === true) {
              followBtn.value = '언팔로우'
            } else {
              followBtn.value = '팔로우'
            }
        })
        ```
        
6. 팔로잉 수와 팔로워 수 비동기 적용
    - 해당 요소를 선택할 수 있도록 span 태그와 id 속성 작성
    
    ```jsx
    <div>
      팔로잉 : <span id="followings-count">{{ person.followings.all|length }}</span>
      / 팔로워 : <span id="followers-count">{{ person.followers.all|length }}</span>
    </div>
    ```
    
    - 각 span 태그를 선택
    
    ```jsx
    // 13. 팔로워 & 팔로잉 수 선택
    const followingsCountTag = document.querySelector('#followings-count')
    const followersCountTag = document.querySelector('#followers-count')
    ```
    
    - Django view 함수에서 팔로워, 팔로잉 인원 수 연산을 진행하여 결과를 응답 데이터로 전달 (14번)
    
    ```python
    def follow(request, user_pk):
        User = get_user_model()
        you = User.objects.get(pk=user_pk)
        me = request.user
    
        if me != you:
            # 9. JS에게 팔로우 상태여부를 전달할 데이터 작성
            if me in you.followers.all():
                you.followers.remove(me)
                # me.followings.remove(you)
                is_followed = False
            else:
                you.followers.add(me)
                # me.followings.add(you)
                is_followed = True
            context = {
                'is_followed': is_followed,
                # 14. 팔로워 수와 팔로잉 수에 대한 데이터 작성
                'followings_count': you.followings.count(),
                'followers_count': you.followers.count(),
            }
            # 10. JSON 데이터로 응답
            return JsonResponse(context)
        return redirect('accounts:profile', you.username)
    ```
    
    - JS에서 응답 데이터를 받아 각 태그의 인원수 값 변경에 활용
    
    ```jsx
    .then((response) => {
      console.log(response)
      // 11. django로부터 응답받은 팔로우 상태 정보
      console.log(response.data)
      // 12. 팔로우 상태 정보 데이터에 따라 팔로우 버튼을 조작
      const isFollowed = response.data.is_followed
      const followBtn = document.querySelector('.follow-input')
      if (isFollowed === true) {
        followBtn.value = '언팔로우'
      } else {
        followBtn.value = '팔로우'
      }
      // 13. 팔로워 & 팔로잉 수 선택
      const followingsCountTag = document.querySelector('#followings-count')
      const followersCountTag = document.querySelector('#followers-count')
      // 15 . django가 응답한 팔로잉, 팔로워 수 데이터를 활용해 DOM 변경
      followingsCountTag.textContent = response.data.followings_count
      followersCountTag.textContent = response.data.followers_count
    })
    ```
    

### `data-*` 속성

1. 설명 : 사용자 지정 데이터 특성을 만들어 임의의 데이터를 HTML과 DOM 사이에서 교환 할 수 있는 방법
2. 사용 예시
    
    ```jsx
    <div data-my-id="my-data></div>
    
    <script>
    	const myId = event.target.dataset.myId
    </script>
    ```
    
    - 모든 사용자 지정 데이터는 JS에서 dataset 속성을 통해 접근
    - 주의사항
        - 대소문자 여부에 상관없이 ‘xml’ 문자로 시작 불가
        - 세미콜론 포함 불가
        - 대문자 포함불가