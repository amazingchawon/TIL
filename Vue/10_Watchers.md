# Watchers

## `watch()`

1. 설명 : 하나 이상의 반응형 데이터를 감시하고, 감시하는 데이터가 변경되면 콜백 함수를 호출
2. 구조 :
    
    ```html
    watch(source, (newValue, oldValue) => {
    	//do something
    }
    ```
    
    - 첫번째 인자 `source` :
        - watch가 감시하는 대상(반응형 변수, 값을 반환하는 함수 등)
    - 두번째 인자 `callback function` :
        - source가 변경될 때 호출되는 콜백 함수
        - new value : 감시하는 대상이 변화된 값
        - old value (optional) : 감시하는 대상의 기존 값
3. 사용 :
    - 구조분해할당
    - 따로 변수에 담을 필요 X
4. 예시 :
    - 감시하는 변수에 변화가 생겼을 때 연관 데이터 업데이트 하기 → 메세지 입력 시 길이 변화
        
        ```jsx
        const count = ref(0)
        const message = ref('')
        const messageLength = ref(0)
        
        watch(message, (newValue) => {
          messageLength.value = newValue.length
        })
        ```
        

### computed vs. watch

|  | Computed | Watchers |
| --- | --- | --- |
| 공통점 | 데이터의 변화를 감지하고 처리, 의존하는 원본 데이터를 직접 변경하지 않음 |  |
| 동작 | 의존하는 데이터 속성의 계산된 값을 반환 (리턴 값 존재) | 특정 데이터의 속성 변화를 감시하고 작업을 수행 (side-effects) |
| 사용목적 | 계산된 값을 캐싱하여 재사용, 중복 계산 방지 | 데이터 변화에 따른 특정 작업을 수행 |
| 사용 예시 | 연산된 길이, 필터링 된 목록 계산 등 | DOM 변경, 다른 비동기 작업 수행, 외부 API와 연동 등 |