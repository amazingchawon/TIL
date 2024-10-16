# Database

## 데이터베이스란

1. 데이터베이스 : 체계적인 데이터 모음
2. 데이터 : 저장이나 처리에 효율적인 형태로 변환된 정보
3. 증가하는 데이터 사용량
    - 배달의 민족 국내 주문 건수 6억 8천만 건 (2020)
    - 구독자 2억 3840만명이 1000억 시간 넷플릭스 시청 (2023 1-6월)
    - 전세계 모든 데이터의 약 90%는 2015년 이후에 생산된 것 (IBM)
4. 데이터 센터의 성장
    - 네이버 : 제2데이터센터에 6500억 투자 (2020)
    - 카카오 : 제1데이터센터와 제2데이터센터에 1.5조 투자 (2022)
    - 전 세계 데이터 센터 시장 2022년부터 2026년까지 연평균 20% 이상 성장 예상

### 데이터  저장

1. 기존의 데이터 저장 방식
    - 파일 이용
        - 장점 : 어디에서나 쉽게 사용 가능
        - 한계 : 데이터를 구조적으로 관리하기 어려움
    - 스프레드 시트 이용
        - 장점 : 테이블의 열과 행을 사용해 데이터를 구조적으로 관리 가능
        - 한계
            - 크기 : 일반적으로 약 100만행까지만 저장 가능
            - 보안 : 단순히 파일이나 링크 소유 여부에 따른 단순한 접근 권한 기능 제공
            - 정확성 :
                - 만약, 공식적으로 “강원”의 지명이 “강언”으로 바뀌었다고 가정
                - 테이블의 모든 위치에서 해당 값을 업데이트 해야 함
                - 찾기 및 바꾸기 기능을 사용해 바꿀 수 있지만 만약 데이터가 여러 시트에 분산되어 있다면 변경에 누락이 생기거나 추가 문제가 발생할 수 있음

## 관계형 데이터베이스

1. 데이터베이스 역할 : 데이터를 저장하고 조작
    - 저장 : 구조적 저장
    - 조작 : CRUD
2. 관계형 데이터베이스 : 데이터 간에 **관계**가 있는 데이터 항목들의 모음
    - 특징 :
        - 테이블, 행, 열의 정보를 구조화하는 방식
        - **서로 관련된 데이터 포인터를 저장**하고 이에 대한 **엑세스**를 제공
3. 관계 : 여러 테이블 간에 논리적 연결
    - 관계로 할 수 있는 것 : 두 테이블을 사용하여 데이터를 다양한 형식으로 조회할 수 있음
    - 고유한 식별 값 사용 (외래 키, Foreign Key)

### 관계형 데이터 베이스 관련 키워드

1. Table (aka Relation) : 데이터가 기록되는 곳
2. Field (aka Column, Attibute) : 각 필드에는 고요한 데이터 형식(타입)이 지정됨
3. Record (aka row, Tuple) : 각 레코드에는 구체적인 데이터 값이 저장됨
4. Database(aka Schema) : 테이블의 집합
5. Primary Key (기본키, PK)
    - 각 레코드의 고유한 값
    - 관계형 데이터 베이스에서 **레코드의 식별자**로 활용
6. Foreign Key (FK)
    - 테이블의 필드 중 **다른 테이블의 레코드**를 식별할 수 있는 키
    - 다른 테이블의 기본 키를 참조 → 이유 : 고유한 식별 값을 사용해야하니까
    - 각 레코드에서 서로 다른 테이블 간의 **관계를 만드는 데** 사용

### RDBMS

1. DBMS Databae Management System
    - 데이터베이스를 관리하는 소프트웨어 프로그램
    - 데이터 저장 및 관리를 용이하게 하는 시스템
    - 데이터베이스와 사용자 간의 인터페이스 역할
    - 사용자가 데이터 구성, 업데이트, 모니터링, 백업, 복구 등을 할 수 있도록 도움
2. RDBMS Relational Database Management System
    - 관계형 데이터 베이스를 관리하는 소프트웨어 프로그램
    - RDBMS 서비스 종류
        - SQLite : 경량의 오픈 소스 데이터베이스 관리 시스템
            - 컴퓨터나 모바일 기기에 내장되어 간단하고 효율적인 데이터 저장 및 관리를 제공
        - MySQL, PostgreSQL, Oracle Database 등

>데이터베이스 정리
   1. Table : 데이터가 기록되는 곳
   2. Tavle에는 행에서 고유하게 식별가능한 기본 키라는 속성이 있으며, 외래 키를 사용하여 각 행에서 서로 다른 테이블 간의 관계를 만들 수 있음
   3. 데이터는 기본 키 또는 외래 키를 통해 결합(join) 될 수 있는 여러 테이블에 걸쳐 구조화 됨