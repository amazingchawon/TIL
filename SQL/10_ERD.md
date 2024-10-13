# ERD

1. 설명 :
    - Entity Relationship Diagram
    - 데이터베이스의 구조를 시각적으로 표현하는 도구
    - Entity(개체), 속성 그리고 엔티티 간의 관계를 그래픽 형태로 나타내어 시스템의 논리적 구조를 모델링하는 다이어그램
2. 구성요소
    - 엔티티 Entity : 데이터베이스에 저장되는 객체나 개념
        - 고객, 주문, 제품
    - 속성 Attribute : 엔티티의 특성이나 성질
        - 고객(이름, 주소, 전화번호)
    - 관계 Relationship : 엔티티 간의 연관성
        - 고객이 ‘주문’한 제품
3. 중요성
    - 데이터베이스 설계의 핵심 도구
    - 시각적 모델링으로 효과적인 의사소통 지원
    - 실제 시스템 개발 전 데이터 구조 최적화에 중요

## Cardinality

1. 설명 : 한 엔티티와 다른 엔티티 간의 수적 관계를 나타내는 표현
2. 주요 유형 :
    - 일대일 one-to-one
    - 다대일 many-to-one
    - 다대다 many-to-many
3. 표현 : 선의 끝부분에 표시되며 일반적으로 숫자나 기호(까마귀 발)로 표현됨

![Cardinality](https://d2slcw3kip6qmk.cloudfront.net/marketing/pages/chart/erd-symbols/ERD-Notation.PNG)