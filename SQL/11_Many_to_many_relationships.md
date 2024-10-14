# Many to Many relationships

1. 정의 :
    - 한 테이블의 0개 이상의 레코드가 다른 테이블의 0개 이상의 레코드와 관련된 경우
    - 양쪽 모두에서 N:1 관계를 가짐
2. M:N 관계의 역할과 필요성 이해하기
    - 병원 진료 시스템 모델 관계를 만들며 M:N 관계의 역할과 필요성 이해하기
    - 환자와 의사 2개의 모델을 사용하여 모델 구조를 구상하기
    - 제공된 99-mtm-practice 프로젝트를 기반으로 진행
3. N:1의 한계 상황
    - 의사와 환자 간 모델 관계 설정 :
        - 한 명의 의사에게 여러 환자가 예약할 수 있도록 설계
            
            ```python
            # hospitals/models.py
            
            class Doctor(models.Model):
                name = models.TextField()
            
                def __str__(self):
                    return f'{self.pk}번 의사 {self.name}'
            
            class Patient(models.Model):
                doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
                name = models.TextField()
            
                def __str__(self):
                    return f'{self.pk}번 환자 {self.name}'
            ```
            
    - 2명의 의사와 환자 생성, 환자는 서로 다른 의사에게 예약
        
        ```python
        doctor1 = Doctor.objects.create(name='allie')
        doctor2 = Doctor.objects.create(name='barbie')
        patient1 = Patient.objects.create(name='carol', doctor=doctor1)
        patient2 = Patient.objects.create(name='duke', doctor=doctor2)
        ```
        
        - DB : hospitals_doctor
            
            
            | id | name |
            | --- | --- |
            | 1 | allie |
            | 2 | barbie |
        - DB : hospitals_patient
            
            
            | id | name | doctor_id |
            | --- | --- | --- |
            | 1 | carol | 1 |
            | 2 | duke | 2 |
    - 1번 환자(carol)가 두 의사 모두에게 진료를 받고자 한다면, 환자 테이블에 1번 환자 데이터가 중복으로 입력될 수 밖에 없음
        - 동일한 환자지만 다른 의사에게도 진료받기 위해서는 객체를 하나 더 만들어 진행해야 함
        - `patient1 = Patient.objects.create(name='carol', doctor=doctor1, doctor2)` : 외래 키 컬럼에는 ‘1, 2’ 형태로 저장하는 것은 DB 타입 문제로 불가능
    - ⇒ 예약 테이블을 따로 만들자
4. 중개 모델 작성
    - 환자 모델의 외래 키를 삭제하고 별도의 예약 모델을 새로 생성
        
        ```python
        # hospitals/models.py
        
        class Doctor(models.Model):
            name = models.TextField()
        
            def __str__(self):
                return f'{self.pk}번 의사 {self.name}'
        
        # 외래키 삭제
        class Patient(models.Model):
            name = models.TextField()
        
            def __str__(self):
                return f'{self.pk}번 환자 {self.name}'
        
        # 중개모델 작성
        class Reservation(models.Model):
            doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
            patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
        
            def __str__(self):
                return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
        ```
        
    - 예약 모델은 의사와 환자에 각각 N:1 관계를 가짐
        - hospitals_reservation
            
            
            | id | doctor_id | patient_id |
            | --- | --- | --- |
            | . | . | . |

### 실습 : M:N 관계 만들기

1. 환자 모델에 ManyToManyField 작성
    - 의사 모델에 작성해도 상관 없으며 참조/역참조 관계만 잘 기억할 것
    - 환자 → 의사 : 참조, 의사 → 환자 : 역참조
    
    ```python
    # hospitals/models.py
    
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        # ManyToManyField 작성
        doctors = models.ManyToManyField(Doctor)
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    ```
    
2. 생성된 중개 테이블 `hospital_patient_doctors` 확인
    - `hospital_patient` 와 `hospital_doctor` 에는 변화 X
    - doctors 복수형으로 한 이유 : 다대다와 일대다를 구분하기 위해서
3. 의사, 환자 생성 및 예약 생성
    
    ```python
    doctor1 = Doctor.objects.create(name='allie')
    patient1 = Patient.objects.create(name='carol')
    patient2 = Patient.objects.create(name='duke')
    
    # 예약 생성 : 환자가
    patient1.doctors.add(doctor1)
    # 참조
    patient1.doctors.all()
    => <QuerySet [<Doctor: 1번 의사 allie>]>
    # 역참조
    doctor1.patient_set.all()
    => doctor1.patient_set.all()
    
    # 예약 생성 : 의사가
    # 생성법 다른 것 참고(역참조)
    doctor1.patient_set.add(patient2)
    doctor1.patient_set.all()
    patient2.doctors.all()
    patient1.doctors.all()
    ```
    
4. 중개테이블에서 현황 확인
    
    
    | id | patient_id | doctor_id |
    | --- | --- | --- |
    | 1 | 1 | 1 |
    | 2 | 2 | 1 |
5. 예약 취소
    - `remove()`
    
    ```python
    # 예약 취소 : 의사가
    doctor1.patient_set.remove(patient1)
    doctor1.patient_set.all()
    patient1.doctors.all()
    
    # 예약 취소 : 환자가
    patient2.doctors.remove(doctor1)
    patient2.doctors.all()
    doctor1.patient_set.all()
    ```
    
6. 예약 정보에 병의 증상, 예약일 등 추가 정보가 포함되어야 한다면
    - 중개 테이블에 추가 테이블이 필요 → `through` argument

### 실습 : 중개 테이블에 추가 데이터 사용

1. Reservation Class 재작성 및 through 설정
    - 예약 정보에 ‘증상’과 ‘예약일’이라는 추가 데이터가 생김
    
    ```python
    from django.db import models
    
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        doctors = models.ManyToManyField(Doctor, through='Reservation')
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    
    class Reservation(models.Model):
        doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
        patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
        symptom = models.TextField()
        reserved_at = models.DateTimeField(auto_now_add=True)
    
        def __str__(self):
            return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
    ```
    
2. 예약 생성
    
    ```python
    doctor1 = Doctor.objects.create(name='allie')
    patient1 = Patient.objects.create(name='carol')
    patient2 = Patient.objects.create(name='duke')
    
    # 1. Reservation class를 통한 예약 생성
    reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
    reservation1.save()
    doctor1.patient_set.all()
    patient1.doctors.all()
    
    # 2. Patient 객체를 통한 예약 생성
    patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
    doctor1.patient_set.all()
    patient2.doctors.all()
    ```
    
    - 예약일은 자동으로 들어가게 설정
    - `through_defaults={key:val}`
3. 생성된 예약 확인
    
    
    | id | symptom | reserved_at | doctor_id | patient_id |
    | --- | --- | --- | --- | --- |
    | 1 | headache | (자동생성) | 1 | 1 |
    | 2 | flu | (자동생성) | 1 | 2 |
4. 예약 삭제
    
    ```python
    doctor1.patient_set.remove(patient1)
    patient2.doctors.remove(doctor1)
    ```
    

## `ManyToManyField(to, **options)`

1. 설명 :
    - Django에서 자동으로 중개모델을 생성
    - M:N 관계 설정 모델 필드
2. 특징 :
    - 양방향 관계 : 어느 모델에서든 관련 객체에 접근할 수 있음
    - 중복 방지 : 동일한 관계는 한 번만 저장됨

### 대표 인자 3가지

1. `related_name` arguments :
    - 역참조시 사용하는 manager name을 변경
    
    ```python
    class Patient(models.Model):
    	doctors = models.MantToManyField(Doctor, related_name='patients')
    	name = models.TextField()
    ```
    
    ```python
    # 변경 전
    doctor.patient_set.all()
    
    # 변경 후 (이전 manager name은 사용 불가)
    doctor.patients.all()
    ```
    
2. `symmetrical` arguments 
    - 관계 설정 시 대칭 유무 설정
    - ManyToMantField가 동일한 모델을 가리키는 정의에서만 사용
    - True (기본값)
        - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면 자동으로 target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함(대칭)
            - source 모델 : 관계를 시작하는 모델
            - target 모델 : 관계의 대상이 되는 모델
        - 즉, A가 B의 친구라면 B도 A의 친구가 됨
    - False:
        - True의 반대 (대칭되지 않음)
    
    ```python
    # 예시
    
    class Person(modles.Model):
    	friends = models.ManyToManyField('self', symmetircal=False)
    ```
    
3. `through` arguments
    - 사용하고자 하는 중개 모델을 지정
    - 일반적으로 **추가 데이터를 M:N 관계와 연결하려는 경우**에 활용

### M:N에서 대표 조작 methods

1. `add()` 
    - 관계 추가
    - 지정된 객체를 관련 객체 집합에 추가
2. `remove()`
    - 관계 제거
    - 관련 객체 집합에서 지정된 모델 객체를 제거

## M:N 관계 주요 사항

1. M:N 관계로 맺어진 테이블에는 물리적인 변화가 없음
2. ManyToManyField는 중개 테이블을 자동으로 생성
3. ManyToManyField는 M:N 관계를 맺는 두 모델 어디에 위치해도 상관 없음
    - 대신 필드 작성 위치에 따라 참조와 역참조 방향을 주의할 것
4. N:1은 완전한 종속의 관계였지만 M:N은 종속적인 관계가 아님
    - 의사에게 진찰받는 환자 / 환자를 진찰하는 의사 이렇게 2가지 표현 모두 가능

## 좋아요 기능 구현

### 모델 관계 설정

1. Article(M) - User(N) 
    - 0개 이상의 게시글은 0명 이상의 회원과 관련
    - 게시글은 회원으로부터 0개 이상의 좋아요를 받을 수 있고, 회원은 0개 이상의 게시글에 좋아요를 누를 수 있음
2. Article 클래스에 ManyToManyField 작성
    
    ```python
    # articles/models.py
    
    class Article(models.Model):
        user = models.ForeignKey(
            settings.AUTH_USER_MODEL, on_delete=models.CASCADE
        )
        # 이 부분
        like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
        title = models.CharField(max_length=10)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    ```
    
3. 에러 : migration 진행 후 에러 발생
    
    ```bash
    articles.Article.like_users: (fields.E304) Reverse accessor 'User.article_set' for 'articles.Article.like_users' clashes with reverse accessor for 'articles.Article.user'.
            HINT: Add or change a related_name argument to the definition for 'articles.Article.like_users' or 'articles.Article.user'.
    articles.Article.user: (fields.E304) Reverse accessor 'User.article_set' for 'articles.Article.user' clashes with reverse accessor for 'articles.Article.like_users'.
            HINT: Add or change a related_name argument to the definition for 'articles.Article.user' or 'articles.Article.like_users'.
    ```
    
    - 역참조 명이 같은 오류 발생
        - Article - User → N:1
            - 유저가 작성한 게시글
            - user가 article 역참조 → user.article_set 이 존재했음
        - Article - User → M:N
            - 유저가 좋아요 한 게시글
            - ManyToManyField를 article에 적었기 때문에, user가 article 역참조 → user.article_set
    - 해결방법 2가지
        - ManyToMantField를 User로 옮기기
        - 역참조 이름 바꾸기 : `user.like_articles`
            - foreign 키 역참조 이름, ManyToManyField 역참조 이름 중 무얼 바꿀까
                - 의미가 통하기 위해 ManyToMantField의 역참조 이름을 변경
4. models.py 최종 수정
    
    ```python
    # articles/models.py
    
    class Article(models.Model):
        ...
        like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
        ...
    ```
    
5. User-Article 간 사용 가능한 전체 related manager
    - `article.user` : N:1, 게시글을 작성한 유저
    - `user.article_set` : N:1, 유저가 작성한 게시글(역참조)
    - `article.like_users` : M:N, 게시글을 좋아요 한 유저
    - `user.like_articles` : M:N, 유저가 좋아요 한 게시글(역참조)

### 기능 구현

1. `url.py`
    
    ```python
    # articles/urls.py
    
    urlpatterns = [
    	...
    	path('<int:article_pk>/likes/', views.likes, name='likes'),
    ]
    ```
    
2. `views.py`
    
    ```python
    # articles/views.py
    
    def likes(request, article_pk):
        # POST GET 나눌 필요가 있을까
        # GET : 좋아요 페이지를 주세요 -> 존재 X
        # 따라서 POST만 존재 -> 나눌 필요 X
        # 어떤 글에 좋아요를 눌렀는지 조회
        article = Article.objects.get(pk=article_pk)
    
        # 좋아요를 추가하는 것인지 / 취소하는 것인지
        # 좋아요 취소 : 좋아요를 요청한 유저가 해당 글의 좋아요를 누른 유저 목록에 포함되어 있다면
        if request.user in article.like_users.all():
            article.like_users.remove(request.user)
        # 좋아요 추가 : 좋아요를 요청한 유저가 해당 글의 좋아요를 누른 유저 목록에 없다면
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    ```
    
3. template
    
    ```HTML
    <!-- articles/index.html -->
    
    {% for article in articles %}
        <p>작성자: {{ article.user.username }}</p>
        <p>글 번호: {{ article.pk }}</p>
        <a href="{% url "articles:detail" article.pk %}">
          <p>글 제목: {{ article.title }}</p>
        </a>
        <p>글 내용: {{ article.content }}</p>
        {% comment %} 좋아요 form 버튼 {% endcomment %}
        <form action="{% url "articles:likes" article.pk %}" method="POST">
          {% csrf_token %}
          {% if reqeust.user in article.like_users.all %}
            <input type="submit" value="좋아요 취소">
          {% else %}
            <input type="submit" value="좋아요">
          {% endif %}
        </form>
        <hr>
      {% endfor %}
    ```