# Managing Tables

## Create a table

### CREATE TABLE

1. 설명 : 테이블 생성
2. CREATE TABLE syntax
    
    ```sql
    CREATE TABLE table_name (
    	column_1 data_type constraints,
    	column_2 data_type constraints,
    	...
    );
    ```
    
    - 각 필드에 적용할 데이터 타입 작성
    - 테이블 및 필드에 대한 제약조건(constaints) 작성
    - 테이블 이름은 단수로 사용하는 것이 보편적
3. CREATE TABLE 활용
    - examples 테이블 생성 및 확인
        
        ```sql
        CREATE TABLE examples (
        	ExamID INTEGER PRIMARY KEY AUTOINCREMENT,
        	LastName VARCHAR(50) NOT NULL,
        	FirstName VARCHAR(50) NOT NULL
        );
        ```
        
4. 데이터 타입
    - INTEGER : 정수
    - NULL : 아무런 값도 포함하지 않음을 나타냄
    - TEXT : 문자열
        - VARCHAR
    - BLOB : 이미지, 동영상, 문서 등의 바이너리 데이터
    - REAL : 부동 소수점
5. 제약 조건 : 
    - 테이블의 필드에 적용되는 규칙 또는 제한 사항
    - 데이터의 무결성을 유지하고 데이터베이스의 일관성을 보장
    - 대표 제약 조건 3가지 :
        - PRIMARY KEY : 해당 필드를 기본 키로 지정, INTEGER 타입에만 적용됨
        - NOT NULL : 해당 필드에 NULL 값을 허용하지 않도록 지정
        - FOREIGN KEY : 다른 테이블과의 외래 키 관계를 정의
6. AUTOINCREMENT 키워드
    - 설명 : 자동으로 고유한 정수 값을 생성하고 할당하는 필드 속성
    - 특징 :
        - 필드의 자동 증가를 나타내는 특수한 키워드
        - 주로 primary key 필드에 적용
        - INTEGER PRIMARY KEY AUTOINCREMENT가 작성된 필드는 항상 새로운 레코드에 대해 이전 최대 값보다 큰 값을 할당
        - 삭제된 값은 무시되며 재사용할 수 없게 됨

### PRAGMA

1. 설명 : 테이블 구조 Schema 확인
2. PRAGMA Syntax :
    
    ```sql
    PRAGMA table_info('examples');
    ```
    
    - cid : Column Id를 의미하며 고유한 식별자를나타내는정수 값
    - 직접 사용하지 않으며 PRAGMA 명령과 같은 메타데이터 조회에서 출력 값으로 활용됨

## Alter table

### ALTER TABLE statememt

1. 설명 : 테이블 및 필드 조작
2. 역할 :
    
    
    | 명령어 | 역할 |
    | --- | --- |
    | ALTER TABLE ADD COLUMN | 필드 추가 |
    | ALTER TABLE RENAME COLUMN | 필드 이름 변경 |
    | ALTER TABLE DROP COLUMN | 필드 삭제 |
    | ALTER TABLE RENAME TO | 테이블 이름 변경 |

### ALTER TABLE ADD COLUMN

1. 설명 : 필드 추가
2. ALTER TABLE ADD COLUMN syntax
    
    ```sql
    ALTER TABLE table_Name
    ADD COLUMN column_definition;
    ```
    
    - ADD COLUMN 이후 추가하고자 하는 새 필드 이름과 데이터 타입 및 제약 조건 작성
    - 단, 추가하고자 하는 필드에 NOT NULL 제약조건이 있을 경우 NULL이 아닌 기본 값 설정 필요
3. ALTER TABLE ADD COLUMN 활용
    - examples 테이블에 다음 조건에 맞는 Country 필드 추가
        
        ```sql
        ALTER TABLE examples
        ADD COLUMN Country VARCHAR(100) NOT NULL DEFAULT 'default value';
        ```
        
    - examples 테이블에 다음 조건에 맞는 Age, Address 필드 추가
        
        ```sql
        ALTER TABLE examples
        ADD COLUMNS	Age INTEGER NOT NULL DEFAULT 0;
        
        ALTER TABLE examples
        ADD COLUMNS	Address VARCHAR NOT NULL DEFAULT 'default value';
        ```
        
        - SQLite는 단일 문을 사용하여 한번에 여러 필드를 추가할 수 없음

### ALTER TABLE RENAME COLUMN

1. 설명 : 필드 이름 변경
2. ALTER TABLE RENAME COLUMN syntax
    
    ```sql
    ALTER TABLE
    	table_name
    RENAME COLUMN
    	current_name TO new_name
    ```
    
    - RENAME COLUMN 키워드 뒤에 이름을 바꾸려는 필드의 이름을 지정하고 TO 키워드 뒤에 새 이름을 지정
3. ALTER TABLE RENAME COLUMN 활용
    - examples 테이블 Address 필드의 이름을 PostCode로 변경
        
        ```sql
        ALTER TABLE examples
        RENAME COLUMN Address TO PostCode;
        ```
        

### ALTER TABLE RENAME TO

1. 설명 : 테이블 이름 변경
2. ALTER TABLE RENAME TO syntax
    
    ```sql
    ALTER TABLE
    	table_name
    RENAME TO
    	new_table_name
    ```
    
    - RENAME TO 키워드 뒤에 새로운 테이블 이름 지정
3. ALTER TABLE RENAME TO 활용
    - examples 테이블 이름을 new_examples로 변경
        
        ```sql
        ALTER TABLE examples
        RENAME TO new_examples;
        ```
        

## Delete a table

### DROP TABLE statement

1. 설명 : 테이블 삭제
2. DROP TABLE syntax
    
    ```sql
    DROP TABLE table_name;
    ```
    
    - DROP TABLE statement 이후 삭제할 테이블 이름 작성
3. DROP TABLE 활용
    - new_examples 테이블 삭제
        
        ```sql
        DROP TABLE new_examples;
        ```
        

## 참고

### 타입 선호도 Type Affinity

1. 설명 : 컬럼에 데이터 타입이 명시적으로 지정되지 않았거나 지원하지 않을 때 SQLite가 자동으로 데이터 타입을 추론하는 것
2. SQLite 타입 선호도의 목적
    - 유연한 데이터 타입 지원
        - 데이터 타입을 명시적으로 지정하지 않고도 데이터를 저장하고 조회할 수 있음
        - 칼럼에 저장되는 값의 특성을 기반으로 데이터 타입을 유추
    - 간편한 데이터 처리
        - INTEGER Type Affinity를 가진 열에 문자열 데이터를 저장해도 SQLite는 자동으로 숫자로 변환하여 처리
    - SQL 호환성
        - 다른 데이터베이스 시스템과 호환성을 유지

### 반드시 NOT NULL 제약을 사용해야 할까? NO!

1. 하지만 데이터베이스를 사용하는 프로그램에 따라 NULL을 저장할 필요가 없는 경우가 많으므로 대부분 NOT NULL을 정의
2. 값이 없다라는 표현을 테이블에 기록하는 것은 0이나 빈 문자열등을 사용하는 것으로 대체하는 것을 권장