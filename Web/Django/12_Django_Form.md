# Django From

## HTML `form`

1. 설명 :
    - 지금까지 사용자로부터 데이터를 제출 받기 위해 활용한 방법
    - but, 비정상적/악의적인 요청을 필터링 할 수 없음
    - → 유효한 데이터인지에 대한 확인이 필요

## 유효성 검사

1. 정의 : 수집한 데이터가 정확하고 유효한지 확인하는 과정
2. 유효성 검사 구현의 어려움
    - 입력 값, 형식, 중복, 범위, 보안 등 고려해야할 것이 많음
    - → 이런 과정과 기능을 직접 개발하는 것이 아닌, Django가 제공하는 Form을 사용

## Form Class

1. Django Form
    - 사용자 입력 데이터를 수집하고, 처리 및 유효성 검사를 수행하기 위한 도구
    - → 유효성 검사를 단순화하고 자동화 할 수 있는 기능을 제공
2. Form Class 정의
    
    ```python
    # articles/forms.py
    
    from django import forms
    
    class ArticleForm(forms.Form):
    	title = forms.CharField(max_length=10)
    	content = forms.CharField(widget=forms.Textarea)
    ```
    
    - `widget=forms.Textarea` : widget 문법
3. Model Class와 비교
    
    ```python
    from django.db import models
    
    # Create your models here.
    class Article(models.Model):
        title = models.CharField(max_length=10)
        content = models.TextField()
        created_at = models.DateTimeField(auto_now_add=True)
        updated_at = models.DateTimeField(auto_now=True)
    ```
    
    - Form에는 TextField가 없음
4. HTML form을 사용했던 로직 → Django form으로 변경 (CRUD 실습 참고)
    - [`views.py`](http://views.py) 수정
        
        ```python
        from .forms import ArticleForm
        
        ...
        
        def new(request):
            form = AriticleForm()
            context = {
                'form': form,
            }
            # 게시글 작성 페이지 응답
            return render(request, 'articles/new.html', context)
        ```
        
        - ArticleForm import
    - `new.html` 수정
        
        ```html
        <h1>New</h1>
          <form action="{% url "articles:create" %}" method="POST">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit">
          </form>
        ```
        

### Form rendering opitons

1. `(form객체).as_태그`
    - `as_p` : \<p> 태그로 감싸서 render
    - `as_div` : \<div> 태그로 감싸서 render
    - `as_table` : \<table> 태그로 감싸서 render
    - `as_ul` : \<ul> 태그로 감싸서 render


## Widgets

1. 정의 : HTML input element의 표현을 담당
2. 사용법 : form 필드의 인자로 사용

# Django ModelForm

1. Django Form VS. Django ModelForm
    - Form : 사용자 입력 데이터를 DB에 저장하지 않을 때
        - (ex) 검색, 로그인
    - ModelForm : 사용자 입력 데이터를 DB에 저장해야 할 때
        - (ex) 게시글 작성, 회원가입
2. ModelForm
    - 특징 : Model과 연결된 Form을 자동으로 생성해주는 기능을 제공
    - ArticleForm 클래스 수정
        
        ```python
        class ArticleForm(forms.ModelForm):
        	class Meta:
        		# Meta : 데이터의 데이터
        		model = Article
        		fields = '__all__'
        ```
        

## Meta class

1. 정의 : ModelForm의 정보를 작성하는 곳
2. 속성 :
    - `__all__` : 모두 포함
    - `fields` : 포함하고 싶은 것
        
        ```python
        ...
        fields = ('title',)
        ```
        
    - `exclude`
        
        ```python
        ...
        exclude = ('title',)
        ```
        
3. 주의사항 :
    - Django에서 ModelForm에 대한 추가 정보나 속성을 작성하는 클래스 구조를 Meta 클래스로 작성 했을 뿐 → 파이썬의 inner class와 같은 문법적인 관점으로 접근하지 말 것

## ModelForm 적용

1. create 로직
    
    ```python
    # articles/views.py
    
    from .forms import ArticleForm
    
    def create(request):
        # 1. 모델폼 인스턴스 생성 (+ 사용자 입력 데이터를 통째로 인자로 작성)
        form = ArticleForm(request.POST)
    
        # 2. 유효성 검사
        if form.is_valid():
            # 3. 저장
            article = form.save()
            return redirect('articles:detail', article.pk)
    
        context = {
            'form': form,
        }
        return render(request, 'articles/new.html', context)
    ```
    
2. edit 로직
    
    ```python
    # articles/views.py
    
    from .forms import ArticleForm
    
    def edit(request, pk):
        # 어떤 게시글을 수정할지 조회
        article = Article.objects.get(pk=pk)
        form = ArticleForm(instance=article)
        context = {
            'article': article,
            'form': form,
        }
        return render(request, 'articles/edit.html', context)
    ```
    
3. update 로직
    
    ```python
    def update(request, pk):
        article = Article.objects.get(pk=pk)
        # 1. 모델 폼 인스턴스 생성 (+ 사용자 입력 데이터 & 기존 데이터)
        form = ArticleForm(request.POST, instance=article)
        # 2. 유효성 검사
        if form.is_valid():
            form.save()
            return redirect('articles:detail', article.pk)
        context = {
            'article': article,
            'form': form,
        }
        return render(request, 'articles/edit.html', context)
    ```
    

### `is_vaild()`

1. 설명 : 여러 유효성 검사를 실행하고, 데이터가 유효한지 여부를 boolean으로 반환
2. 에러메시지 출력 과정
    - 유효하지 않은 입력은 `is_vaild()`에 의해 False로 평가
    - form 객체에는 그에 맞는 에러 메시지가 포함 → 다음 코드로 진행

### `Save()`

1. 정의 : 데이터베이스 객체를 만들고 저장하는 ModelForm의 인스턴스 메서드
2. 생성과 수정을 구분하는 법
    - ModelForm 인스턴스 생성 시, 키워드 인자 `instance` 여부를 통해 생성할 지, 수정할지 구분
3. ModelForm의 키워드 인자 구성
    - `ArticleForm(request.POST, instance=article)`
    - `request.POST` 는 data
        - data는 첫번째에 위치한 키워드 인자이기 때문에 따로 `data=` 이라고 명시해줄 필요 X
    - `instance` 는 9번째에 위치한 키워드 인자이기 때문에 생략 불가

## Django Form 정리

- 사용자로부터 데이터를 수집하고 처리하기 위한 강력하고 유연한 도구
- HTML form의 생성, 데이터 유효성 검사 및 처리를 쉽게 할 수 있도록 도움

# HTTP 요청 다루기

## View 함수 구조 변화

1. new / create view 함수
    - 공통점 : 데이터 생성을 구현하기 위함
    - 차이점 : new는 GET method 요청, create은 POST method 요청 처리
2. 함수 구조 변화 : HTTP request method 차이점을 활용해 동일한 목적을 가지는 2개의 view 함수를 하나로 구조화
    - new/create
        
        ```python
        def create(request):
            if request.methon ==  'POST':
                form = ArticleForm(request.POST)
                if form.is_valid():
                    article = form.save()
                    return redirect('articles:detail', article.pk)
            # 요청 메서드가 POST가 아닐때(GET, PUT, DELETE 등 다른 메서드),
            else:
                form = ArticleForm()
            context = {
                'form': form
            }
            return render(request, 'articles/new.html, context')
        ```
        
        - context에 담기는 form은
            - is_vaild()를 통과하지 못해 에러메세지를 담은 form이거나
            - else 문을 통과한 form 인스턴스
        - new 함수, create 함수 합치고 new 관련 키워드를 create으로 변경
    - edit/update
        
        ```python
        def update(request, pk):
            article = Article.objects.get(pk=pk)
            if request.method == 'POST':
                form = ArticleForm(request.POST, instance=article)
                if form.is_valid():
                    form.save()
                    return redirect('articles:detail', article.pk)
            else:
                form = ArticleForm(instance=article)
            context = {
                'article': article,
                'form': form,
            }
            return render(request, 'articles/update.html', context)
        ```