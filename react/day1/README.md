## Node.js란?

Node.js는 브라우저 밖에서도 자바스크립트를 실행할 수 있는 환경을 의미합니다.

Node.js가 나오기 전까지는 자바스크립트가 브라우저의 동작을 제어하는데 사용되었고 브라우저에서만 실행할 수 있었지만

이제는 Node.js로 자바스크립트를 **브라우저 없이도 실행**할 수 있게 되었다.

## NPM이란?

NPM(Node Package Manager)은 Node.js를 설치하면 자동으로 설치가 된다.

NPM은 명령어로 자바스크립트 라이브러리를 설치하고 관리할 수 있는 패키지 매니저로,

전 세계 자바스크립트 개발자들이 모두 자바스크립트 라이브러리를 공개된 저장소에 올려놓고

npm 명령어로 편하게 다운로드 가능하다.

### Node.js 설치

- 브라우저 없이 cmd 콘솔에서 자바스크립트 실행 가능
<img width="537" alt="image" src="https://github.com/user-attachments/assets/9fb5845d-82eb-478a-84c0-90cf7b619a0c" />


React

- **npx create-react-app my-app ******
    - 해당 디렉토리 위치에 my-app이라는 앱(프로젝트)을 만들어줌
- my-app 디렉토리 >
    - package.json이 있는 위치에서 npm start를 해야 함
    - src 안에 실행하고자 하는 자바스크립트 파일이 있어야 함

---

## 자바스크립트

### 데이터 타입

- Undefined : 변수가 할당되지 않았거나 값을 알 수 없는 경우
- Null : 값이 없음을 명시적으로 정의

```jsx
function f1(uname) {
}

hello('gildong')
hello()  // -> 이 경우 uname = undefined가 들어가서 호출됨

* 따라서 함수 내에서 uname에 대한 유효성 검사 필요
function f1(uname) {
	if(!uname) { }
}
```

### ★ 함수

- 함수 선언문
- 함수 표현식
- 화살표 함수
    - function 키워드 X
    - 바로 리턴값만 명시할 수 있으면 중괄호 X
    - 보통 한 줄의 명령문으로 표현
- 콜백함수
    - 이벤트(ex. Ajax) 발생 시 호출되는 함수
    - 콜백함수가 나중에 호출됨 → 비동기 처리

### 반복문

- 기본 for문
- for-of : 파이썬 for-in과 동일
- for-in : index를 가져와서 반복
- while

---

## React - Import, Export

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/export

### JavaScript모듈(module)

JavaScript에서 개발 애플리케이션의 크기가 커지면 파일을 여러 개로 분리해야 하는데

이때 분리된 파일 각각을 모듈(module)이라고 합니다.

→ 여러개로 분리 시 업무 분담 가능 + 재사용 가능

→ 파일 == 모듈

### 기존 자바스크립트 파일과 차이점

- 엄격한 모드(use strict) 실행됩니다.
    
    => 예를 들어 선언되지 않은 변수는 할당 시 에러가 발생합니다.
    
     su=16; //에러
    
- 모듈은 독립적인 스코프를 갖습니다.
- file프로토콜에서는 사용을 못하고 http 또는 https에서 실행됩니다.
- 모듈은 파일이고 .js가 모듈이라고 보면 되는데 기본 자바스크립트파일과 다른점은 export와 import를 사용하는 것입니다.

### 모듈 사용법

- export ==> module1_export.js
- import ==> module1_import.js
    
    : 즉 자바스크립트 간에는 export한 속성과 함수를 import하여 사용한다
    

- HTML파일 내 type = “module”
    
    test1.html
    
    : html페이지에서 module을 호출할때는 type="module"을 명시한다.
    

- module1_import.js
    
    ```jsx
    1. import {사용할 값 나열} from "PATH"
    import { title, add } from './module1_export.js'
    
    console.log(add(10,20))
    console.log(title)
    
    -----------------------------------------------------------
    2. export된 Object를 import할 경우
    //import 객체 from "PATH"
    import obj from './module2_export.js'
    
    console.log(obj.add(10,20))
    console.log(obj.title)
    ```
    
- module1_export.js
    
    ```jsx
    1. 다른 파일에서 쓸 수 있도록 할 객체에 export 키워드 붙이기
    
    const title = 'ureca'
    export function add(su1, su2) {
        return su1+su2
    }
    
    2. 한번에 export
    const title = 'ureca'
    function add(su1, su2) {
        return su1+su2
    }
    
    export { title, add }
    
    3. Object로 export
    const title = 'ureca'
    function add(su1, su2) {
        return su1+su2
    }
    
    export default { 
    //default: 모듈 내에서 한번만 사용 가능
    //JSON형식
    //속성에 변수명, 값에 변수 값이 들어가서 전달됨
        title, add 
    }
    
    -------- Or --------
    
    export default { 
        title : "Hungry",
        add(su1, su2) {
            return su1+su2
        }// add : function(su1,su2) { return su1 + su2 } 메서드 축약 버전!
    }
    
    ```
    

- test1.html
    
    ```jsx
    <body>
        <h3>모듈테스트</h3>
        <script type="module"
            src="module1_import.js"></script>
    </body>
    ```
    

---

### 자바스크립트 - JSON

- 속성-값 쌍 데이터 형식
- XML 사용 시 복잡한 파싱을 해결
- 종료 태그가 없고 구문이 짧음
- 처리속도가 XML보다 빠름 : XML은 문서의 DOM을 이용하여 접근하는데 JSON은 문자열로 전송받은 후 바로 파싱

<aside>
💡 Data : 쉼표로 나열

객체 : { }

배열 : [ ]

</aside>

- 사용법
    - JSON.stringify( JSON객체 ) : 인자로 전달받은 자바스크립트 객체를 문자열로 변환
        
        ⇒ 문서에 JSON형식의 텍스트로 적혀있어도 실제로 전달 시 json이 아니라 텍스트로 전달되기 때문에 변환이 필요함!!
        
    - JSON.parse( JSON형식의 문자열 ) : 인자로 전달받은 문자열을 자바스크립트 객체로 변환

```jsx
1. XML 형식 데이터 가져오기
<body>
    <h2>사용자 정보 (XML)</h2>
    <p id="userid"></p>
    <p id="username"></p>
</body>
<script>
    let xmlTxt = `
                <user>
                    <userid>ureca</userid>
                    <username>유레카</username>
                </user>
                `;    
    let parser = new DOMParser();
    let xmlDoc = parser.parseFromString(xmlTxt, "text/xml")
    
    console.log(xmlDoc);
    let userid = xmlDoc.getElementsByTagName("userid")[0].childNodes[0].nodeValue
    let username = xmlDoc.getElementsByTagName("username")[0].childNodes[0].nodeValue

    document.querySelector('#userid').innerHTML =  `id : ${userid}`
    document.querySelector('#username').innerHTML =  `id : ${username}`
    
</script>

2. JSON 형식 데이터 가져오기

```

- WebStorage : LocalStorage, SessionStorage
    
    키와 값을 하나의 세트로 저장되고, 도메인과 브라우저별로 저장
    
    값은 **문자열**로 저장됨
    
    - 공통 메소드
    
    | setItem(k,v) | key-value 쌍으로 저장
    (v = 문자열이어야 함) |
    | --- | --- |
    | getItem(k) | key에 해당하는 데이터 읽기 |
    | removeItem(k) | key에 해당하는 데이터 삭제 |
    | length | 저장된 k-v쌍의 개수 |
    | clear( ) | 모든 데이터 삭제 |
    
    - LocalStorage : 저장한 데이터를 지우지 않는 이상 영구적으로 보관 가능(세션이 끊겨도 사용 가능) / 도메인이 같으면 전역적으로 공유 가능
    - SessionStorage : 브라우저 종료 시 데이터 삭제 / 동일한 세션에서만 사용 가능

```jsx
//LocalStorage
```

- 개발자도구 > Application > Storage에서 스토리지 저장값 확인

```jsx
코드 추가하기
```

---

### 실습코드

<lotto 미션>

- 번호 뽑기 버튼 클릭 시 6개의 중복되지 않는 난수를 발생.
- 화면에 오름차순으로 출력
- 1초에 한개씩 숫자 뽑기
    <img width="641" alt="image" src="https://github.com/user-attachments/assets/b3f285aa-65c8-4b8d-b656-4eab01073668" />

```jsx
//강사님 코드
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Lotto</title>
    <style>
      h1 {
        text-align: center;
      }
      #btn {
        display: block;
        margin: 0 auto 20px;
        padding: 5px 10px;
        background-color: plum;
        color: blueviolet;
        border: none;
        border-radius: 5px;
        font-weight: bold;
        font-size: 19px;
      }
      #view {
        display: flex;
        justify-content: space-evenly;
        margin: 0 auto;
        padding: 20px;
        width: 700px;
        height: 120px;
        background-color: rgb(250, 247, 247);
      }
      .ball {
        width: 70px;
        height: 70px;
        text-align: center;
        border-radius: 50%;
        line-height: 70px;
        font-weight: bold;
        align-self: center;
      }
      .ball0 {
        background-color: rgb(247, 239, 130);
      }
      .ball1 {
        background-color: skyblue;
      }
      .ball2 {
        background-color: rgb(255, 89, 47);
      }
      .ball3 {
        background-color: lightgray;
      }
      .ball4 {
        background-color: greenyellow;
      }
    </style>
  </head>
  <body>
    <h1>이번주 로또 예상 번호</h1>
    <button id="btn">번호 생성</button>
    <div id="view"></div>
    <script>
      document.querySelector("#btn").addEventListener("click", game);
      function game() {
        let lotto = [];
        while (lotto.length < 6) {
          let num = parseInt(Math.random() * 45 + 1); // 1 - 45 난수
          // 같은수 배제
          if (lotto.indexOf(num) == -1) lotto.push(num);
        }
        lotto.sort(function (a, b) {
          return a - b;
        });
        let i = 0;
        let view = ``;
        let intervalId = setInterval(function () {
          if (lotto.length == i) {
            clearInterval(intervalId);
            return;
          }
          view += `<div class="ball ball${parseInt(lotto[i] / 10)}">${lotto[i++]}</div>`;
          document.querySelector("#view").innerHTML = view;
        }, 1000);
      }
    </script>
  </body>
</html>
```

<aside>
💡 display: flex;
justify-content: space-evenly; /* 동일한 간격으로 노출되도록 함*/
line-height: 70px;
align-self: center;
십의자리 단위로 공 색깔 다르게 노출시키는 방법

</aside>

<lotto 미션2>

- 화면 번호출력 밑에 로그 남기기

(재시작해도 로그 기억하기)

=> 전체 브라우저를 닫고 새로 시작시에도 로그 기억하기예)

2024-8-6 15:20 [2, 20, 27, 31, 34, 40]

2024-8-6 15:22 [1, 17, 30, 31, 44, 45]

2024-8-6 15:23 [3, 15, 19, 32, 33, 40]

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>셀프 도전</title>
    <style>
        body {
            text-align: center;
        }
        
        button {
            border: 0px;
            border-radius: 8%;
            background-color: cornflowerblue;
        }

        .box {
            display: flex;
            justify-content: space-evenly; /* 동일한 간격으로 노출되도록 */
            background-color: lightblue;
            width: 700px;
            height: 120px;
            margin: auto;
            padding: auto;
        }

        .ball {
            border-radius: 50%;
            width: 70px;
            height: 70px;
            text-align: center;
            line-height: 70px;
            align-self: center;
        }
        .ball0 {
            background-color: tomato;
        }
        .ball1 {
            background-color: plum;
        }
        .ball2 {
            
            background-color: cadetblue;
        }
        .ball3 {
            background-color: slateblue;
        }
        .ball4 {
            background-color: yellowgreen;
        }

    </style>
</head>
<body onload="before()">
    <h1>이번주 로또 예상 번호</h1>
    <button onclick="lotto()">번호 생성</button>
    <br>
    <br>
    <div class="box"></div>
    <div class="log"></div>
</body>
<script>
    function lotto() {
        let lotto = []
        while(lotto.length < 6) {
            let num = parseInt(Math.random()*45 +1)
            if(lotto.indexOf(num) == -1) {
                lotto.push(num)
            }
        }

        let str = ""
        for (let i = 0; i < 6; i++) {
            str += `<div class="ball ball${parseInt(lotto[i]/10)}">${lotto[i]}</div>`
        }
        document.querySelectorAll('div')[0].innerHTML = str

        let now = new Date()
        let date = `${now.getFullYear()}-${now.getMonth()}-${now.getDay()} ${now.getHours()}:${now.getMinutes()}`
        localStorage.setItem("date", date) 
        localStorage.setItem("lotto", JSON.stringify(lotto))
        
        document.querySelector('.log').innerHTML += `${localStorage.getItem("date")} ${localStorage.getItem("lotto")} <br>`
    }

    function before() {
        let now = new Date()
        let date = `${now.getFullYear()}-${now.getMonth()}-${now.getDay()} ${now.getHours()}:${now.getMinutes()}`

        document.querySelector('.log').innerHTML += `${localStorage.getItem("date")} ${localStorage.getItem("lotto")} <br>`
    }
</script>
</html>
```

<aside>
💡 TimeStamp 찍기 :
 `let date = new Date()`  >>완전 지저분한 문자열임

원하는 형식대로 만들어서 변수에 저장하기 :  
`let date =${now.getFullYear()}-${now.getMonth()}-${now.getDay()} ${now.getHours()}:${now.getMinutes()}`

</aside>
