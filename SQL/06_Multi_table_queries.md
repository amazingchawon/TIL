# Multi table queries

## join

1. 관계 : 여러 테이블 간의 논리적 연결
2. 관계의 필요성 :
    - 테이블을 분리하면 데이터 관리는 용이해질 수 있으나 출력시에는 문제가 있음
    - 테이블 한 개 만을 출력할 수 밖에 없어 다른 테이블과 결합하여 출력하는 것이 필요해짐
    - → 이때 사용하는 것이 JOIN

## Joining tables

### JOIN clause

1. 설명 : 둘 이상의 테이블에서 데이터를 검색하는 방법
    - JOIN의 종류 :
        - INNER JOIN
        - LEFT JOIN
2. 사전 준비
    
    ```sql
    CREATE TABLE users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name VARCHAR(50) NOT NULL
    );
    
    CREATE TABLE articles (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      title VARCHAR(50) NOT NULL,
      content VARCHAR(100) NOT NULL,
      userId INTEGER NOT NULL,
      FOREIGN KEY (userId) 
        REFERENCES users(id)
    );
    
    INSERT INTO 
      users (name)
    VALUES 
      ('하석주'),
      ('송윤미'),
      ('유하선');
    
    INSERT INTO
      articles (title, content, userId)
    VALUES 
      ('제목1', '내용1', 1),
      ('제목2', '내용2', 2),
      ('제목3', '내용3', 1),
      ('제목4', '내용4', 4),
      ('제목5', '내용5', 1);
    ```
    

### INNER JOIN clause

1. 설명 : 두 테이블에서 값이 일치하는 레코드에 대해서만 결과를 반환
2. INNER JOIN syntax
    
    ```sql
    SELECT
    	select_list
    FROM 
    	table_a
    INNER JOIN
    	table_b
    	ON table_b.fk = table_a.pk
    ```
    
    - FROM 절 이후 메인 테이블 지칭(table_a)
    - INNER JOIN 절 이후 메인 테이블과 조인할 테이블을 지정(table_b)
    - ON 키워드 이후 조인 조건을 작성
    - 조인 조건은 table_a와 table_b 간의 레코드를 일치시키는 규칙을 지정
3. INNER JOIN 예시
    - articles
        
        
        | id | title | content | userId |
        | --- | --- | --- | --- |
        | 1 | 제목1 | 내용1 | 1 |
        | 2 | 제목2 | 내용2 | 2 |
        | 3 | 제목3 | 내용3 | 1 |
        | 4 | 제목4 | 내용4 | 4 |
        | 5 | 제목5 | 내용5 | 1 |
    - users
        
        
        | id | name |
        | --- | --- |
        | 1 | 하석주 |
        | 2 | 송윤미 |
        | 3 | 유하선 |
    - 작성자가 있는 (존재하는 회원) 모든 게시글을 작성자 정보와 함께 조회
        
        ```sql
        SELECT
        	*
        FROM
        	articles
        INNER JOIN
        	users
        	ON users.id = articles.userID;
        ```
        
    - 결과
        
        
        | id | title | content | userId | id | name |
        | --- | --- | --- | --- | --- | --- |
        | 1 | 제목1 | 내용1 | 1 | 1 | 하석주 |
        | 2 | 제목2 | 내용2 | 2 | 2 | 송윤미 |
        | 3 | 제목3 | 내용3 | 1 | 1 | 하석주 |
        | 5 | 제목5 | 내용5 | 1 | 1 | 하석주 |
4. INNER JOIN 활용
    - 1번 회원(하석주)가 작성한 모든 게시글의 제목과 작성자명을 조회
        
        ```sql
        SELECT
        	articles.title, users.name
        FROM
        	articles
        INNER JOIN
        	users
        	ON users.id = articles.userId
        WHERE users.id = 1;
        ```
        

### LEFT JOIN

1. 설명 : 오른쪽 테이블의 일치하는 레코드와 함께 왼쪽 테이블의 모든 레코드 반환
2. LEFT JOIN syntax
    
    ```sql
    SELECT
    	select_list
    FROM
    	table_a
    LEFT JOIN
    	table_b
    	ON table_b.fk = table_a.pk;
    ```
    
    - FROM 절 이후 왼쪽 테이블 지정 (table_a)
    - LEFT JOIN 절 이후 오른쪽 테이블 지정 (table_b)
    - ON 키워드 이후 조인 조건을 작성
        - 왼쪽 테이블의 각 레코드를 오른쪽 테이블의 모든 레코드와 일치시킴
3. LEFT JOIN 예시
    - articles
        
        
        | id | title | content | userId |
        | --- | --- | --- | --- |
        | 1 | 제목1 | 내용1 | 1 |
        | 2 | 제목2 | 내용2 | 2 |
        | 3 | 제목3 | 내용3 | 1 |
        | 4 | 제목4 | 내용4 | 4 |
        | 5 | 제목5 | 내용5 | 1 |
    - users
        
        
        | id | name |
        | --- | --- |
        | 1 | 하석주 |
        | 2 | 송윤미 |
        | 3 | 유하선 |
    - 모든 게시글을 작성자 정보와 함께 조회
        
        ```sql
        SELECT
        	*
        FROM
        	articles
        LEFT JOIN
        	users
        ON users.id = articles.userId;
        ```
        
    - 결과
        
        
        | id | title | content | userId | id | name |
        | --- | --- | --- | --- | --- | --- |
        | 1 | 제목1 | 내용1 | 1 | 1 | 하석주 |
        | 2 | 제목2 | 내용2 | 2 | 2 | 송윤미 |
        | 3 | 제목3 | 내용3 | 1 | 1 | 하석주 |
        | 4 | 제목4 | 내용4 | 4 | NULL | NULL |
        | 5 | 제목5 | 내용5 | 1 | 1 | 하석주 |
4. LEFT JOIN 특징
    - 왼쪽 테이블의 모든 레코드를 표시
    - 오른쪽 테이블과 매칭되는 레코드가 없으면 NULL 표시
5. LEFT JOIN 활용
    - 게시글을 작성한 이력이 없는 회원 정보 조회
        
        ```sql
        SELECT
        	articles.name
        FROM
        	users
        LEFT JOIN
        	articles
        	ON articles.userId = users.id
        WHERE articles.userId is NULL;
        ```