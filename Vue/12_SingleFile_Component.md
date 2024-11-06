# Single-File Components

1. 설명 :
    - 컴포넌트의 템플릿, 로직 및 스타일을 하나의 파일로 묶어낸 특수한 파일 형식 (*.vue 파일)
    - HTML, CSS, JS를 단일 파일로 합친 것

## Components

1. 설명 : 재사용 가능한 코드 블록
2. 특징 :
    - UI를 독립적이고 재사용 가능한 일부분으로 분할하고 각 부분을 개별적으로 다를 수 있음
    - 자연스럽게 애플리케이션을 중첩된 Component의 트리 형태로 구성됨

## SFC 구성요소

1. 언어블록
    - 각 *.vue 파일은 세가지 유형의 최상위 언어블록 <template>, <script>, <style>로 구성됨
    - 언어 블록의 작성 순서는 상관 없음
        - 일반적으로 template → script → stype 순서로 작성
2. `<template>` 블록
    - 각 vue 파일은 최상위 template 블록을 하나만 포함 가능
3. `<script setup>` 블록
    - 각 vue 파일은 `<script setup>` 블럭을 하나만 포함할 수 있음
        - 일반 <script>는 제외
    - 컴포넌트의 setup() 함수로 사용되며 컴포넌트의 각 인스턴스에 대해 실행
    - 변수 및 함수는 동일한 컴포넌트의 템플릿에서 자동으로 사용 가능
4. `<style scoped>` 블록
    - vue 파일에는 여러 <style> 태그가 포함될 수 있음
    - scoped가 지정되면 CSS는 현재 컴포넌트에만 적용됨
        - 부모 컴포넌트의 스타일이 자식 컴포넌트로 유출되지 않음
        - 자식 컴포넌트의 최상위 요소는 부모 CSS와 본인 CSS 모두에게서 영향을 받음
            - 자식 컴포넌트의 최상위 요소는 부모에서 사용되기 때문
            - → 이는 부모가 레이아웃 목적으로 자식 컴포넌트 최상위 요소의 스타일을 지정할 수 있도록 의도적으로 설계된 것
5. 컴포넌트 사용하기
    - Vue SFC는 일반적인 방법으로 실행할 수 없으며 컴파일러를 통해 컴파일 된 후 빌드 되어야 함
    - Vite와 같은 공식 빌드 도구를 사용

# SFC build tool

## Vite

1. 설명 :
    - 프론트 엔드 개발 도구
    - 빠른 개발 환경을 위한 빌드 도구와 개발 서버를 제공
2. Build :
    - 프로젝트의 소스 코드를 최적화하고 번들링하여(묶어) 배포할 수 있는 형식으로 변환하는 과정
    - 개발 중 사용되는 여러 소스 파일 및 리소스(JavaScript, CSS, 이미지 등)를 최적화된 형태로 조합하여 최종 소프트웨어 제품을 생성하는 것

## Vue Project

1. 생성
    - Vue Project (Application) 생성 (Vite 기반 빌드)
        
        ```bash
        $ npm create vue@latest
        ```
        
    - 프로젝트 폴더이동
    - 패키지 설치
        
        ```bash
        $ npm install
        ```
        
        - node_modules 파일, packagelock.json 생성
    - Vue 프로젝트 서버 실행
        
        ```bash
        $ npm run dev
        ```
        

## NPM Node Package Manager

1. 설명 :
    - Node.js 패키지 관리자
2. Node.js 영향 :
    - 기존 브라우저 안에서만 동작할 수 있었던 JavaScript를 브라우저가 아닌 서버 측에서도 실행할 수 있게 함
        - 프론트엔드와 백엔드에서 동일한 언어로 개발할 수 있게 됨
    - NPM을 활용하여 수많은 오픈소스 패키지와 라이브러리를 제공하여 개발자들이 손쉽게 코드를 공유하고 재사용할 수 있게 함

### 모듈과 번들러

1. Module :
    - 설명 : 프로그램을 구성하는 독립적인 코드 블록 (.js 파일)
    - 필요성 :
        - 개발하는 애플리케이션의 크기가 커지고 복잡해지면서 파일 하나에 모든 기능을 담기가 어려워짐
        - 자연스레 파일을 여러 개로 분리하여 관리를 하게 됨 → 분리된 각 파일이 모듈
    - 한계 :
        - 애플리케이션 발전 → 모듈 개수 증가
        - 모듈 간의 의존성이 깊어짐 → 문제의 원인이 어떤 모듈 간인지 파악 어려움
        - 따라서 이 의존성 문제를 해결하기 위한 도구가 필요 → Bundler
2. Bundler :
    - 여러 모듈과 파일을 하나(혹은 여러 개)의 번들로 묶어 최적화하여 애플리케이션에서 사용할 수 있게 만들어주는 도구
    - 역할 :
        - 의존성 관리, 코드 최적화, 리소스 관리 등
        - Bundler가 하는 작업 → Bunding
        - Vite는 Rollup이라는 Bundler를 사용하며 개발자가 별도로 기타 환경설정에 신경쓰지 않도록 모두 설정해두고 있음

## Vue Project 구조

### node_modules

1. 설명 :
    - Node.js 프로젝트에서 사용되는 외부 패키지들이 저장되는 디렉토리
    - 프로젝트의 의존성 모듈을 저장하고 관리하는 공간
    - 프로젝트가 실행될 때 필요한 라이브러리와 패키지들을 포함
    - .gitignore에 작성됨

### package-lock.json

1. 설명 :
    - 패키지들의 실제 설치 버전, 의존성 관계, 하위 패키지 등을 포함하여 패키지 설치에 필요한 모든 정보를 포함
    - 패키지들의 정확한 버전을 보장하여, 여러 개발자가 협업하거나 서버 환경에서 일관성 있는 의존성을 유지하는 데 도움을 줌
    - npm install 명령을 통해 패키지를 설치할 때, 명시된 버전과 의존성을 기반으로 설치

### package.json

1. 설명 :
    - 프로젝트의 메타 정보와 의존성 패키지 목록을 포함
    - 프로젝트의 이름, 버전, 작성자, 라이센스 등과 같은 메타 정보를 정의
    - package_lock.json과 함께 프로젝트의 의존성을 관리하고, 버전 충돌 및 일관성을 유지하는 역할

### public 디렉토리

1. 설명 :
    - 주로 아래와 같은 정적 파일을 위치 시킴
        - 소스코드에서 참조되지 않는
        - 항상 같은 이름을 갖는
        - import 할 필요 없는
    - 항상 root 절대 경로를 사용하여 참조
        - public/icon.png는 소스코드에서 /icon.png로 참조할 수 있음

### src 디렉토리

1. 설명 :
    - 프로젝트의 주요 소스 코드를 포함하는 곳
    - 컴포넌트, 스타일, 라우팅, 프로젝트의 핵심 코드를 관리
2. src/assets
    - 프로젝트 내에서 사용되는 자원 (이미지, 폰트, 스타일 시트 등)을 관리
    - 컴포넌트 자체에서 참조하는 내부 파일을 저장하는데 사용
    - 컴포넌트가 아닌 곳에서는 public 디렉토리에 위치한 파일을 사용
3. src/components
    - Vue 컴포넌트들을 작성하는 곳
4. src/App.vue
    - Vue 앱의 최상단 Root 컴포넌트
    - 다른 하위 컴포넌트들을 포함
    - 애플리케이션 전체의 레이아웃과 공통적인 요소를 정의
5. src/main.js
    - Vue 인스턴스를 생성하고 애플리케이션을 초기화하는 역할
    - 필요한 라이브러리를 import 하고 전역 설정을 수행

### index.html

1. 설명 :
    - Vue 앱의 기본 HTML 파일
    - 앱의 진입점 (entry point)
    - Root 컴포넌트인 App.vue가 해당 페이지에 마운드(mount) 됨
        - Vue앱이 SPA인 이유
    - 필요한 스타일시트, 스크립트 등의 외부 리소스를 로드할 수 있음 (ex. bootstrap CDN)

### 기타 설정 파일

1. jsconfig.jsom 
    - 컴파일 옵션, 모듈 시스템 등 설정
2. vite.config.js
    - Vite 프로젝트 설정 파일
    - 플로그인, 빌드 옵션, 개발 서버 설정 등

## Vue Component 활용

1. 사전 준비
    - App.vue 제외, 초기에 생성된 모든 컴포넌트 삭제
    - App.vue 코드 초기화
2. 단계 :
    - 컴포넌트 파일 생성
        - 주의 : 모든 컴포넌트에는 최상단 HTML 요소가 작성되는 것이 권장
            - 가독성, 스타일링, 명확한 컴포넌트 구조를 위해 각 컴포넌트에는 최상단 HTML 요소를 작성해야 함 (Single Root Element)
            
            ```html
            <!-- bad -->
            <template>
            	<h2>Heading</h2>
            	<p>Paragraph</p>
            	<p>Paragraph</p>
            </template>
            
            <!-- good -->
            <template>
            	<div>
            		<h2>Heading</h2>
            		<p>Paragraph</p>
            		<p>Paragraph</p>
            	</div.>
            </template>
            ```
            
    - 컴포넌트 등록 (import)
        - App 컴포넌트에 만든 컴포넌트 등록
        
        ```html
        <template>
          <h1>App.vue</h1>
          <MyComponent />
        </template>
        
        <script setup>
        // @ - src를 뜻하는 약어
        import MyComponent from '@/components/MyComponent.vue';
        </script>
        ```
        
3. `@` : `src/`를 뜻하는 약어

### Component 이름 지정 스타일 가이드

1. Component files
    - 각각의 개별 파일
    - PascalCase 아니면 kebab-case
2. Base component names
    - 앞에 Base, App, v 중 하나 붙이기
3. 긴밀하게 결합된 컴포넌트
    - 부모 컴포넌트 명시
        - 부모 컴포넌트 이름 다 쓰기
4. 컴포넌트 이름에서 단어 순서
    - 보편적인 것부터 앞에 쓰기

# Vitual DOM

1. 설명 :
    - 가상의 DOM을 메모리에 저장하고 실제 DOM과 동기화하는 프로그래밍 개념
    - 실제 DOM과의 변경 사항 비교를 통해 변경된 부분만 실제 DOM에 저장하는 방식
    - 웹 애플리케이션의 성능을 향상 시키기 위한 Vue의 내부 렌더링 기술
2. 내부 렌더링 과정
    
    https://vuejs.org/assets/render-pipeline.CwxnH_lZ.png
    
    - 템플릿
    - 렌더 함수 코드
    - 컴포넌트 반응형 상태
    - 가상 DOM 트리
    - 실제 DOM
3. Virtual DOM 패턴의 장점
    - 효율성
        - 실제 DOM 조작을 최소화, 변경된 부분만 업데이트하여 성능을 향상
    - 반응성
        - 데이터의 변경을 감지하고, Virtual DOM을 효율적으로 갱신하여 UI를 자동으로 업데이트
    - 추상화
        - 개발자는 실제 DOM 조작을 Vue에게 맡기고 컴포넌트와 템플릿을 활용하는 추상화된 프로그래밍 방식으로 원하는 UI 구조를 구성하고 관리할 수 있음
4. 주의 사항
    - 실제 DOM에 직접 접근 X → Vue의 ref()와 Lifecycle Hooks 함수를 사용해 간접적으로 접근/조작
        - 직접 DOM 엘리멘트에 접근해야 하는 경우 : ref 속성을 사용하여 특정 DOM 엘리먼트에 직접적인 참조를 얻을 수 있음
            
            ```html
            <template>
              <input ref='input'>
            </template>
            
            <script setup>
            import { ref, onMounted } from 'vue'
            
            // 변수명은 템플릿 ref 값과 일치해야 함
            const input = ref(null)
            
            onMount(() => {
            	console.log(input.value) // <input>
            })
            </script>
            ```