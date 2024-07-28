# HTML-Day2

날짜: 2024년 7월 2일

### 기본태그

```html
textarea Tag : 여러 줄을 입력할 수 있는 영역

<textarea> : rows / cols 속성으로 크기 지정
						 disabled="disabled" 속성 -> 비활성화
```

span ↔ div : 노션 참고

```html
공간분할태그

1. block 형식 : 사용하는 element가 한 줄을 모두 사용

2. inline 형식 : content의 크기만큼만 공간을 사용
```

### 시멘틱 태그

- 브라우저, 검색엔진, 개발자에게 컨텐츠의 의미를 설명하는 역할
- semantic 요소 : form, table, img ⇒ content의 의미를 명확히 설명
- non-semantic 요소 : div, span ⇒ content의 의미 설명 안함

---

### CSS

- 웹 페이지를 표현하기 위한 스타일 규칙을 모아 놓은 문서
- 사용 이유
    - 웹 문서의 내용과 상관없이 디자인만 바꿀 수 있음
    
- 구성 = 선택자 + 선언부
    - 선택자(***Selector***) : 규칙이 적용될 Element
    - 선언부 : 선택자에 적용될 스타일로, 속성과 값으로 이루어짐
    
    <aside>
    💡 div { } → div태그를 찾아서 적용
    
    href { } → href는 속성임! 
                ⇒ 대괄호로 묶어줘서 속성임을 명시 : ***[href] { }*** 
                     href 속성이 들어갈 수 있는 태그 중 href 속성이 있으면 적용
    
    </aside>
    

- 적용방법
    1. 내부 스타일 시트 - <style> 태그 내에서 사용 
    2. 외부 스타일 시트 적용    ← **권장!**
    3. 인라인 스타일 - 태그에서 직접 적용
    
    ```html
    1. 내부 스타일 시트 적용
    
    <head>
    	<style> 
    		css규칙
    	</style>
    </head>
    
    ------------------------------------------------------------------------------
    2. 외부 스타일 시트 적용
    
    <head>
    	<link rel="stylesheet" (type="text/css") href="css파일">
    </head>
    
    ------------------------------------------------------------------------------
    3. 인라인 스타일
    
    <h2 style="color : blue;"> 인라인 스타일 적용 </h2>
    ```
    
    겹치는 CSS 스타일에 대한 우선순위 : 3 > 1 > 2
    
    (겹치지 않는 스타일의 경우엔 해당 없음)
    

### CSS - 상속

- 부모 요소의 속성을 자식 요소에게 상속시킴 : Text 관련, Opacity, Color, list-style
- 모든 속성이 상속되는 것은 아님 : Box Model 관련(width, height, margin..)

---

### **Selector ****
1. 일반 선택자

```html
일반 선택자
<style>
  * {
		  margin: 0px;  //전체 요소에 대해 마진 없애기
  }
  
  div, h3, p {
	  스타일 규칙
  }
  
	div p {//div 하위 내에 있는 p에 대해서 스타일 적용 
	 스타일 규칙
  }
 
	div>p {//div 바로 밑 p에 대해서만 스타일 적용
		
	}
	
	p.클래스이름 {//class가 클래스이름인 p 태그에 대해서 적용
	
	}
	
	#id이름 {
	스타일 규칙
	}
  
</style>
```

- p.target1  != p .target1
    - p.target1 : class가 target1인 p 태그에 대해서 적용
    - p .target1 : p 하위에 있는 target1 클래스 태그에 대해서 적용

- <body class = “Ureca Water”> : CSS를 적용한다면 클래스Ureca 태그랑 클래스 Water 태그에 대해서 각각 적용되어야 함

1. 복합 선택자
    
    ```html
    복합 선택자
    ----------------------------------------------------------------------------------
    인접 형제 선택자 : 같은 부모 태그를 가지고, **바로 옆에 인접**한 형제 태그
    								 형제 관계인 엘리먼트가 여러 개 존재한다면 첫번째 요소만 선택
    <style>
    	element1 + element2 { //인접형제 선택자 = element1과 형제관계를 가진 ***element2에 적용***
    		스타일 규칙
    	}
    </style>
    
    일반 형제 선택자 : 같은 부모 태그를 가지고, 바로 옆에 인접하지 않아도 그냥 형제 태그면 적용됨!
    									형제 관계인 엘리먼트가 여러 개 존재한다면 모든 요소를 선택
    <style>
    	element1 ~ element2 { 
    	//인접형제 선택자 : element1 형제인 ***element2 요소들***에 적용(element1에는 적용X)
    		
    	}
    </style>
    ```
    

1. 가상 클래스 선택자
    
    ![Untitled](HTML-Day2%20300ddecb9cf44327900b70e902cbe8dc/Untitled%206.png)
    
    - li ← 자식 요소임
    - ‘ : ‘ = 자식 요소임을 나타냄 ⇒ **ul의 자식임!~!!!!!**
    - li:nth-child(5) -  <ul>의 5번째 <li>요소는 마젠타 컬러
    - li:nth-child(3n+1) - <ul>에 대한 1,4,7 ..번째 <li> 요소는 스카이블루 컬러
    - li:last-child : <ul>의 마지막 <li>자식 요소
    
    - 3번째 퀴즈 코드에 적용해보기
        
          → <tr> 직속 하위 <td>에 대해서 align center 일일이 적용하지 않고 
        
               한방에 처리될 수 있도록
        
        ```html
        <style>
        	1번 방법)
          td:first-child {
            text-align: center;
          }
            
          2번 방법)
          td:nth-child(1) {
        		text-align : center;
          }
        </style>
        ```
        

1. 속성 선택자
    
    ```html
    속성 선택자 : 특정 속성 or 속성값을 갖는 엘리먼트를 선택
    ----------------------------------------------------------------------------------
    
    <style>
    	[속성] { }
    	[속성 = V] { }    //속성값이 V와 일치하는 엘리먼트만
    	[속성 ^= V] { }   //속성값이 V로 시작하는 엘리먼트만
    	[속성 $= V] { }   //속성값이 V로 끝나는 엘리먼트만
    	p[속성 = V] { }   //p 태그 중 속성값이 V인 엘리먼트만
    	
    </style>
    ```
    

---

### CSS 속성

- Font 속성 : font-style, font-weight, font-size

- Text 속성 : text-align ( → center, left, right, justify (=좌우 맞춰서 정렬)), color, text-decoration
    - text-align 비교해보기
     
    - white-space속성 :
        
        Element {white-space : normal/pre/nowrap/pre-wrap}
        
        nowrap - 텍스트가 길어서 부모 요소 안의 가로폭을 넘어가더라도 자동 줄바꿈X
        
- **사용자 인터페이스 속성 : *display* ****
    - display속성의 설정값을 변경하여 개발자가 원하는 모양으로 변경
    
    ```html
    <style>
    	div.target1 { ***display : inline*;** } //block 형식인 div를 inline 형식으로 노출되게
    	
    	div.target1 { ***display : inline-block*;** } //inline 형식이지만 block성질도 있어서 폭/길이 변경 가능
    	
    	span.target2 { ***display : block***; }  //inline 형식인 span을 block 형식으로 노출되게
    </style>
    ```
    
    <aside>
    💡 이미지에 대해 자바스크립트로 visibility 속성을 hidden으로 설정한 것과 비교해보기
    
    ⇒ **영역은 그대로 있고** 이미지를 안보이게 CSS 변경
    
    Display 속성을 none으로 변경하는 경우 **이미지 영역 자체가 사라짐**
    
    </aside>
    
- <table> 관련 속성
    - width / height
    - table { border : } : 테이블 전체 겉 테두리에 적용됨
        
        ⇒ tr, td에도 테두리를 주고 싶다면 tr, td에 대해서 따로 CSS 규칙 만들어서 적용해야 함
        

---

### Positioning

- HTML 내 문서의 위치를 지정하거나 객체의 visibility를 다룸
- 엘리먼트의 위치를 고정시키거나 브라우저 크기에 따라 이동하는 등의 설정
- 정적인 HTML을 자바스크립트를 통해 동적으로 만들기 위한 속성

<aside>
💡 static(디폴트) : top, left에서의 거리 지정 불가 / 문서 작성 순서 그대로 ****좌측상단 배치

relative : top, left 거리 지정

absolute : 자신의 상위 BOX(부모 엘리먼트)를 기준으로 상하좌우 절대 위치 지정

  
fixed : 스크롤해도 지정된 위치에 고정되어서 둥둥 따라옴

</aside>

- z-index → relative or absolute 등의 속성으로 사용
    - 엘리먼트가 겹치는 경우에 대한 우선순위를 정할 수 있음
    
---

### 게임으로 연습하기

<Selector 연습>

https://flukeout.github.io/ - 셀렉터 게임 ㅎ,ㅎ

<Grid Layout 연습>- 행과 열을 기준으로 배치하는 방법입니다.

https://cssgridgarden.com/#ko

<Flexbox 연습>

https://flexboxfroggy.com/#ko

- Flex(플렉스)는 Flexible Box, Flexbox라고 부르기도 함
- Flex는 레이아웃 배치 전용 기능으로 고안
- float나 inline-block 등을 이용한 기존 방식보다 훨씬 강력하고 편리함
- justify-content 속성/align-items 속성/flex-direction

![justify-content 속성 중에 헷갈리는거](HTML-Day2%20300ddecb9cf44327900b70e902cbe8dc/Untitled%208.png)

justify-content 속성 중에 헷갈리는거
