### 자바스크립트

- HTML문서내에서 동적페이지(DynamicWebPage)를 구성하는 용도
- 용도
    - 문서조작
    - 유효성 검사

### AJAX

- 변경할 데이터를 외부에서 가져오는 통신 방법
    - 외부문서 : 서버에 존재하는 문서
- AJAX는 자바스크립트다!
- 비동기 서버 통신
- ajax의 요청페이지는 웹브라우저에 보여지는 전체페이지가 아니고
요청HTML의 특정한 부분을 변경할 '부분 HTML'이다
    
    ⇒ **페이지의 일부를 변경하기 위해 사용 *****
    
- `XMLHttpRequest`라는 객체를 통해 서버에게 필요한 데이터를 요청할 수 있음 ★★

<aside>
💡 <클라이언트>                                                   <서버>
- JavaScript                                                        - JSP,Servlet(자바)
- 데이터요청                                                      - 데이터 준비, 데이터 출력 out.print(데이터)

```
            1.open(URL요청과 get방식의 데이터)
           ---------------------------->

            2.send(post방식의 데이터)
           ---------------------------->

            3.responseText 또는 responseXML
          <----------------------------
            var v1= xhr.responseText;
            var v2= xhr.responseXML;

            v1의 자료형? String
            v2의 자료형? Document

```

※ 위의 1,2,3번을 실행하기 위해서는 XMLHttpRequest(xhr)객체가 필요!!
※ 3번(서버데이터 받기)은 콜백함수에 정의!! - xhr을 통한 URL요청 이벤트 발생 후 실행

</aside>

### XMLHttpRequest(서버통신객체)

- 속성/메서드

```jsx
const xhr = new XMLHttpRequest();

* **xhr.readyState**
	 0 (아무상태아님)
	
	 -------요청-------
	 1 (open메소드호출 - URL요청)
	 2 (send메소드호출 - 파라미터전달)
	 
	 -------응답-------
	 3 (요청한 데이터를 일부분 받는 중)
	 4 (요청한 데이터 전체를 받았을때)

* **xhr.onreadystatechange** = 실행할 콜백함수

* **xhr.status** => 서버의 상태
	200 : 정상
	403 : 요청 권한 없음	
	404 : 요청 페이지 없음
	500 : 서버 내부 에러

* **xhr.open('HTTP요청방식','요청URL')**
	HTTP요청방식 - GET/POST/PUT/DELETE

* **xhr.send('요청URL에게 전달할 데이터')**
```

### 실습코드

- 강사님 톰캣 서버 'http://10.4.2.100:8080/res.jsp'에 있는 텍스트 가져와서 출력

** CORS 에러 → 클라쪽에서 플러그인 설치 or 서버쪽에서 설정 변경(cors filter)

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AJAX테스트</title>
</head>
<body>
    <h3>Ajax test</h3>
    <hr>
    <div></div>
    <button onclick="hello()">Click</button>
</body>
<script>
    //비동기 통신을 통해 서버에서 문자열 받아오기
    const xhr = new XMLHttpRequest();
    
    //이벤트 발생 시 호출
    function hello() {
        xhr.open('GET','http://10.4.2.100:8080/res.jsp') //CORS 에러 **
        
        xhr.onreadystatechange = changeDiv
        
        xhr.send(null)
    }

		//콜백함수
    function changeDiv() {
        // alert(xhr.readyState) //1,2,3,4 상태 출력

        if(xhr.readyState == 4 && xhr.status == 200) {//(요청한 데이터 전체를 받은 상태) && (서버정상)
        //응답 데이터 얻어오기
        const str = xhr.responseText

        //문서 조작
        const div = document.querySelector('div') //div자료형: Element
        div.innerHTML = str
        }
    }
</script>
</html>
```

- 현재 폴더에 있는 텍스트 파일에서 읽어와서 출력
    - open( )의 URL부분만 변경하면 됨

```jsx
xhr.open('GET','user.txt')
```

- 다른 폴더에 있는 텍스트 파일에서 읽어와서 출력

```jsx
xhr.open('GET','/img/ajax.txt')
```

- 미션

```jsx
// <미션> '희망메뉴' 버튼 클릭하면 서버에 메뉴 데이터를 요청하여 
// 현 페이지 div에 순서없는 목록으로 출력
// ==> 서버 데이터 페이지 : food.jsp

-내가한거
<body>
	<ul></ul> --> 여기에 ul 추가해두고
</body>
<script>
const xhr = new XMLHttpRequest();
    function menu() {
        xhr.open('GET', 'http://10.4.2.100:8080/food.jsp')
        xhr.onreadystatechange = changeDiv2
        xhr.send(null)
    }

    function changeDiv2() {
        if(xhr.readyState == 4 && xhr.status == 200) {
            const ul = document.querySelector('ul')
            //console.log(xhr.responseText.trim().split(',')) 
            //xhr.responseText.trim().split(',') ==> string[] 배열~~!

            for (const str of xhr.responseText.trim().split(',')) {
                var liNode = document.createElement('li')
                liNode.innerHTML = str
                ul.appendChild(liNode)
            }
        }
    }
```

```jsx
강사님 코드
   function menuChoice(){
        xhr.open("GET","http://10.4.2.100:8080/food.jsp");
        xhr.onreadystatechange = function(){
          if(xhr.readyState==4 && xhr.status==200){
            //   const str = xhr.responseText; //str자료형: string
              const strArr = xhr.responseText.trim().split(','); //strArr자료형: string[]

              let foods='<ul>';
              strArr.forEach(function(menu){
                   foods+="<li>"+menu+"</li>"
              });
              foods+='</ul>';
              

              const div = document.querySelector("div"); //div자료형: Element
              div.innerHTML=foods;
          }            
        };
        xhr.send(null);
      }//menuChoice
```

---

## 교재 - ch10 AJAX

### AJAX의 이해

- AJAX
    - 웹페이지 콘텐츠 일부 또는 전체 영역(html 전체가 아니고 브라우저 내 영역)을 갱신하는 방법
    - 비동기 처리로 성능 향상
    - CSR
        
        ↔ SSR : 페이지 전체(html 헤드 바디 전체) 리로드 & 변경
        
    - 데이터 컨테이너 : 데이터 저장방식 ⇒ XML, JSON
        - XML : 태그 형식 → <age>20</age>,  <person age = “20”></person>
            
                      ( age : 변수명, 20 : 데이터값 )
            
        - JSON : { key : value }

- 비동기 프로세스
    
    ![Untitled](https://github.com/user-attachments/assets/f2702e4e-bb74-4144-8156-d9eeea55b3e9)

    - 동기 통신 : 통신을 요청한 쪽에서 응답이 올 때까지 블락킹 (URL이 직접 나오면서 변경) - 계주 이어달리기
    - 비동기 통신 : 통신을 요청한 쪽에서 응답이 올 때까지 블락킹 X - 실행 차단없이 여러 요청을 동시에 처리 가능
    
- **`XMLHttpRequest(XHR)`** 객체를 사용
    
    <aside>
    💡 1. XMLHttpRequest(XHR) 객체 생성 및 초기화
    2. XMLHttpRequest(XHR) 객체 open
    3. XMLHttpRequest(XHR) 객체를 서버로 send하여 요청과정 수행
    4. 서버로 전달된 요청에 대한 응답을 처리할 콜백함수 정의
    
    </aside>
    
    - XMLHttpRequest API
   ![Untitled (1)](https://github.com/user-attachments/assets/78c2b81d-3acd-4356-bf83-f2e0da34a674)
    onload == (onreadyState= 4)
    
- responseXML : Document로 저장됨
- responseText : String으로 저장됨

### XML

- 데이터 저장하는 형식
    - 데이터 : 시작태그와 끝태그 사이에 정의
- Well-Formed XML문서 : XML Spec에 맞게 작성된 문서
- Validate 문서 : Well-Formed XML문서 + 추가적인 제한사항, 형식(DTD, XMLSchema)
    - DTD : XML문서내에 출현할 태그와 속성에 대한 정의
    (태그종류, 태그순서, 태그 포함관계, 태그내의 속성)
    
    ```html
    - 엘리먼트(태그) 선언
    <!ELEMENT 태그명 (태그내의 내용)>
    
    <!ELEMENT tag (a,b,c)> : 자식 태그간의 순서 표현
            ---> <tag>
                    <a/>
                    <b/>
                    <c/>
                 </tag>
    
    <!ELEMENT tag (a|b|c)+> : 선택을 표현. 순서 없는 관계
    
     - 속성선언
     <!ATTLIST 태그명         속성명          속성자료형           기본선언>
                                             --------           ---------
                                               CDATA            #REQUIRED : 필수요소
                                               ID               #IMPLIED : 선택요소
                                         (a|b) : 열거데이터      #(FIXED)? 데이터 : 고정데이터
    ```
    
    ```html
    DTD
    <?xml version='1.0' encoding='UTF-8'?>
    <!ELEMENT root(compare)+> : 루트 다음에 compare 엘리먼트가 1개 이상 와야함
    <!ELEMENT compare(#PCDATA)> #PCDATA: XML Parser(해석기)가 해석하는 문자 데이터
    ```
    

---

### XML 문법

💡 최소 한개 이상의 element를 가져야 함 - 사용자 정의 태그도 ㄱㅊ

💡 문서 전체를 감싸는 한 개의 Root Element가 있어야 함

```jsx
   test.xml  ===>   <A/>
                    <B/>
                    ===> (X)
                         
   test.xml ===>    <ROOT>    
                       		<A/>
                       		<B/>
                    </ROOT>
                    ===> (O) 
```

💡 시작태그가 있다면 반드시 끝 태그가 존재해야 함

- html은 끝 태그 없어도 봐줌

```html
<root>
<br>
</root>
===> (X)

<root>
<br/>
</root>
===> (O)
```

💡 태그 대소문자 구분

💡 엘리먼트의 포함관계가 꼬여서는 안됨

```html
    <?xml version='1.0' encoding='UTF-8'?>
    <a>
       <b></b>
       <c>
    </a>               
       </c>
    ---> (X)  a,c엘리먼트 누가 부모이고 자식인지 알 수 없음 : 에러!!
    
    cf.루트 엘리먼트 앞 공백은 상관없음
    cf. <?xml version='1.0' encoding='UTF-8'?> <?xml>앞에 공백 있으면 에러남
    cf. 텍스트끼리는 자식관계 성립X
```

💡 태그 content에 들어갈 수 없는 문자 :  '&'  '<'   ']]>’

⇒ CDATA 섹션 사용 : <![CDATA[ >, **<, &, …** 특수문자 ]]>

💡 속성값은 반드시 인용부호 사용!!!

💡 서로 다른 속성은 반드시 공백으로 구분

<table border='1'cellpadding="5">   (X) 속성명앞에 공백없음

💡 주석문에 '--’(대시 2개) 사용불가

---

### XML vs. JSON

- Ajax-XML
    - https://github.com/kwakmoonki/booksr/blob/main/ch10.AJAX/10.ajax-xml.html

```html
xml파일

<?xml version="1.0" encoding="UTF-8"?>
<lists>
  <list>서울<happy/><good/> </list> 
	  : 첫째자식 서울 텍스트, 둘째자식 happy 엘리먼트, 셋째자식 good 엘리먼트, 넷째자식 공백
  <list>대전 대구</list> : 텍스트끼리는 자식관계 없으니까 just 대전 대구 텍스트
</lists>

---------------------------------------------------------------------------

html파일

<script>
if (this.readyState == 4 && this.status == 200) {
   const doc = ***this.responseXML*** //=> doc의 data type = Document     
   const lists = doc.querySelectorAll('선택자');
   
   
   // 뽑아온 배열 데이터 처리
   for (let list of lists) {
	   ~~// let title = list.childNodes[0].nodeValue;~~ 
	   == list.textContent 한줄로 출력 가능
	   
	   listGroup += '<li class="list-group-item">' + title + '</li>'
	  }
	  
	  document.쿼리셀렉.innerHTML = '<ul class="list-group">' + listGroup + '</ul>'
</script>
```

- Ajax-json
    - https://github.com/kwakmoonki/booksr/blob/main/ch10.AJAX/10.ajax-json.html
    - **responseText**를 통해 가져온 String을 Object로 바꿔야함 ***by JSON.parse***
    - JSON 메서드 (형식에 일단 적합해야 변환 가능)
        - ***parse :** text to **Object***
        - ***stringify :** object to **Text***

```html
<script>
if (this.status == 200) {
	const json = **JSON.parse(this.responseText)** //=> json의 data type = Object
****	const lists = json.lists //=> lists data type =  array
	
	//오브젝트 데이터 처리
	let listGroup = [];

	lists.forEach(function(item) {
		listGroup.push('<li class="list-group-item">' + item.list + '</li>');
	}
}

let newUI = '<ul class="list-group">' + listGroup.join('') + '</ul>';
document.querySelector('#pocket').innerHTML = newUI;
</script>
```

---

### Ajax응용

- rss
    - 자주 업데이트되는 사이트에서 정보를 제공하기 위한 서비스로, XML형태로 정보를 제공하며 이때 사용되는 xml문서를 말한다
    - 예시 : 기상청 rss
        
        https://www.kma.go.kr/weather/forecast/mid-term-rss3.jsp?stnId=109
        

- CORS
    - 특정 서버에서 실행중인 웹 애플리케이션이 다른 서버의 자원에 접근
    할 수 있는 권한을 부여하도록 브라우저에 알려주는 제약사항
    

---

### Fetch API

- XMLHttpRequest 객체 보다 기능이 향상된 대체제
- Promise 객체 사용 ⇒ then 메서드, catch 메서드 사용

```html
**fetch( URL, 옵션 ).then().then().catch()**
```

```html
 <script>
	 document.querySelector('.btn-primary').onclick = function() {
      fetch('10.ajax-xml.xml')
        .then(response => ***response.text()***) //response는 전체 왕창 다 긁어와서 text로 변환해줘야함 **
                // == .then function (response) { return response.text() }
        .then(result => {//result = 앞 then의 리턴값
          console.log('[성공]', result);
        })
        .catch(error => {
          console.error('[실패]', error);
        });
    }
 
 
 
    document.querySelector('.btn-primary').onclick = function() {
      fetch('10.ajax-json.json')
        .then(response => ***response.json()***)
        .then(result => {
          console.log('[성공]', result);
        })
        .catch(error => {
          console.error('[실패]', error);
        });
    }
  </script>
```

### jQuery의 Ajax함수

```
$.ajax({ key:value  });

<ajax함수에 사용되는 key>
url:            //요청url
type:          //HTTP요청 방식 서술. ('GET','POST')
data:          //서버에 전달할 파라미터 데이터의 의미.
dataType:     //서버로부터 전달받는(출력한) 데이터의 자료형을 명시. (xml, json, script, or html)
success: 콜백함수  ( Anything data, String textStatus, jqXHR jqXHR )
error:콜백함수  ( jqXHR jqXHR, String textStatus, String errorThrown )

==> 가장많이 사용되는 key : url, data, success콜백
```
