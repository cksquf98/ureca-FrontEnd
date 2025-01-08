<8주차수업>

- 프로젝트 구현 및 발표
- 화면디자인(BootStrap)
- UI/UX 개념과 디자인 원리
- 디자인 프로세스에 따른 UX 컨셉 도출
- 디바이스 별 UI, 디자인 최적화

---

## 부트스트랩

- 반응형( 스마트폰 태블릿 PC 사이즈에 맞는 배치Layout)
- 모바일 우선
    
    ⇒ CSS, JS프레임워크/frontEnd프레임워크 : 틀이 정해져 있음 (미리 정의된 HTML/CSS/JS를 모아 놓은것)
    ⇒ 개발자를 위한 디자인 프레임워크 (디자인 혁명)
    ⇒ class로 대화해요
    
- 장점
    - **내부클래스**를 빠삭 파악해 놓았다면 빠르고 쉽게 다양한 형태의 웹사이트를 제작
    (copy & paste하면 된다)
    - 반응형 즉 각 디바이스별로 대응이 가능 퀄리티도 나름 뛰어나 많은 회사/기업에서 사용

- 단점
    - **미리 구축되어 있는 스타일 요소**가 많기 때문에 디자인과 기능이 정해져 있어서
    커스터 마이징이 힘든 편.
    - 미리 정해져 있는 클래스들을 확실하게 인지해야 함.
    - 페이지 로딩 속도가 타 프레임워크와 비교했을때 상당히 무겁고 느린편
    - 부트스트랩은 코드의 약 10%만사용 (**나머지 90%정도 가량 다른 코드들을 함께 로딩**) ⇒ 무거움

- Bootstrap 5를 직접 다운로드하여 호스팅하고 싶지 않으면 CDN(콘텐츠 전송 네트워크)에서 포함할 수 있음

```jsx
<!-- Latest compiled and minified CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Latest compiled JavaScript (약간 이벤트 액션가튼거 기능 제공) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

### 컨테이너

- 클래스 `.container` : 반응형 고정 너비 컨테이너 제공
- 클래스 `.container-fluid` : 전체 너비 컨테이너 제공

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    div {
        background-color: aqua;
    }
  </style>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>

<!--<div class="container-fluid"> -->
<div class="container">
  <h1>My First Bootstrap Page</h1>
  <p>This part is inside a .container class.</p>
  <p>The .container class provides a responsive fixed width container.</p>
  <p>Resize the browser window to see that the container width will change at different breakpoints.</p>
</div>

</body>
</html>
```

- 고정 컨테이너
    - 화면 크기에 따라 max-width가 변경됨
<img width="805" alt="image" src="https://github.com/user-attachments/assets/a2a006ee-148c-44b0-af6d-c43e9237584a" />

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>
  
<div class="container pt-3">
  <h1>Responsive Containers</h1>
  <p>Resize the browser window to see the effect.</p>
</div>

<div class="container-sm border">.container-sm</div>
<div class="container-md mt-3 border">.container-md</div>
<div class="container-lg mt-3 border">.container-lg</div>
<div class="container-xl mt-3 border">.container-xl</div>
<div class="container-xxl mt-3 border">.container-xxl</div>

</body>
</html>

**브라우저 창 줄이면서 max-width 확인
```

### 그리드
<img width="791" alt="image" src="https://github.com/user-attachments/assets/0027b329-d126-4003-aa3d-a778cdeb026f" />

- flexbox를 기반으로 구축되었으며 페이지 전체에 최대 **12개의 열**을 허용
    - 근데 10개 열만 쓰면 나머지 2개는 안채워지고 빈 상태가 되는군
- 열을 그룹화하여 더 넓은 열을 만들 수 있음

- 그리드 클래스
    - `.col-` (extra small devices - screen width less than 576px)
    - `.col-sm-` (small devices - screen width equal to or greater than 576px)
    - `.col-md-` (medium devices - screen width equal to or greater than 768px)
    - `.col-lg-` (large devices - screen width equal to or greater than 992px)
    - `.col-xl-` (xlarge devices - screen width equal to or greater than 1200px)
    - `.col-xxl-` (xxlarge devices - screen width equal to or greater than 1400px)
    
    <aside>
    💡 cf.
    p = padding
    
    mt = margin
    
    </aside>
    

### **More Typography Classes**

- h1 ~ h6
- display-1 ~ display-6

| `.text-decoration-none` | Removes the underline from a link |
| --- | --- |

### Text Colors Classes

`.text-success`, `.text-info`, `.text-warning`, `.text-danger` 등등

### Table

 <table class=**"table"**>

- 줄무늬 테이블 CSS

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>

<div class="container mt-3">
  <h2>Striped Rows</h2>
  <table class=**"table table-striped"**>
    <thead>
      <tr>
        <th>Email</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>john@example.com</td>
      </tr>
      <tr>
        <td>mary@example.com</td>
      </tr>
    </tbody>
  </table>
</div>

</body>
</html>

```

- Bordered Table
    - \<table class="table table-bordered">

- 마우스 오버 시 행에 Hover 효과(회색 배경 전환)
    - \<table class="table table-hover">

- 반응형 테이블 : 스크롤 추가됨
    - \<div class="table-responsive"><br/>
      \<table class="table">

### Image

- 중앙정렬 <br />
  \<img src="사진.jpg" class="mx-auto d-block" style="width:50%">

### Alerts

\<div>에 색상 넣기 + 애니메이션(`.fade` and `.show` classes)

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>

<div class="container mt-3">
  <h2>Animated Alerts</h2>
  <p>The **.fade and .show classes adds a fading effect when closing the alert message.**</p>
  <div class="alert alert-success alert-dismissible fade show">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <strong>Success!</strong> This alert box could indicate a successful or positive action.
  </div>
  <div class="alert alert-info alert-dismissible fade show">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <strong>Info!</strong> This alert box could indicate a neutral informative change or action.
  </div>
  <div class="alert alert-warning alert-dismissible fade show">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <strong>Warning!</strong> This alert box could indicate a warning that might need attention.
  </div>
  <div class="alert alert-danger alert-dismissible fade show">
    <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
    <strong>Danger!</strong> This alert box could indicate a dangerous or potentially negative action.
	</div>
</div>

</body>
</html>

** X버튼 클릭하면 효과 발생
btn-close = 닫기 버튼
```

---

자바스크립트 Dialog : alert, prompt, confirm 

얘네 대신 모달창을 보여주는게 좋음

### Modal

- 현재 페이지 위에 표시되는 대화 상자/팝업 창

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>

<div class="container mt-3">
  <h3>Modal Example</h3>
  <p>Click on the button to open the modal.</p>
  
  <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#myModal">
    Open modal
  </button>
</div>

<!-- The Modal -->
<div class="modal" id="myModal">
  <div class="modal-dialog">
    <div class="modal-content">

      <!-- Modal Header -->
      <div class="modal-header">
        <h4 class="modal-title">Modal Heading</h4>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>

      <!-- Modal body -->
      <div class="modal-body">
        Modal body..
      </div>

      <!-- Modal footer -->
      <div class="modal-footer">
        <button type="button" class="btn btn-danger" data-bs-dismiss="modal">Close</button>
      </div>

    </div>
  </div>
</div>

</body>
</html>
```
<img width="775" alt="image" src="https://github.com/user-attachments/assets/01d6a7c9-0298-41f5-ae5f-e1ac42d3f4f4" />
