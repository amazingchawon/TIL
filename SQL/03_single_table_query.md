# Single Table Queries

## Querying data : `SELECT` statment

1. 설명 : 테이블에서 데이터를 조회
2. SELECT syntax
    
    ```sql
    SELECT
    	select_list
    FROM
    	table_name;
    ```
    
    - `SELECT` 키워드 이후 데이터를 선택하려는 필드를 하나 이상 지정
    - `FROM` 키워드 이후 데이터를 선택하려는 테이블의 이름을 지정
3. `SELECT` 활용
    - 테이블 employees에서 LastName 필드의 모든 데이터를 조회
        
        ```sql
        SELECT LastName
        FROM employees;
        ```
        
    - 테이블 employees에서 LastName, FirstName 필드의 모든 데이터를 조회
        
        ```sql
        SELECT LastName, FirstName
        FROM employess;
        ```
        
    - 테이블 employees에서 모든 필드의 데이터를 조회
        
        ```sql
        SELECT *
        FROM employees;
        ```
        
    - 테이블 employees에서 FirstName 필드의 모든 데이터를 조회. 단, 조회 시 FirstName이 아닌 ‘이름’으로 출력 될 수 있도록 변경
        
        ```sql
        SELECT FirstName AS '이름'
        FROM employees;
        ```
        

## Sorting data : `ORDER BY` statement

1. 설명 : 조회 결과의 레코드를 정렬
2. ORDER BY syntax
    
    ```sql
    ORDER BY
    	column1 [ASC|DESC],
    	column2 [ASC|DESC],
    	...
    ```
    
    - FROM clause(절) 뒤에 위치
    - 하나 이상의 컬럼을 기준으로 결과를 오름차순(ASC, 기본 값), 내림차순(DESC)으로 정렬
3. ORDER BY 활용
    - 테이블 employees에서 FirstName 필드의 모든 데이터를 오름차순으로 조회
        
        ```sql
        SELECT FirstName
        FROM employees
        ORDER BY FirstName;
        ```
        
    - 테이블 employees에서 FirstName 필드의 모든 데이터를 내림차순으로 조회
        
        ```sql
        SELECT FirstName
        FROM employees
        ORDER BY FirstName DESC;
        ```
        
    - 테이블 customers에서 Country 필드를 기준으로 내림차순 정렬한 다음 City 필드 기준으로 오름차순 정렬하여 조회
        
        ```sql
        SELECT Country, City
        FROM customers
        ORDER BY Country DESC, City
        ```
        

### 특징

1. 정렬에서의 NULL : NULL 값이 존재할 경우 오름차순 정렬 시 결과에 NULL이 먼저 출력
2. SELECT statement 실행 순서
    - FROM → SELECT → ORDER BY
    - 테이블에서 → 조회하여 → 정렬

## Filtering data

1. Filtering data 관련 Keywords
    
    
    | Clause | Operator |
    | --- | --- |
    | DISTINCT | BETWEEN |
    | WHERE | IN |
    | LIMIT | LIKE |
    |  | Comparison |
    |  | Logical |

### DISTINCT statement

1. 설명 : 조회 결과에서 중복된 레코드를 제거
2. DISTINCT syntax
    
    ```sql
    SELECT DISTINCT select_list
    FROM table_name;
    ```
    
    - SELECT 키워드 바로 뒤에 작성해야 함
    - SELECT DISTINCT 키워드 다음에 고유한 값을 선택하려는 하나 이상의 필드를 지정
3. DISTINCT 활용
    - 테이블 customers에서 Country 필드의 모든 데이터를 오름차순 조회
        
        ```sql
        SELECT DISTINCT Country
        FROM customers
        ORDER BY Country;
        ```
        

### WHERE statement

1. 설명 : 조회 시 특정 검색 조건을 지정
2. WHERE syntax
    
    ```sql
    SELECT
    	select_list
    FROM
    	table_name
    WHERE
    	search_condition;
    ```
    
    - FROM clause 뒤에 위치
    - search_condition은 비교연산자 및 논리연산자(AND, OR, NOT 등)를 사용하는 구문이 사용됨
3. WHERE 활용
    - 테이블 customers에서 City 필드 값이 ‘Prague’인 데이터의 LastName, FirstName, City 조회
        
        ```sql
        SELECT LastName, FirstName, City
        FROM customers
        WHERE City = 'Prague';
        ```
        
    - 테이블 customers에서 City 필드 값이 ‘Prague’가 아닌 데이터의 LastName, FirstName, City조회
        
        ```sql
        SELECT
        	LastName, FirstName, City
        FROM
        	customers
        WHERE
        	City != 'Prague';
        ```
        
    - 테이블 customers에서 Company 필드 값이 NULL이고 Country 필드 값이 ‘USA’인 데이터의 LastName, FirstName, Company, Country 조회
        
        ```sql
        SELECT LastName, FirstName, Company, Country
        FROM customers
        WHERE Country = 'USA';
        ```
        
    - 테이블 customers에서 Company 필드 값이 NULL이거나 Country 필드 값이 ‘USA’인 데이터의 LastName, FirstName, Company, Country 조회
        
        ```sql
        SELECT LastName, FirstName, Company, Country
        FROM customers
        WHERE
        	Company IS NULL
        	OR Country = 'USA';
        ```
        
    - 테이블 tracks에서 Bytes 필드 값이 10,000 이상 500,000 이하인 데이터의 Name, Bytes 조회
        
        ```sql
        SELECT Name, Bytes
        FROM tracks
        WHERE
        	Bytes BETWEEN 100000 AND 500000;
        ```
        
    - 테이블 customers에서 Country 필드 값이 ‘Canada’ 또는 ‘Germany’ 또는 ‘France’인 데이터의 LastName, FirstName, Country 조회
        
        ```sql
        SELECT LastName, FirstName, Country
        FROM customers
        WHERE
        	Country IN (‘Canada’, ‘Germany’, ‘France’);
        ```
        
    - 테이블 customers에서 LastName 필드 값이 ‘son’으로 끝나는 데이터의 LastName, FirstName 조회
        
        ```sql
        SELECT LastName, FirstName
        FROM customers
        WHERE LastName LIKE '%son';
        ```
        
    - 테이블 customers에서 FirstName 필드 값이 4자리면서 ‘a’로 끝나는 데이터의 LastName, FirstName 조회
        
        ```sql
        SELECT LastName, FirstName
        FROM customers
        WHERE FirstName LIKE '___a';
        ```
        

### Operators

1. Comparison Operators 비교 연산자
    - =, >=, <=, IS, LIKE, IN, BETWEEN…AND
2. Logical Operators 논리 연산자
    - AND &&
    - OR ||
    - NOT !
3. IN Operator : 값이 특정 목록 안에 있는지 확인
4. LIKE Operator : 값이 특정 패턴에 일치하는지 확인, Wildcards와 함께 사용
    - Wildcard Charaters
        - % : 0개 이상의 문자열과 일치하는지 확인
        - ‘_’ : 단일 문자와 일치하는지 확인

### LIMIT clause

1. 설명 : 조회하는 레코드 수를 제한
2. LIMIT syntax
    
    ```sql
    SELECT
    	select_list
    FROM
    	table_name
    LIMIT
    	[offset,] row_count;
    ```
    
    - 하나 또는 두 개의 인자를 사용 (0 또는 양의 정수)
    - row_count는 조회하는 최대 레코드 수를 지정
3. LIMIT 활용
    - 테이블 tracks에서 TrackId, Name, Bytes 필드 데이터를 Bytes 기준 내림차순으로 7개만 조회
        
        ```sql
        SELECT TrackId, Name, Bytes
        FROM tracks
        ORDER BY Bytes DESC
        LIMIT 7;
        ```
        
    - 테이블 tracks에서 TrackId, Name, Bytes 필드 데이터를 Bytes 기준 내림차순으로 4번째부터 7번째 데이터만 조회
        
        ```sql
        SELECT TrackId, Name, Bytes
        FROM tracks
        ORDER BY Bytes DECS
        LIMIT 3, 4;
        ```
        

## Grouping data

### GROUP BY clause

1. 설명 : 레코드를 그룹화하여 요약본 생성, 집계함수와 함께 사용
2. Aggregation Functions 집계 함수
    - 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수
    - `SUM`, `AVG`, `MAX`, `COUNT`
3. GROUP BY syntax
    
    ```sql
    SELECT
    	c1, c2, ..., cn, aggregate_funtion(ci)
    FROM
    	table_name
    GROUP BY
    	c1, c2, ..., cn;
    ```
    
    - FROM 및 WHERE 절 뒤에 배치
    - GROUP BY 절 뒤에 그룹화 할 필드 목록을 작성
4. GROUP BY 예시
    - Country 필드를 그룹화
        
        ```sql
        SELECT Country
        FROM customers
        GROUP BY Country;
        ```
        
        - DISITINCT 후 정렬한 결과와 유사
    - COUNT 함수가 각 그룹에 대해 집계된 값을 계산
        
        ```sql
        SELECT Country, COUNT(*)
        FROM customers
        GROUP BY Country;
        ```
        
5. GROUP BY 활용
    - 테이블 tracks에서 Composer 필드를 그룹화하여 각 그룹에 대한 Bytes의 평균 값을 내림차순 조회
        
        ```sql
        SELECT
        	Composer, AVG(Bytes)
        FROM
        	tracks
        GROUP BY
        	Composer;
        ORDER BY
        	AVG(Bytes) DESC;
        ```
        
    - 테이블 tracks에서 Composer 필드를 그룹화하여 각 그룹에 대한 Milliseconds의 평균 값이 10 미만인 데이터 조회  (단, Milliseconds 필드는 60,000으로 나눠 분 단위 값의 평균으로 계산)
        
        ```sql
        SELECT
        	Composer, AVG(Milliseconds/60000) AS avgOfMinutes
        FROM
        	tracks
        GROUP BY
        	Composer
        HAVING
        	avgOfMinutes < 10;
        ```
        
        - 만약 GROUP BY 위에 WHERE가 들어갔다면, `Invaild use of group function` 에러 발생
        
        ### HAVING clause
        
        1. 설명 :
            - 집계 항목에 대한 세부 조건을 지정
            - 주로 GROUP BY와 함께 사용되며 GROUP BY가 없다면 WHERE 처럼 동작

## SELECT statement 실행 순서

1. 테이블에서 FROM
2. 특정 조건에 맞추어 WHERE
3. 그룹화 하고 GROUP BY
4. 만약 그룹 중에서 조건이 있다면 맞추고 HAVING
5. 조회하여 SELECT
6. 정렬하고 ORDER BY
7. 특정 위치의 값을 가져옴 LIMIT