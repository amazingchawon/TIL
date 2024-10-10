# Modifying Data

## Insert data

### INSERT statement

1. 설명 : 테이블 레코드 삽입
2. INSERT syntax
    
    ```sql
    INSERT INTO
    	table_name (c1, c2, ...)
    VALUES
    	(v1, v2, ...);
    ```
    
    - INSERT INTO 절 다음에 테이블 이름과 괄호 안에 필드 목록 작성
    - VALUES 키워드 다음 괄호 안에 해당 필드에 삽입할 값 목록 작성
3. INSERT 활용
    - articles 테이블에 다음과 같은 데이터 입력
        
        ```sql
        INSERT INTO
        	articles (title,content, createdAt)
        VALUES
        	('hello', 'world', '2000-01-01');
        ```
        
    - articles 테이블에 다음과 같은 데이터 추가 입력
        
        ```sql
        INSERT INTO
        	articles (title, content, createdAt)
        VALUES
        	('title1', 'content1', '1900-01-01'),
        	('title2', 'content2', '1800-01-01'),
        	('title3', 'content3', '1700-01-01');	
        ```
        
    - DATE 함수를 사용해 articles 테이블에 다음과 같은 데이터 추가 입력
        
        ```sql
        INSERT INTO
        	articles (title, content, CreatedAt)
        VALUES
        	('mytitle', 'mycontent', DATE());
        ```
        

## Update data

### UPDATE statement

1. 설명 : 테이블 레코드 수정
2. UPDATE syntax
    
    ```sql
    UPDATE
    	table_name
    SET column_name = expression,
    [WHERE
    	condition];
    ```
    
    - SET 절 다음에 수정 할 필드와 새 값을 지정
    - WHERE 절에서 수정 할 레코드를 지정하는 조건 작성
    - WHERE 절을 작성하지 않으면 모든 레코드를 수정
3. UPDATE 활용
    - articles 테이블 1번 레코드의 title 필드 값을 ‘update Title’로 변경
        
        ```sql
        UPDATE articles
        SET title = 'update Title'
        WHERE id = 1;
        ```
        
    - articles 테이블 2번 레코드의 title, content 필드 값을 각각 ‘update Title’, ‘update Content’로 변경
        
        ```sql
        UPDATE articles
        SET
        	title = 'update Title',
        	content = 'update Content'
        WHERE id = 2;
        ```
        

## Delete data

### DELETE statement

1. 설명 : 테이블 레코드 삭제
2. DELETE syntax
    
    ```sql
    DELETE FROM table_name
    [WHERE
    	condition];
    ```
    
    - DELETE FROM 절 다음에 테이블 이름 작성
    - WHERE 절에서 삭제할 레코드를 지정하는 조건 작성
    - WHERE 절을 작성하지 않으면 모든 레코드를 삭제
3. DELETE 활용
    - articles 테이블의 1번 레코드 삭제
        
        ```sql
        DELETE FROM articles
        WHERE id = 1;
        ```
        
    - articles 테이블에서 작성일이 오래된 수능로 레코드 2개 삭제
        
        ```sql
        DELETE FROM
        	articles
        WHERE id IN (
        	SELECT id FROM articles
        	ORDER BY createdAt
        	LIMIT 2
        );
        ```
        

## 참고 : SQLite의 날짜 및 시간

1. SQLite의 날짜 및 시간을 저장하기 위한 별도 데이터 타입이 없음
2. 대신 날짜 및 시간에 대한 함수를 사용해 표기 형식에 따라 TEXT, REAL, INTEGER 값으로 저장