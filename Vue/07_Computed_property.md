# Computed Property

## `Computed()`

1. 설명 :
    - 계산된 속성을 정의하는 함수
    - 여러 번 계산하지 않고, 미리 계산된 속성을 사용하여 템플릿에서 표현식을 단순하게 하고 불필요한 반복 연산을 줄임
2. 필요한 경우 :
    - 예시 : 할 일이 남았는지 여부에 따라 메시지를 출력하기
        
        ```jsx
        // computed
        const restOfTodos = computed(() => {
          return todos.value.length > 0 ? '아직 남았다.' : '퇴근!'
        })
        ```
        
3. 특징 :
    - 반환되는 값은 computed ref이며 일반 refs와 유사하게 계산된 결과를 `.value` 로 참조 할 수 있음 (템플릿에서는 `.value` 생략 가능)
    - computed 속성은 의존된 반응형 데이터를 **자동으로 추적**
    - 의존하는 데이터가 **변경될 때만 재평가**
        - restOfTools의 계산은 todos에 의존하고 있음
        - 따라서 todos가 변경될 때만 restOfTools가 업데이트 됨
    - 의존된 반응형 데이터 기반으로 캐시cache됨
        - 즉, 의존된 반응형 데이터가 변경되지 않는 한 이미 계산된 결과에 대한 여러 참조는 다시 평가할 필요 없이 이전에 계산된 결과를 즉시 반환
4. 주의사항
    - computed는 읽기 전용 → computed 값을 변경하려는 시도는 X
        - 변경할 시, Write operation failed: computed value is readonly 라는 경고 메세지
    - 원본 배열 변경하지 말 것
        - return numbers.reverse() → X
        - return […numbers].revers() → O

### 캐시 Cache

1. 설명 :
    - 데이터나 결과를 일시적으로 저장해두는 임시 저장소
    - 이후에 같은 데이터나 결과를 다시 계산하지 않고 빠르게 접근할 수 있도록 함

### computed vs method

1. 설명 :
    - computed 속성 대신 method로도 동일한 기능을 정의할 수 있음
2. 예시 :
    
    ```jsx
    const getRestOfTodos = function () {
    	return todos.value.length > 0 ? '아직 남았다' : '퇴근!'
    }
    ```
    
3. 차이 :
    
    | computed | method |
    | --- | --- |
    | - 의존된 데이터가 변경되면서 자동으로 업데이트 | - 호출해야만 실행됨<br>- 다시 렌더링이 발생할 때마다 항상 함수를 실행 |
    | - 의존하는 데이터에 따라 결과가 바뀌는 계산된 속성을 만들 때 유용<br>- 동일한 의존성을 가진 여러 곳에서 사용할 때 계산 결과를 캐싱하여 중복 계산 방지 | - 단순히 특정 동작을 수행하는 함수를 정의할 때 사용<br>   - 데이터에 의존하는지 여부와 관계 없이 항상 동일한 결과를 반환하는 함수 |
    | → 무조건 computed만 사용하는 것이 아니라 사용 목적과 상황에 맞게 computed와 method를 적절히 조합하여 사용 |