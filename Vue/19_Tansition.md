# Transition

-   참고 링크 : [공식 문서(Transition)](https://ko.vuejs.org/guide/built-ins/transition)

## `<Transition>` 컴포넌트

1. 설명 :
    - 상태 변화에 대응하기 위해 트랜지션/애니메이션에 도움을 주는 컴포넌트
    - 빌트인 컴포넌트로 등록하지 않고 사용 가능
2. 애니메이션 조건
    - `v-if`를 통한 조건부 렌더링
    - `v-show` 를 통한 조건부 표시
    - `<component>`를 통해 전환되는 통적 컴포넌트
    - `key` 라는 특수한 속성 값 변경
3. 사용법

    ```jsx
    <template>
      <button @click="show = !show">토글</button>
      <Transition name = 'hello'>
        <p v-if="show">안녕</p>
      </Transition>
    </template>

    <script>
    <script setup>
    import { ref } from 'vue'

    const show = ref(true)
    </script>

    <style>
    .hello-enter-active,
    .v-leave-active {
      transition: opacity 0.5s ease;
    }

    .hello-enter-from,
    .hello-leave-to {
      opacity: 0;
    }
    </style>
    ```

    - `<Transition>` 의 `name` 설정에 따라 트렌지션 클래스의 접두사가 바뀐다
        - 설정 안할시 → `v`
