# Lifecycle Hooks

1. 설명 : Vue 컴포넌트의 생성부터 소멸까지 각 단계에서 실행되는 함수
2. Lifecycle Hooks Diagram
    
    ![Lifecycle Hooks Diagram](https://vuejs.org/assets/lifecycle.MuZLBFAS.png)
    
    - 컴포넌트의 생애 주기 중간 중간에 함수를 제공
    - 개발자는 컴포넌트의 특정 시점에 원하는 로직을 실행할 수 있음
3. 사용 :
    - 구조분해할당
    - 변수에 담지 않음
4. 예시 :
    - Mounting : Vue 컴포넌트 인스턴스가 초기 렌더링 및 DOM 요소 생성이 완료된 후 특정 로직을 수행하기
        
        ```jsx
        const { createApp, ref, onMounted } = Vue
        
        onMounted(() => {
          console.log('mounted')
        })
        ```
        
    - Updating: 반응형 데이터의 변경으로 컴포넌트의 DOM이 업데이트된 후 특정 로직을 수행하기
        
        ```jsx
        const { createApp, ref, onMounted, onUpdated } = Vue
        onUpdated(() => {
          console.log('updated')
        })
        ```
        
5. 주요 Lifecycle Hooks:
    - 생성 단계 / 마운트 단계 / 업데이트 단계 / 소멸 단계 등 다양한 단계 존재
    - 가장 일반적으로 사용 되는 Lifecycle Hooks
        - onMounted, onUpdated, onUnmounted
6. 주의 :
    - Lifecycle Hooks는 동기적으로 작성
        - 비동기적으로 작성하면 코드 실행 X
            - setTimeout 같은거 사용하면 X
    - Vue 내부 매커니즘과의 동기화
        - Vue의 내부 로직은 컴포넌트의 라이프사이클에 맞춰 최적화되어 있음

# Vue Style Guide

1. Vue의 스타일 가이드 규칙 → 우선순위에 따라 4가지 범주로 나뉨
2. 필수 Essential :
    - 오류를 방지하는 데 도움이 되므로 어떤 경우에도 규칙을 학습하고 준수
    - 예시
        - v-for에 key 작성
        - 동일 요소에 v-if와 v-for 함께 사용하지 않기
3. 적극 권장 Strongly Recommend :
    - 가독성 및 개발자의 경험을 향샹시킴
    - 규칙을 어겨도 코드는 여전히 실행
        - 정당한 사유가 있어야 규칙을 위반할 수 있음
4. 권장 Recommended : 
    - 일관성을 보장하도록 임의의 선택을 할 수 있음
5. 주의 필요 Use with caution :
    - 잠재적 위험 특성을 고려함