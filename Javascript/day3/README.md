### ch6 요소의 선택과 이벤트

- 요소 선택 메서드 : getElement & getElement**s** & Selector
![Untitled](https://github.com/user-attachments/assets/cf70e0f9-7e76-48a0-a20d-9fa02eea266c)

- getElementById
    - 유일한 값인 id로 하나의 요소 선택
    
    ```jsx
    <head>
    //1번
    <script> => script가 먼저 실행되어서 getElement 정상 동작 못함
        document.getElementById('two').style.backgroundColor = 'lightblue';
    </script>
    
    //2번
    <script>
        **window.onload = function() { }** //=> 페이지 모두 로드된 후 동작 => 정상 동작 O **
        document.getElementById('two').style.backgroundColor = 'lightblue';
    </script>
    
    </head>
    
    <body>
    <ul class="list-group">
          <li class="list-group-item">첫 번째</li>
    </ul>
    </body>
    ```
    
- getElementsByTagName
    - 동일한 태그 이름의 여러 요소를 한꺼번에 선택
    - 배열로 가져옴
    
- getElementsByClassName()
    - 동일 클래스 이름의 여러 요소를 모두 선택
    - 배열로 가져옴
    
    ```jsx
    <body>
    <div class="container">
      <ul class="list-group">
        <li class="list-group-item">첫 번째</li>
        <li class="list-group-item">두 번째</li>
        <li class="list-group-item active">세 번째</li> **--> **클래스이름 2개 : list-group-item, active**
        <li class="list-group-item">네 번째</li>
        <li class="list-group-item">다섯 번째</li>
      </ul>
    </div>
    
    <script>
      const lists = document.getElementsByClassName('list-group-item');
      const active = document.getElementsByClassName('active');
    
      console.log(lists, active);
      console.log(lists.length, active.length);
    
      let i = 0;
      if (lists.length > 0) {
        while (i < lists.length) {
          lists[i].innerText = i + ' 번째';
          i++;
        }
      }
    
      active[active.length - 1].innerHTML = '<em>선택한 목록</em>';
    </script>
    ```
    

### For문

- 기본For문은 동일
- For/in문
    - 모든 열거할 수 있는 프로퍼티(enumerable properties)를 순회할 수 있도록 해주는 반복문
    - in으로 객체의 **key**, 배열의 **index**를 뽑음
    
    ```jsx
    for(변수 in 객체){
       객체의 모든 열거할 수 있는 프로퍼티의 개수만큼 반복적으로 실행할 문장
    }
    
    var ob ={ name: "홍길동", age:16};
    
    for(var i in ob){
       document.write(i + "<br>"); //Object의 key값 - "name", "age"
       document.write(arr[i] + "<br>"); //Object의 value값 - "홍길동", 16
    }
    ```
    

- For/of문
    - 반복할 수 있는 객체(iterable objects) 데이터를 순회할 수 있도록 해주는 반복문
    - 반복할 수 있는 객체 ⇒ Array, Map, Set, arguments 객체 등
    - **Object { }는 반복할 수 없는 객체임**

```jsx
 var arr=[1,2,3,4,5]

for(var value of arr){
   document.write(value + "<br>"); // 배열값 1,2,3,4,5
}
```

- 자바스크립트 forEach메서드
    - array.forEach(function(currentValue, index, arr));
    - index, arr : optional 파라미터
    
    ```jsx
    배열명.forEach(function() { 반복동안 실행할 문장 })
    배열명.forEach(function(value) { 반복동안 실행할 문장 }) //value = 배열에 들어있는 값
    배열명.forEach(function(value,index) { 반복동안 실행할 문장 })
    
    var arr=[1,2,3,4,5]
    arr.forEach(function() { arr.length만큼 반복 }
    
    arr.forEach(function(value,index) { document.write(value + "(" + index + ")") })
    
    //객체(object)는 forEach안되넴 ㅎ.ㅎ
    ```
    

---

### ch5-3 콜백함수

- 이벤트 발생 시(=발생 후) 호출되는 함수
- 함수를 매개변수 형식으로 다른 함수에 전달하는 방식
- **함수괄호 표시XXX**

```jsx
{
      function showResult(sum) {
        var msg = '결과는 ' + sum + ' 입니다.';
        document.querySelector('input[name="sum3"]').value = msg;
      }

      function plusOperation(x, y, callback) {
        callback(x + y); //callback함수(전달된 showResult) 실행
      }

      //***매개변수로써의 함수 <- 이름만 넣어줘야함***
      plusOperation(6, 7, showResult);
      
      이 함수호출 방식랑 결국 똑가틈
    // plusOperation(6, 7, function (sum) {
    //     var msg = '결과는 ' + sum + ' 입니다.';
    //     document.querySelector('input[name="sum3"]').value = msg;
    //   });
      
    != plusOperation(6, 7, showResult()); //showResult함수가 먼저 실행 -> plus함수 실행
}
```

---

### 이벤트

- onchange
    
    ```jsx
    <select class="form-select" id="subject" name="subject">
              <option selected>선택하세요...</option>
              <option value="html">HTML</option>
              <option value="css">CSS</option>
              <option value="javascript">자바스크립트</option>
    </select>
    
    <script>
    	document.querySelector('select[name="subject"]').onchange = function() { 
    		console.log(this); // this = 이벤트를 발생시킨 애 = document.querySelector(select[name="subject"]') = <select class="form-select" id="subject" name="subject"> 
    		console.log(this.value) //= 선택된 option의 value
    	}
    </script>
    
    (cf. 화살표함수의 경우엔 this 안씀)
    ```
    
- onload
    - body.onload : body가 모두 수행된 후 실행되도록 함 ← 비권장
    - window.onload : 전체 페이지 로딩 완료된 후 실행되도록 함

- onkeydown
    - 키를 누를 때 발생
    
    ```jsx
    getElement('#keyword').addEventListener('keydown', function(evt) {
    	//function(evt) : 콜백함수 - 이벤트 발생 후 호출 **
    	//evt : 이벤트 발생된 객체
    	console.log(evt);
    	console.log(evt.keyCode); //아스키코드 - 'A'가 눌린 경우 == 65, Enter == 13
    }
    
    == getElement('#keyword').onkeydown = function() { }
    == <input type="type이름" onkeydown = "function이름()">
    ```
    

### 타이밍이벤트

- 정해진 시간이 지나고 난 후 설정된 코드가 실행됨
- setTimeout(콜백함수, 밀리초 단위 시간)
    - 정해진 시간이 지나면 한 번 실행
    
    ```jsx
    <script>
      setTimeout(function () {
        document.querySelector('button.btn').click();
      }, 1500);
      
      
      == function A() {} 함수A 정의
      setTimeout(A, 1500)
      
    </script>
    ```
    

- setInterval(콜백함수, 밀리초 단위 시간)
    - 정해진 시간 간격마다 계속 실행
    
    ```jsx
    <style>
        #circle {
    	    border-radius: 50%;
          position: absolute;
        }
    </style>
    
    <script>
      let move = 0;
      document.querySelector('#circle').onclick = function() {
        setInterval(function() {
          move += 20;
          document.querySelector('#circle').style.left = move + 'px';
        }, 250);
      }
    </script>
    ```
    

---

### DOM(Document / Object Model)

- Document : HTML, XML 텍스트 파일
- Object : 메모리
- Model : 데이터

- Tree-based API
    - Parsing 후에 Memory상에 문서전체의 Tree구조를 만들어 사용
    - 문서의 구조가 충실히 보존됨

- XML문서 트리 - 사용자 정의 태그 사용 ㅇㅇ
    - 맨 첨 시작태그 = 루트
    - 하위 태그 = 자식
    - white space도 자식 노드로 처리됨 (html과의 차이점 - html은 무시)
    ![Untitled](https://github.com/user-attachments/assets/957bed1e-fa42-4f5c-9d09-2031d8c66f7f)


- DOM 대표 API
    1. **Node**
        
        -모든 객체의 공통적인 특성을 모아놓은 추상화된 객체
        
        -Object 클래스 느낌 > 최상위 부모
      
        <aside>
        💡 **Node자체의 정보를 get하거나 set하는 method**
        
           - getNodeName/getNodeValue ..
        
        **Node를 조작하는 method** 
        
        -  부모노드.appendChild (Node newChild) : 부모 태그 하위에 추가
        
        -  부모노드.removeChild (Node oldChild) : 메모리에서 삭제
        
        …
        
        **Related Node를 얻는 method**
        - 부모노드.getChildNodes() : 자식 ***NodeList***를 반환
        - 노드.getNextSibling() or getPreviousSibling() : 다음 or 이전 형제노드 반환
        
        - 자식노드.getParentNode()
        
        ++ 부모 노드를 불러오는 속성 → .parentNode
        
        </aside>
        
    2. **Document**
        
        -XML문서의 루트 바로 위에 위치
        
        -문서 전체의 루트 객체
        
        <aside>
        💡 **Document에 있는 element를 조건에 따라 얻는 method**
          - getElementById
          - getElementsByTagName
        
        **Node 생성 관련 method**
          - createAttribute (String name)
          - createCDATASection (String data) : XML은 부등호 문자를 무조건 시작 태그로
             보기 때문에 부등호 수식 표시하려고 사용
          - ***createElement (String tagName)***
          - ***createTextNode (String data) =** 태그 ****내 ****content 역할*
        
        </aside>
        
    3. **Element**
        
        -XML 문서의 기본단위
        
        <aside>
        💡 Attribute와 관련된 method
        
          - setAttribute (String name, String value) : 기존 태그에 속성 추가
        
        </aside>
        

- 추가 인터페이스

*** *NodeList* : 노드 속성까지는 저장 안됨

*** *NamedNodeMap* : 노드 속성까지 저장 됨

```jsx
<공통 메서드>
Int **getLength**()
Node **item** (int index)
```

---

### 실습코드

[mission_re.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/adc68080-688f-4d0d-8380-43331fd5e42f/mission_re.html)
