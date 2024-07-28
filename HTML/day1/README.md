# HTML-Day1

<4주차수업>
-HTML Markup
-CSS3 기본 구조 및 CSS 활용
-javascript 기본 문법, javascript 함수와 객체, 콜백 함수
-요소 선택, 이벤트
-DOM 요소 접근, BOM

---

## VScode 설치

- 추가작업 : Code로 열기 작업 추가 체크, Code로 불가 작업을 ~~ 메뉴에 추가 체크
- Live Server필수 확장 프로그램 install **
    로컬 서버 실행해서 동작시켜줌
    코드 저장과 동시에 동기화되어 바로 반영됨
  
<aside>
💡 ! 입력 시 html 기본 틀 자동 완성

</aside>

<aside>
💡 alt + shift + 화살표 : 줄 복붙

</aside>

<aside>
💡 Emmet 맛보기

’>’ : 자식 요소임을 명시
1. ul> li*4   ⇒ ul 하위에 li 4개 만들어짐
2. ol>li

</aside>

---

## HTML(HyperText Markup Language)

- 마크업 언어로써, 웹문서를 작성하며 태그를 통해 문서의 구조를 기술하는 언어

- 프로그램 코드는 크게 head - body로 나눠서 생각할 수 있음
    - head: 선언 부분 - 클래스, 패키지 등등
        
        ```html
        <!DOCTYPE html> : 현재 문서가 HTML문서임을 의미
        <html>
        <head>  : 브라우저에게 HTML문서의 머리 부분임을 인식
        <meta>  : 문서에 대한 정보를 담는 태그 - charset, name, content, http-equiv ..
        <title> : 웹페이지 탭 타이틀 설정
        <style> : 브라우저에게 CSS 영역임을 인식
        ```
        
    - body: 실행 부분
        
        ```html
        <body>
        <span> : inline 방식으로 텍스트 길이만큼 영역 잡힘
        <div> : block 한 줄이 다 영역으로 잡힘
        <p> : (div와 다르게)다른 블럭과의 영역을 좀 띄워서 블럭을 잡음
        <script> : 브라우저에게 자바스크립트 영역임을 인식
        ```
        
    - html에서는 head-body를 **태그**로 구분!

- ## Tag
    - 표기법 : <태그이름> </태그이름>
    - 종류
        1. 시작 태그 : <table>
        2. 끝 태그 : </table>
        3. 빈 태그 : <br>
    
    - 기본 태그
        
        ```html
        목록 Tag - 각 항목을 들여쓰기로 표현하며 하나 이상의 하위 태그를 포함
        
        	**<ul> : 번호 없이 항목 앞에 심볼을 붙여 목록 표시 (Unordered List) ***
        	<ol> : 번호 있는 목록 표시 (Ordered List) ***
        	<li> : <ul>이나 <ol> 하위에 사용되는 항목**
        	<dl> : 데이터에 대한 목록 타이틀 표현
        	<dt> : 데이터 목록에 대한 항목
        	<dd> : 하위 항목들
        
        --------------------------------------------------------------------------------
        
        <!--목록태그에 타입 지정하면 하위 항목 심볼에 전체 적용됨-->
        <ul type="square/circle/disc"> 
                <li style="list-style-type: circle;">메뉴1</li>
                <li>메뉴2</li>
                <li>메뉴3</li>
                <li>메뉴4</li>
        </ul>
        
        <ol type="1/A/a/I/i">
                <li>야구</li>
                <li>축구</li>
                <li>농구</li>
                <li>배구</li>
        </ol>
        ```
 
       ### 테이블 Tag 
        ```html
        테이블 Tag - 데이터를 행과 열로 표현
        					  각 행 그룹은 최소 하나 이상의 <tr>을 가져야 함
        						width 명시 안하면 안에 들어가는 글자 기준으로 길이 잡힘  
        - tr: 테이블 행
        - td: 테이블 행에 들어갈 데이터 (열에 해당)
        
        -------------------------------------------------------------------------------
        
        <table> : 브라우저에게 테이블임을 명시 - border / cellspacing / cellpadding **
        	<caption> : 테이블 타이틀 
        	<colgroup> : 열 그룹
        		<col span(col에 의해 묶여진 열의 개수) / style(배경색) / width(컬럼 폭 길이)>
        		
        <thead> : 테이블 컬럼 이름 - <tr> <th>
        
        <tbody> : 실제 셀 데이터 - <tr> <td>
        	<tr>
        		<td> - colspan : 두 개 이상의 **열**을 하나로 합침
        					 rowspan : 두 개 이상의 **행**을 하나로 합침
        
        <tfoot>
        
        -------------------------------------------------------------------------------
        
        ***** <td rowspan = "합칠 행의 수">** : 다음 행에 해당하는 <td>는 쓰지 않아야함
        
        ***** <td colspan = "합칠 열의 수">** : 다음 열에 해당하는 <td>는 쓰지 않아야함
        ```
 
 ### 이미지 Tag
        ```html
        이미지 Tag
        
        <img src ="PATH" height = "세로" width = "가로" 
        	   title = "마우스 오버 시 노출될 텍스트" alt = "대체텍스트">
        
        src -> ../ (부모 디렉토리)
        			 ./  (현재 디렉토리)
        ```
        
        ```html
        ***** 하이퍼링크 Tag 속성 : href** - 다른 문서로 이동할 수 있음
        
        <a href> : <a> 태그 내에서 href 속성을 통해 이동할 문서 지정, 
        								          target 속성을 통해 현재 창에서 띄울지 새 창 띄울지 지정
        
        <a href="URL or HTML파일">노출 텍스트</a>
        
        <a href="URL or HTML파일" target = "_self / _blank">노출 텍스트</a>
        	target: **_self** = 현재 창에서 이동
        				  **_blank** = 새로운 탭에서 이동
        
        * 자바스크립트 함수를 실행하고 싶다면
        <a href="javascript:함수이름()">노출 텍스트</a>
        
        * 같은 페이지 안에서 특정 태그 위치로 이동시키고 싶다면
        <태그 id="id이름">특정 아이디로 선언된 태그 존재</태그>
        <a href="#id이름">노출 텍스트</a>
        
        * CSS파일 적용하고 싶다면 - head부분에
        <link rel = "stylesheet" href = "Filename.css">
        ```
        
### Form Tag
      ```html
        *** form Tag - 사용자로부터 데이터를 입력받아 서버에서 처리하기 위한 용도 (for SSR)
        					 
        		form에 데이터를 입력한 후 서버로 Submit 
        		-> 서버는 요청을 분석한 후 데이터를 등록하거나 조회하여 결과를 반환 (for CSR)
        		
        -------------------------------------------------------------------------------
        
        <form> : 사용자가 입력하는 요소들은 모두 form태그 하위에 위치해야 함
        	<button>
        	<input> : type 속성에 따라 여러 형태로 표시됨 
        					  (type = text, password, search, submit, reset, email, checkbox ...) 
        						form 태그 내부에 있어야만 submit, reset 버튼이 원하는 역할을 함!!
        						
        						readonly / disabled 속성
        						
        						value = "미리 입력되는 값"
        						
        						size = "사이즈" 속성
        						
        	<textarea>
        	<select> - <option> : selected / multiple 속성
        	<fieldset>
        
        -------------------------------------------------------------------------------
        
        * form 속성 : 어떤 방식으로 서버로 전달할 것인지
        <form 속성 = "속성값"> </form>
        	1. method : **GET** = **주소 표시줄에 사용자의 입력 내용이 표시됨, 서버로부터 html을 받아옴**
        							**POST** = **form안에 입력데이터를 서버에 전송** 
        	2. name : form 태그 구분 목적
        	3. action : form 태그 안의 내용들을 처리해줄 서버상의 프로그램 지정
        	4. target
        	5. enctype="multipart/form-data" : 서버에 해당 파일을 업로드시켜줌, post 메서드와 쓰임
        	
        	
        ```
        
    - Tag와 Element의 차이점
        - Tag : 태그 그 자체
        - Element : 시작 태그 + Content + 끝 태그 전체 객체 (자식 요소 포함)
    
    - Tag와 속성
        
        - Attribute : Tag의 속성을 의미
        
        - 글로벌 속성: 어느 Tag에나 넣어서 사용할 수 있음
            
            → class, id와 같이 태그를 구분시켜주는 속성
            
               Class 속성 : 여러 Tag에 공통적인 특성(CSS)을 부여시켜줌.    ← ***중복 가능***
            
               Id 속성 : 문서 내에서 Tag를 유일하게 식별시켜줌.                 ← ***중복 불가***
            
        
    
    - 특수문자 - content부분에서 사용 (&nbsp;,  &lt;,  &gt;, ..)
    - 포맷팅 : 노션 참고
    - pre태그 **

- HTML5 특징
    - 별도의 플러그인 없이도 멀티미디어 요소 재생 가능
    - 서버와 클라이언트 사이 소켓 통신 가능
    - Semantic Tag : 특정 Tag에 의미를 부여함 → 검색 엔진의 빠른 검색을 가능하게 함

- 문서 구조
    - 시작 종료 태그가 있고 그 태그 사이에 문서 내용을 정의
    - 웹 브라우저는 태그의 의미에 따라 문서를 화면에 표시
    
- 작동원리 : 노션 참고


- 웹 문서 구성요소
    1. HTML : 웹페이지 문서 담당 (= 구조)
    2. CSS : 디자인 담당 (= 표현)
    3. JavaScript : 이벤트 담당 (= 동작)

- 실습 퀴즈
    - 1번
        목록형요소Quiz1.html

      
    - 2번
        테이블Quiz.html
    
    - 3번 - 테이블을 감싸는 form 태그 **
        
                  form태그로 감싸야지만 reset(초기값으로 노출), submit 버튼이 제대로 동작함

        라디오 버튼 : name 속성을 동일하게 묶어야 둘중하나만 선택 가능
 
      <select><option value="val">텍스트</option></select>
        체크박스 : name 속성으로 동일하게 묶어버리면 자바스크립트에서 배열 for문 처리해서 편하댕
        실제로 페이지상 보이는 텍스트는 하얀글씨
        * value 속성의 데이터는 서버로 보내지는 데이터임 (자바스크립트와 서버스크립트가 실제로 처리하는 데이터)](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/bb4997d5-41b4-41f2-b0da-02cb41907066/Untitled.png)
 
        <input type="radio" name="gender" value="남" checked="checked">남
        checked << 변수처럼 사용
        select 태그의 경우에선 checked대신 selected 사용
    

---

<aside>
💡 ((알아두면 좋은 용어))

DTD

- Document Type Definition을 나타내는 용어로 XML 파일이 어떤 구조와 어떤  element, 어떤 attributes를 가지는지 정의하는 것을 말한다.

- xml 

: 사용자가 정의한 태그를 사용함

   데이터 저장 방식 → 데이터 교환 목적

SSR : Server Side Rendering

 - ASP. PHP, …

 - 서버쪽에서 데이터를 반영해서 새로 페이지 노출시키는 케이스

CSR : Client Side Rendering

- 자바스크립트를 통해 메모리상에서 페이지 이동없이 안에 태그만 수정되도록

- 데이터를 반영해서 엘리먼트 요소에 데이터를 변경시키는 케이스 (페이지 이동 or 로드 XX)

</aside>

** Margin vs. Padding 노션 참고

-Padding : 요소 내부에서의 공간

-Margin: 요소 바깥 객체과의 공간

**cellpadding ↔ cellspacing



<aside>
💡 style 태그 내에서 .클래스이름 { … } ⇒ 해당 클래스 이름을 가진 태그에 모두 적용됨

<style>
.className { background-color : blue; }
</style>

</aside>
