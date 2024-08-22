# Bootstrap Grid system

## Grid system

1. 정의 : 웹 페이지의 레이아웃을 조정하는 데 사용되는 12개의 컬럼으로 구성된 시스템
    - 왜 12일까?
        - 약수가 많은 수
        - 적당히 큰 수
2. 목적 : 반응형 디자인을 지원해 웹 페이지를 모바일, 태블릿, 데스크탑 등 다양한 기기에서 적절하게 표시할 수 있도록 도움
3. 기본 요소
    - Container : Column들을 담고 있는 공간
    - row : 1개의 row 안에 12개의 column 영역이 구성 → 각 요소는 12개 중 몇 개를 차지할 것 인지 결정
    - Column : 실제 컨텐츠를 포함하는 부분
    - Gutter : 컬럼과 컬럼 사이의 여백 영역
4. Grid system 실습
    - 기본
    - 중첩 Nesting
        
        ```html
          <h2 class="text-center">Nesting</h2>
          <div class="container">
            <div class="row">
              <div class="box col-4">col-4</div>
              <div class="box col-4">
                <div class="row">
                  <div class="box col-6">col-6</div>
                  <div class="box col-6">col-6</div>
                  <div class="box col-6">col-6</div>
                  <div class="box col-6">col-6</div>
                </div>
              </div>
            </div>
          </div>
        ```
        
    - 상쇄 Offset
        
        ```html
        <h2 class="text-center">Offset</h2>
          <div class="container">
            <div class="row">
              <div class="box col-4">col-4</div>
              <div class="box col-4 offset-4">col-4 offset-4</div>
            </div>
            <div class="row">
              <div class="boxcol-3 offset-3">col-3 offset-3</div>
              <div class="boxcol-3 offset-3">col-3 offset-3</div>
            </div>
            <div class="row">
              <div class="box col-6 offset-3">col-6 offset-3</div>
            </div>
          </div>
        ```
        
    - Gutter : Grid system에서 column 사이에 여백 영역
        - x축 : padding으로 여백 생성
            - padding : 테두리와 컨텐츠 사이의 여백 → 사이즈가 절대적으로 줄어드는 것이 X
            - margin으로 안하는 이유 : row가 이미 결정되어 있기 때문, margin 값이 생기면 튀어나가는 컬럼이 생김
        - y축 : margin으로 여백 생성
            - 높이는 row의 너비와 상관 없기 때문에 margin으로 적용
        - 주의 : Gutter의 값은 row가 컨트롤 하는 것!
        
        ```html
        <h2 class="text-center">Gutters(gx-0)</h2>
        <div class="container">
        <div class="row gx-0">
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
        </div>
        </div>
        
        <h2 class="text-center">Gutters(gy-5)</h2>
        <div class="container">
        <div class="row gy-5">
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
        </div>
        </div>
        
        <h2 class="text-center">Gutters(g-5)</h2>
        <div class="container">
        <div class="row g-5">
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
          <div class="col-6">
            <div class="box">col</div>
          </div>
        </div>
        </div>
        ```