### 실습코드 - request / response

<미션>

- request.html과 response폴더의 response1.jsp ~ response6.jsp 복사 → html or txt파일로 변경

- **자바스크립트(Fetch API)** 또는 **jQuery의 $.ajax()함수**를 사용하여 응답페이지 호출하고 결과를 반영
    - 1번버튼: 응답데이터를 ',' 구분해서 <ul><li> 구성하고 <div>엘리먼트 내용변경
    each()함수사용!!
    - 2번버튼: <input>내의 이름 파라미터를 전달하고 응답데이터를 <div>엘리먼트 내용으로 변경
    - 3번버튼: <input>내의 이름을 전달하고 응답데이터를 <div>엘리먼트 내용변경
        
        //엥 2번 3번 똑가튼데
        
    - 4번버튼: 응답데이터 중 '책제목'만  <ol><li> 구성하고 <div>엘리먼트 내용변경
        - XML 태그 content 뽑아오는 법 ⇒ getElementsByTagName으로 배열 뽑아오고, for문 돌리면서 객체.**textContent**
    - 5번버튼: 응답데이터에서 이름,나이,직업구분해서 서로 다른 줄에 표시하고 <div>엘리먼트 내용변경
    - 6번버튼: 응답데이터중 '책제목'만 <ol><li>구성하고 빨강마크업후 <div>엘리먼트 내용변경

```jsx
* **Fetch API** 파라미터 전달 방법
	    function load2(){
       fetch(`(URL)**?username=${document.querySelector("input[name=uname]").value}**`)
       //username : 파라미터 변수
       
           .then(function(response){
                  console.log(response);
                  // console.log(response.text());
                  
                  if(!response.ok) {//응답이 정상이 아닌 경우
	                  throw new Error("에러발생!") //에러메세지 리턴-> catch문으로 throw
		              }
                  
                  return response.text();
           })
       
           .then(function(data){
                  console.log(data);
                  var content=data;
                  document.getElementById('d1').innerHTML=content;
           })
          
           .catch(function(err) {
              alert('에러메세지: '+err)
       });
    }
```

```jsx
 **jQuery의 $.ajax()함수**
 
 * 엘리먼트 생성
 var ul = $('<ul></ul>')
 
 * innerHTML과 같은 동작
 ul.append($('<li></li>').text(<li> content에 들어갈 텍스트)
 
 
 * 버튼이 클릭된 경우
 window.onload()
 -> 축약 : $(document).ready()
 -> 축약 : $(function(){ $('button:eq(idx)').click(function(){} }
 
 
 * 파라미터 전달 방법
 $.ajax({
   url:'http://10.4.2.100:8080/response/response2.jsp',
   data:{
	   username: $('#uname').val() //username : 파라미터 변수
	   //비교) username: $('input[name=uname]').val()
   },
   
   success:function(result){
	   $('div').html(result);
   },
   
   error:function(jqueryXHR, msg) {
	    alert(jqueryXHR, jqueryXHR.status, jqueryXHR.statusText)
	   	alert(msg)
	 }
});

* document.getElement와 같은 동작
	$(doc).find('title')
	
	
* createElement, appendChild와 같은 동작
	let ol = $('<ol>')
	let li = $('<li>').text(CONTENT)
	ol.append(li)
```
