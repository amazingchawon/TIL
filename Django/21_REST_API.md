# REST API

## API Application Programming Interface

1. 설명 :
    - 두 소프트웨어가 서로 통신할 수 있게 하는 메커니즘
    - 클라이언트-서버처럼 서로 다른 프로그램에서 요청과 응답을 받을 수 있도록 만든 체계
2. 역할 :
    - 우리 집 냉장고에 전기를 공급해야 한다고 가정
    - 냉장고의 플러그에 소켓에 꽂으면 제품이 작동
        - 중요한 것은 우리가 가전 제품에 전기를 공급하기 위해 직접 배선을 하지 않음
        - 이는 매우 위험하면서도 비효율적이기 때문
    - 복잡한 코드를 추상화하여 대신 사용할 수 있는 몇 가지 더 쉬운 구문을 제공

## Web API

1. 설명 :
    - 웹 서버 또는 웹 브라우저를 위한 API
    - 현대 웹 개발은 하나부터 열까지 직접 개발하기보다 여러 Open API들을 활용하는 추세
2. 대표적인 Third Party Open API 서비스 목록
    - Youtube API
    - Google Map API
    - Naver Papago API
    - kakao Map API

## REST API

1. REST Representational State Transfer :
    - API Server를 개발하기 위한 일종의 소프트웨어 설계 방법론, 규칙이 아님
    - API Server를 설계하는 구조가 서로 다르니 이렇게 맞춰서 설계하는 게 어때?
2. RESTful API
    - REST 원리를 따르는 시스템을 RESTful 하다고 부름
    - 자원을 정의하고 자원에 대한 주소를 지정하는 전반적인 방법을 서술
3. REST API
    - REST 라는 설계 디자인 약속을 지켜 구현한 API

### REST에서 자원을 정의하고 주소를 지정하는 방법

1. 자원의 식별 : URI
2. 자원의 행위 : HTTP Methods
3. 자원의 표현 : JSON 데이터 (궁극적으로 표현되는 데이터 결과물)

## 자원의 식별

1. URI Uniform Resource Identifier :
    - 통합 자원 식별자
    - 인터넷에서 리소스(자원)을 식별하는 문자열
    - 가장 일반적인 URI는 웹 주소로 알려진 URL

### URL Uniform Resource Locator

1. 설명 :
    - 통합 자원 위치
    - 웹에서 주어진 리소스의 주소
    - 네트워크 상에 리소스가 어디 있는지 알려주기 위한 약속
- 구성
    - http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument

### 구성

1. Schema (or Protocol) `http`
    - 브라우저가 리소스를 요청하는 데 사용해야 하는 규약
    - URL의 첫 부분은 브라우저가 어떤 규약을 사용하는지를 나타냄
    - 기본적으로 웹은 http(s)를 요구
        - 메일을 열기 위한 mailto: 파일을 전송하기 위한 ftp: 등 다른 프로토콜도 존재
2. Domain Name `www.example.com`
    - 요청 중인 웹 서버를 나타냄
    - 어떤 웹 서버가 요구되는 지를 가리키며 직접 IP 주소를 사용하는 것도 가능하지만, 사람이 외우기 어렵기 때문에 주로 Domain Name으로 사용
        - 도메인 google.com의 IP 주소는 142.251.42.142
3. Port `80`
    - 웹 서버의 리소스에 접근하는데 사용되는 기술적인 문(Gate)
    - HTTP 프로토콜의 표준 포트
        - HTTP - 80
        - HTTPS - 443
    - 표준 포트만 작성 시 생략 가능
4. Path `/path/to/myfile.html`
    - 웹 서버의 리소스 경로
    - 초기에는 실제 파일이 위치한 물리적 위치를 나타냈지만, 오늘날은 실제 위치가 아닌 추상화된 형태의 구조를 표현
        - /articles/create/라는 주소가 실제 articles 폴더 안에 create 폴더안을 나타내는 것은 아님
5. Parameters `?key1=value1&key2=value2`
    - 웹 서버에 제공하는 추가적인 데이터
        - GET method일 때만 사용
    - ‘&’ 기호로 구분되는 key-value 쌍 목록
    - 서버는 리소스를 응답하기 전에 이러한 파라미터를 사용하여 추가 작업을 수행할 수 있음
6. Anchor `#SomewhereInTheDocument`
    - 일종의 “북마크”를 나타내며 브라우저에 해당 지점에 있는 콘텐츠를 표시
    - ‘#’ (fragment identifier : 부분 식별자)  이후 부분은 서버에 전송되지 않음
    - https://docs.djangoproject.com/en/4.2/intro/install/#quick-install-guide
        - 요청에서 #quick-install-guide는 서버에 전달되지 않고 브라우저에게 해당 지점으로 이동할 수 있도록 함

## 자원의 행위

### HTTP Request Methods

1. 설명 :
    - 리소스에 대한 행위(수행하고자 하는 동작)를 정의
    - HTTP verbs라고도 함
2. 대표 HTTP Request Methods
    - GET
        - 서버에 리소스의 표현을 요청
        - GET을 사용하는 요청은 데이터만 검색해야 함
    - POST
        - 데이터를 지정된 리소스에 제출
        - 서버의 상태를 변경
    - PUT
        - 요청한 주소의 리소스를 수정
    - DELETE
        - 지정된 리소스를 삭제

### HTTP response status codes

1. 설명 : 특정 HTTP 요청이 성공적으로 완료 되었는지 여부를 나타냄
2. 분류 :
    - `100-199` : Informational responses
    - `200-299` : Successful responses
    - `300-399` : Redirection messages
    - `400-499` : Client error responses
    - `500-599` : Server error responses

## 자원의 표현

1. 그동안 서버가 응답(자원을 표현)했던 것
    - 지금까지 Django 서버는 사용자에게 페이지(html)만 응답하고 잇었음
    - 하지만 서버가 응답할 수 있는 것은 페이지 뿐만 아니라 다양한 데이터 타입을 응답할 수 있음
    - REST API는 이 중에서도 JSON 타입으로 응답하는 것을 권장
2. 응답 데이터 타입의 변화
    - Django는 더 이상 Template 부분에 대한 역할을 담당하지 않게 되며, Front-end와 Back-end가 분리되어 구성됨
    - 클라이언트 - front-end framework ← (json) - server(django)