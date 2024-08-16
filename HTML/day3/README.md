### DOM (Document / Object Model)

- Document = Text (HTML, XML)
- Object Model = 메모리에 올라가는 데이터

### 자바스크립트

- 자료형을 명시하지 않는다! ⇒ 변수 타입 선언 X (let, const, var로 선언) / 할당 Only
- 동적 타이핑 언어(ex. JS, Python)
    - 값이 할당될 때 자료형이 결정되는 언어
- HTML 문서 내에서 실행되는 프로그램 ⇒ HTML에 종속적
- HTML문서 = 정적페이지
    
      →  자바 스크립트 적용 ⇒ 동적페이지
    
- 자바스크립트로 데이터의 유효성을 검사 ⇒ 서버의 부하를 줄여줄 수 있음

```html
자바스크립트 변수 출력 테스트
---------------------------------------------------------------------------------------

<body>
    <h3>자바스크립트 테스트</h3>
    <script>
        //프로그램 영역
        var v;
        v = '유레카';
        v = 1000;
        v = new Date();
        
        //변수 데이터 확인 
        //by 브라우저 출력 : document.write 많이 쓰이진 않음
        document.write("<font color = blue> v는 " + v + "</font>");

        //by 메세지창 : 마크업 적용 X
        // alert("v는 " + v)

        //by 콘솔 : 제일 많이 사용됨 -> F12 개발자창 콘솔에 뜸
        console.log("console 출력 >> v는 " + v);
        console.log("console 출력 >> v는 ",v,"입니다"); //콤마 == 띄어쓰기
        console.log("변수 v의 자료형은",typeof(v));
        console.dir(v); //Date에 대한 생성자, 메서드들을 보여줌
        console.log('현재시간 :',v.getHours(),'시',v.getMinutes,'분');
    </script>
</body>
```

### 자바스크립트의 차이점

- 실행 위치
    - 자바 스크립트 : 클라이언트 스크립트 ⇒ 브라우저에서 실행
    - 자바 : 서버 스크립트 ⇒ JVM에서 실행
- 자바스크립트는 HTML문서 내 어느 위치에서든지 정의 가능
    - 보통 body 태그 끝나고 이후에 정의 : body까지 일단 딱 실행하고 나서 스크립트 수행하는게 이상적인 순서다 그런 느낌

```html
 <html>
      <head>
         <script>
            프로그램영역!!
            ----> 변수선언, 함수(function)정의                       
         </script>
      </head>
------------------------------------------------------------
      <body>
         <script>
              프로그램영역!!
              ----> 함수 호출
         </script>
      </body>
   </html> 
```

- **자바스크립트는 자료형을 선언 & 정의하지 않는다 *****

- 문자열 비교

```
자바case)
"java".equals("JAVA")   ==> (O) 문자열 내용 비교
"java" == "JAVA"        ==> (O) 메모리 주소 비교

자바스크립트case)
"javascript".equals("JAVASCRIPT") ==> (X) 에러발생
"javascript" == "JAVASCRIPT"      ==> (O) 문자열 내용 비교
```

- 문자열(text) 표현
    - 작은 따옴표와 큰 따옴표 구분 없이 사용!
    
    ```
    name1= "홍길동";  (O)
    name2= '홍길동';  (O)
    name3= "김주원';  (X)
    name4= `길라임`;  (O)
    
    `` (=백틱) 쓰는 이유 -> 표현식 사용 가능
    `${변수}문자열` <<한번에 쓰기 가능 : print(f"{변수}문자열"} 파이썬 이 느낌이넴
    ```
    
    ※ 백틱( ` ` ) - 템플릿 리터럴, 템플릿 문자열, ES6(ECMA 2015)에서 추가
    1. 여러줄 표현 (multi-line지원)
    2. ${ }표현식을 사용할 수 있음
    => 문자열 내에 변수를 포함하여 '+'를 사용하지 않아도 됨
    
    3. 백틱 안에서 enter가 \n과 같음
    
- 세미콜론 생략 가능 : 줄바꿈을 통해서도 문장을 구분하기 때문에
    
    ```
    name='길동'
    age=13
    ==> (O)
    
    name="라임" age=15
    ==> (X)
    
    name="주원"; age=17
    ==> (O)
    ```
    
- /(몫), %(나머지)
    - 자바스크립트는 integer, float 따로 구분하지 않고 number로 관리하기 때문에 / 연산 시 몫이 나오는게 아니라 진짜 나누기 연산으로 나옴
    - 정수 몫 구하려면 parseInt 사용
    
    ```
    10/5  ---> 2
    
    10%5  ---> 0
    
    10/3  ---> 3.33333333
              parseInt(3.33333333)  ---> 3
    10%3  ---> 1
    ```
    
- ‘===’  '!==’ 연산자

```html
== != : 등가 연산자 - 데이터 내용만 비교 (자동 형변환 발생)

=== !== : 엄격한 비교 연산자 - 자료형 비교 + 내용 비교

     예)  
        100 == '100'    --->true  내용만 같다면 자동형변환 해서 비교
        100 === '100'   --->false  1.자료형비교 (자료형이 다르면 false) 
                                   2.자료형 같았을때 내용비교
```

- 조건문

```html
 if(조건식1){
          조건식1의 결과가 참일 때 실행할 문장;
 }
 else if(조건식2){
          조건식1의 결과가 거짓이고!!
          조건식2의 결과가 참일 때 실행할 문장;
 }
 else {
          조건식1,2의 결과가 거짓일 때 실행할 문장;
 }
   
   
   ---------------------------------------------------------------------------------
     **※ 차이점**
     if(조건식 ==> boolean, **숫자**, **객체**){ }

     -숫자: **0인 수(false)**, 0아닌 수(true)로 구분
     -객체: 브라우저에서 지원되는 객체인지 아닌지 판별. (ex. ActiveXObject)
		        --> 변수에 값이 존재하는지 판단.
        
				   -존재하는 객체(문자열, 브라우저가 지원하는 객체)  ---> true
				   -**null, undefined, "", NaN**                      ---> false
				         -----------
		             = 초기화 되지 않은 변수
```

- 빈 문자열 체크
    - str.length : length 속성을 통해서 체크 가능!
    - str == ‘’  or str == ””로 체크

- 함수

```jsx
* 함수 자료형 & 파라미터 자료형 정의 X
* 호출하기 전까지 대기 상태로 존재
* 외부 자바스크립트 파일에 함수 정의해서 호출해도 됨
	 -> 외부파일에 있는 함수를 불러올 때엔 <script src="경로">를 통해 불러와야 함
		  <script src="경로">는 레퍼런스 전용으로, 다른 메서드를 동시에 정의하면 안됨

function 함수명() { }  
	==> **function도 Object(일급 객체)이기 때문에 클래스처럼 변수에 할당할 수 있음
		  function 안에 또 다른 메서드 정의할 수 있음 (내부 클래스처럼)**

------------------------------------------------------------------------------------

****function hello(name, age){
  if(age == undefined){ //age파라미터에 **암것도 전달 안된 경우** 체크
      처리코드
  }
        ==>호출:  hello('길동',13);
        ==>호출:  hello("길동");
        ==>호출:  hello();
   if(name) {
   
------------------------------------------------------------------------------------
        
※주의: 자바스크립트의 함수는 오버로딩을 제공할까요?
		제공되지 않는다! **동일 이름의 함수 중 제일 아래에 있는 함수**로 **오버라이딩**됨** 에러도 안남**
```

```jsx
함수 호출 테스트.html

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Function 호출 테스트</title>
    
    //불러올 때에는 이렇게 딱 쓰고 끝!
    <script src="../JS/my.js"></script>
    
    <!-- <script src="../JS/my.js">  
    src로 불러오는 경우 하단에 다른 메서드를 정의하면 무시되기 때문에--> 
    <script>
        
        function hello() {
            alert('안녀엉');
        }

        function hello() {
            alert('하이루');
        }

        function hello(name){
        if(name)
          alert('봉쥬르,'+name);
        else
          alert('봉쥬르');
       }
    </script>
</head>
<body>
    <h3>함수 호출 테스트</h3>
    <script>
        // hello();
        // hello("이름");
        // good();
    </script>

    <button onclick="var v=100;hello()">hello함수 호출 버튼</button>
    <input type="button" value="good함수 호출 버튼" onclick="good()">
</body>
```

- HTML태그 내의 속성 중 on접두사 속성은 이벤트 속성임

```jsx
예) <input type='button' value='버튼' onclick="자바스크립트 코드">
                                     ===> 주로 함수 호출
                                     ===> 버튼을 클릭했을때 동작
                                     
     onfocus : 포커스가 들어왔을때                                              
     onblur : 포커스를 잃었을때
     onchange : select태그에서 선택을 바꾸었을때
     onkeydown : 키보드를 눌렀을때
     onkeyup   : 키보드 땠을때
     onmouseover   : 마우스가 진입했을때, 엘리먼트위에 올려졌을때 
     onmouseout    : 마우스가 나갔을때
     onmousedown   : 마우스를 클릭했을때
```

### 자바스크립트 - 배열

- 자바스크립트 Array = 자바 ArrayList
    - 정해진 범위를 넘어서는 데이터도 입력 가능 =가변길이배열
- 한 변수명에 여러 자료형의 데이터를 담을 수 있음(비권장)
- 데이터 집합을 표현할 때 {}를 사용하는게 아니라 []를 사용
    - { } = JSON

- 배열 생성

```jsx
- 변수명 = new Array(size);
- 변수명 = new Array(데이터1, 데이터2, ..)
- 변수명 = [데이터1, 데이터2, ..]

※ 변수선언에 자료형 뿐만 아니라 []를 명시하지 않음.
var String []myArry = new Array(5);   ===> (X)
var        []myArry = new Array(5);   ===> (X)

※ [] 사용
var su = {1,2,3,4};  ===> 에러발생!!
var su = [1,2,3,4,'안녕',new Date( )];   ===> (O)

※{}표기는 JSON 표현임!!
{ key : value }
```

### 데이터 GET/SET

![image_720.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/76b17f50-46ec-4aab-8d96-d19f07f2678d/image_720.png)

- 데이터 얻어오기 : 변수명 = inputName.value
- 데이터 수정하기 : inputName.value = 데이터
    
    

```jsx
   <body>
        <form name='frm'>
             이름: <input type=text name=username><br>
             나이: <input type=text name=userage>
        </form>        
   </body>
```

- form 안의 데이터 얻어오기 : var uname = document.frm.username.value
- form 안의 데이터 설정하기 : document.frm.username.value='홍길동’
    
    (cf) window.onload = function( ) {  } → body html이 실행된 이후에 함수가 실행되도록 해줌
    

### 실습

[계산기.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/30da4856-5fb9-4441-bb22-08ee691a75e0/%EA%B3%84%EC%82%B0%EA%B8%B0.html)

[계산기_브라우저출력.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/bb96602c-389d-4339-ab12-7844ae78daa4/%EA%B3%84%EC%82%B0%EA%B8%B0_%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%B6%9C%EB%A0%A5.html)

[구구단출력.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/d605cdde-5c01-4529-83e1-291e8f24da00/%EA%B5%AC%EA%B5%AC%EB%8B%A8%EC%B6%9C%EB%A0%A5.html)

[메세지창.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/4677e86c-3d2e-4664-87fc-a9b5879b0ce6/%EB%A9%94%EC%84%B8%EC%A7%80%EC%B0%BD.html)

[배열세로출력.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/a3c5e242-120e-491f-8a0d-113911ef0222/%EB%B0%B0%EC%97%B4%EC%84%B8%EB%A1%9C%EC%B6%9C%EB%A0%A5.html)

[배열출력.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/04646144-411e-4eac-b524-0d8677acd3f5/%EB%B0%B0%EC%97%B4%EC%B6%9C%EB%A0%A5.html)

[이미지변경.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/64443f5c-d590-4dd3-bbfe-4a7e8f77f633/%EC%9D%B4%EB%AF%B8%EC%A7%80%EB%B3%80%EA%B2%BD.html)

```jsx
plus_function.js외부파일

function plus(su1, su2) {
    return su1+su2
}
```

---

- isNaN 함수
    
    <aside>
    💡 숫자 문자열이면 False
    숫자여도 False
    그 외 문자열은 True 반환
    
    * parseInt 함수 → 숫자 아닌 문자열은 걍 알아서 제거함
    
    </aside>
    
- 동일 파일 내에서 Form 내의 엘리먼트 값 접근
    
    <aside>
    💡 document.Form이름.엘리먼트이름.value
    
    </aside>
    
- Type : Button vs. Submit
    - Form태그 없으면 둘이 똑같음
    
    <aside>
    💡 <Button> 디폴트 타입 = Submit
    - Submit 쓰고싶지 않으면 type="button" 명시 필요
    
    Submit : 
    
    - 얘를 포함하는 form태그가 존재하면 form action을 찾아서 해당 move path로 데이터 전송 
    
    - 태그의 name을 파라미터에 넣어서 전달하기 때문에 name 설정 필수 **
    
    - action path가 비어있으면 현재 html파일이 디폴트 path임 
    
    ⇒ 새로 로드시켜서 이동 안하는 것처럼 보임
    
    </aside>
    
- CSR vs. SSR
    - CSR : 페이지 이동 없이 태그 요소에 대해서만 변경
    - SSR : 페이지 이동하여 데이터 반영된 페이지 노출
    
    <aside>
    💡 form → action, method **
    
    method :
    ”get” : URL에 name과 파라미터 값 노출
    “post” : URL에 파라미터 값 노출되지 않도록 보안
    
    </aside>
    

- 문서 조작
    1. 엘리먼트 속성 사용하기
    
    ```jsx
    inner - content영역
    * element.innerHTML = 변경값 : **element는 반드시 엘리먼트 객체여야 함**
    
    * element.innerTEXT = 변경값 : **element는 반드시 엘리먼트 객체여야 함
    
    ->** 엘리먼트 객체 얻기
    	 Element e = document.getElementById("id")
    ```
    
    - innerHTML : 마크업 태그 적용 가능
    - innerTEXT : 마크업 태그 적용 안됨
    
    1. Document, Node 객체 사용하기
    
    ```jsx
    var divNode = document.createElement('div') //<div></div>
    var txtNode = document.createTextNode(`결과값: ${su1} + ${su2} = ${res}`)
    
    //부모노드.appendChild(자식노드)
    divNode.appendChild(txtNode) // == <div>결과값: ${su1} + ${su2} = ${res}</div>
    
    element.appendChild(divNode) // == <div id="d1"><div>결과값: ${su1} + ${su2} = ${res}</div></div>
    	(element = body)
    ```
    
- 엘리먼트에서 querySelector를 통해 데이터값 얻어오기
    
    ```
    <form name="frm">
             아이디 : <input type="text" name="id" placeholder="아이디입력"><br>
             비밀번호: <input type="password" name="pwd"><br>
    </form>       
    
    ------------------------------------------------------------------------------
    
    var pw = document.querySelector("input[name=pwd]").value 
    //input태그 내 속성 name이 pwd인 엘리먼트의 value
    //[] = 속성
    ```
    

- checkbox, radio에서 체크된 값 가져오기

```jsx
**checkbox** : 
var check = document.frm.inputName속성이름.checked //체크되어 있으면 true 아니면 false
=> 폼에 대해서만 가져올 수 있다는 한계점

**radio** : checkbox와 동일

**select - option** : option태그가 아니라 select태그에 name 속성을 지정해야함 **

**querySelector로 뽑아오는게 나은 느낌
```
