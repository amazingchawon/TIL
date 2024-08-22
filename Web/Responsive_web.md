# Responsive web

1. 반응형 웹 디자인 Responsive Web Design : 디바이스 종류나 화면 크기에 상관없이, 어디서든 일관된 레이아웃 및 사용자 경험을 제공하는 디자인 기술

## breakpoints

1. 정의 : 웹 페이지를 다양한 화면 크기에서 적절하게 배치하기 위한 분기점
2. bootstrap에서 반응형 웹 디자인 : 12개의 column과 6개의 breakpoints를 사용하여 반응형 웹 디자인을 구현
    - 각 breakpoints 마다 설정된 최대 너비 값 **이상으로** 화면이 커지면 grid system이 변경됨
    
    |  | **xs** <576px | **sm** ≥ 576px | **md** ≥768px | **lg** ≥992px | **xl** ≥1200px | **xxl** ≥1400px |
    | --- | --- | --- | --- | --- | --- | --- |
    | container (max-width) | None (auto) | 540px | 720px | 960px | 1140px | 1320px |
    | class prefix | `.col-`  | `.col-sm-`  | `.col-md-` | `.col-lg-` | `.col-xl` | `.col-xxl`  |
3. 실습
    
    ```html
    <h2 class="text-center">Breakpoints</h2>
    <div class="container">
      <div class="row">
        <div class="box col-12 col-sm-6">
          col
        </div>
        <div class="box col-12 col-sm-6">
          col
        </div>
        <div class="box col-12 col-sm-6">
          col
        </div>
        <div class="box col-12 col-sm-6">
          col
        </div>
      </div>
    ```
    
4. Media Query로 작성된 Grid system의 breakpoints
    - Bootstrap을 사용하지 않고 breakpoints를 구현하기 위한 CSS 코드 작성 방법