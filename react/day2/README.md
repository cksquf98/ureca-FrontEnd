# 리액트
https://ko.react.dev/

**리액트는 사용자 인터페이스를 만들기 위한 자바스크립트 라이브러리-!**

**Single-Page-Application 프로젝트에 활용**

**가상DOM을 이용해서 변경해야 할 사항만 브라우저DOM에 반영 ⇒ 빠르다!**

<aside>
💡 Virtual DOM (구글링)
<img width="640" alt="image" src="https://github.com/user-attachments/assets/d22b4f08-8e41-48f3-bbcf-90f8ecee9585" />

리액트는 DOM(document = 페이지 전체)을 수정하는게 아니라 업데이트할 부분만 수정

- 상태 변경 발생 시 리액트는 업데이트해야할 일부분을 탐색 = Compute Diff
- 검색된 부분만 수정
- 다시 렌더링하여 사용자에게 출력
</aside>

### Node.js - Module 추가설명

<Node.js가 모듈 JavaScript파일을 실행하기위해 필요한 것>==> type이 module이라는 것을 알려줌

1. npm init -y
2. package.json파일 생성
3. "type": "module",  추가
4. 실행

```jsx
Terminal> 

1. 실행하려는 파일이 있는 디렉토리 이동
2. npm init -y 
	 entry point : (export_module파일이름.js) 시작점 설정 파일import_module.js
3. 디렉토리 내 package.json파일 생성됨
4. 해당 파일 내 "type": "module", 문장 추가
5. 실행 : node .\module_import.js (tab키 누르면 파일명 자동완성)
```

# 리액트

### 사용법

```jsx
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>

*crossorigin : 익명으로 접근해서 인증 정보를 보내지 않게 해서 보안을 높여주기 위함
```

- ReactDOM을 통해서 브라우저에 변경 사항 반영
    - **createElement(type태그명, [props속성], […children])**
        - 
        - JSX를 자바스크립트로 변환하는 역할
        - 속성을 지정할 경우 key-value객체로 넣어야 한다는 점

```jsx
<body>
    <h3>Hello 리액트</h3>
    <div id="root"></div>
</body>
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
<script>
    "use strict"

    //엘리먼트 가져오기
    const container = document.getElementById("root")

    //ReactDOM을 통한 root 엘리먼트 생성 => ReactDom : 변경 사항만 브라우저에 반영 가능
    //루트 엘리먼트는 하나만 있어야 함
    const root = ReactDOM.createRoot(container)

    //반영
    root.render**(React.createElement("h2", {style : {color : "blue"}}, "Hi React!"))** 
    //createElement(태그명, 속성, 내용) 
    //== <h2>Hi React!</h2>
</script>
```

- 클래스를 정의해서 render함수를 생성하는 방법

```jsx
<script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
<script>
  "use strict";

  class Hello extends React.Component {
    render() {
      return **React.createElement('h2', null, 'Hello World!#3');**
    }
  }

  const container = document.getElementById('root');
  const root = ReactDOM.createRoot(container);
  root.render(React.createElement(Hello));
</script>
```

<aside>
💡 use strict : 엄격 모드로 전환시켜줌

⇒ 컴파일 에러 발생시킬만한 요소들을 사전에 고칠 수 있음

엄격 모드란? 

1. 기존에는 조용히 무시되던 에러들을 throwing합니다.
2. JavaScript 엔진의 최적화 작업을 어렵게 만드는 실수들을 바로잡습니다.
3. 엄격 모드는 ECMAScript의 차기 버전들에서 정의 될 문법을 금지합니다.

엄격 모드 적용법

“use strict” 구문을 script 맨 위에 삽입

“use strict” 구문을 함수 블록 맨 위에 삽입

</aside>

### 클래스

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes

prototype기반의 객체지향 프로그래밍 언어

- ES6부터 추가
- class는 직관적인 코드로 쉽게 읽을 수 있고 작성할 수 있음
- class 기반 언어에 익숙한 개발자가 더 빠르게 적응할 수 있다
- 클래스는 호이스팅되지 않는다!

```jsx
//파라미터로 받은 name을 this.name에 저장하는 함수(객체)
function My(name) {
    this.name = name
}

My.prototype.hello = function() {
    console.log("안녕 프로토타입~! " + this.name)
}

const my = new My("나길동")
my.hello()

---------------------------------------------------------------
class My2 {
    //클래스 생성자 명시 -> constructor
    constructor(name) {
        this.name = name
    }

    hello = function() {
        console.log('My2 > 안녕, 프로토타입!!=>'+ this.name)
    }
}

const my2 = new My2("너길동")
my2.hello()
```

- 클래스도 객체임 **
    - 변수에 할당 가능
    
    ```jsx
    // unnamed
    let Rectangle = class {
      constructor(height, width) {
        this.height = height;
        this.width = width;
      }
    };
    console.log(Rectangle.name);
    // 출력: "Rectangle"
    
    // named
    let Rectangle = class Rectangle2 {
      constructor(height, width) {
        this.height = height;
        this.width = width;
      }
    };
    console.log(Rectangle.name);
    // 출력: "Rectangle2"
    ```
    

- 클래스의 본문(body)은 strict mode로 실행된다!

- 클래스 속성, 메서드 호출 시 this를 통해 접근해서 호출

- Static
    - 정적 메서드는 클래스의 **인스턴스화 없이** 호출되며, **클래스의 인스턴스에서는 호출할 수 없음**
    - 정적 속성은 캐시, 고정 환경설정 또는 인스턴스 간에 복제할 필요가 없는 기타 데이터에 유용함
    
    ```jsx
      class Point {
        constructor(x, y) {
          this.x = x;
          this.y = y;
        }
      
        static displayName = "Point";
        static distance(a, b) {
          const dx = a.x - b.x;
          const dy = a.y - b.y;
      
          return Math.hypot(dx, dy);
        }
      }
      
      const p1 = new Point(5, 5);
      const p2 = new Point(10, 10);
      console.log(p1.displayName); // undefined
      console.log(p1.distance); // undefined
      console.log(p2.displayName); // undefined
      console.log(p2.distance); // undefined
      
      console.log(Point.displayName); // "Point"
      console.log(Point.distance(p1, p2)); // 7.0710678118654755
    ```
    

### 클래스 상속 - extends

- 하위 클래스에 생성자가 있다면 this사용 전 super( )를 호출해야 함

```jsx
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // super class 생성자를 호출하여 name 매개변수 전달
  }

  speak() {
    console.log(`${this.name} barks.`);
  }
}

let d = new Dog("Mitzie");
d.speak(); // Mitzie barks.
```

## JSX와 바벨

https://ko.react.dev/learn/writing-markup-with-jsx

- JSX(Javascript XML) : 자바스크립트를 확장한 자바스크립트 문법
    - UI가 어떻게 보여야 하는지 설명하기 위한 형식 - JS에 마크업 태그를 넣은 것!
    - Web이 interactive해지면서 로직이 내용을 결정하는 경우가 많아짐 ⇒ JS가 HTML까지 담당하게 되었는데, 이로 인해 React에서 렌더링 로직과 마크업이 같은 위치(=컴포넌트)에 있게 되었음
        <img width="307" alt="image" src="https://github.com/user-attachments/assets/5a91e2ce-77ca-4bb6-8380-e63f51d437d2" />

    
    <aside>
    💡 XML 문법
    엘리먼트(마크업 태그)가 하나 이상 있어야 함
    
    </aside>
    
- 바벨 : 자바스크립트 컴파일러
    - 호환성!! 맞춰주는 역할
    - 최신 버전의 JS코드를 모든 브라우저가 해석할 수 있는 JS코드로 변환해줌
    - 사용법
    
    ```jsx
    <script src="https://unpkg.com/@babel/standalone/babel.min.js" crossorigin></script>
    <script type="text/babel">
    	//코드 작성
    </script>
    ```
    
    ```jsx
    <body>
    <h1>Hello 리액트</h1>
    
    <div id="root"></div>
    
    <script src="https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js" crossorigin></script>
    <script type="text/babel">
      "use strict";
    
      function Hello() {
        return <h2>Hello World!#2</h2>;
      }
    
      const container = document.getElementById('root');
      const root = ReactDOM.createRoot(container);
      root.render(<Hello />);
    </script>
    </body>
    ```
    

### JSX 규칙

💡 엄격한 태그 문법

- JSX 태그는 하나로 감싸줘야 함
- 모든 태그는 닫혀야 함

```jsx
<h1>Hedy Lamarr's Todos</h1>
<img
  src="https://i.imgur.com/yXOvdOSs.jpg"
  alt="Hedy Lamarr"
  class="photo"
>
<ul>
    <li>Invent new traffic lights
    <li>Rehearse a movie scene
    <li>Improve the spectrum technology
</ul>
```

→ JSX 변환

```jsx
<>
  <h1>Hedy Lamarr's Todos</h1>
  <img
    src="https://i.imgur.com/yXOvdOSs.jpg"
    alt="Hedy Lamarr"
    className="photo"
  />
  <ul>
    <li>Invent new traffic lights</li>
    <li>Rehearse a movie scene</li>
    <li>Improve the spectrum technology</li>
  </ul>
</>
```

     1> class → **className** : **class예약어는 식별자로 사용 불가**

2> img 닫는 태그 추가

3> li 닫는 태그 추가

4> 하나의 부모태그로 감싸기 위해 <>, </> (=Fragment)태그 추가

 - Fragments는 브라우저상의 HTML 트리 구조에서 흔적을 남기지 않고 그룹화해줌

💡 JSX 변환기

https://transform.tools/html-to-jsx

💡 [`aria-*`](https://developer.mozilla.org/docs/Web/Accessibility/ARIA)와 [`data-*`](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes)의 어트리뷰트는 HTML에서와 동일하게 대시를 사용하여 작성

- 다른 어트리뷰트엔 예약어, 특수문자( _, $ 제외) 사용 X

### JSX 안에서 자바스크립트 사용하기

- JSX에서 **중괄호**를 사용하여 **JavaScript**를 사용할 수 있음!

```jsx
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

- 이중 중괄호를 사용해서 객체를 전달

```jsx
JS 객체 생성
let v1 = { }
let v2 = new Object()

<JS>
person = { name: "Hedy Lamarr", inventions: 5 }

<JSX>
person = {{ name: "Hedy Lamarr", inventions: 5 }}

<JSX 인라인 CSS스타일>
export default function TodoList() {
  return (
    <ul style={
     {
      backgroundColor: 'black',
      color: 'pink'
     }
    }>
      <li>Improve the videophone</li>
    </ul>
  );
}
```

## 실습

<aside>
💡 npm = Node Project Manager

</aside>

- 프로젝트 생성
    - npx create-react-app 프로젝트이름
    - public > index.html

- Book.jsx 파일 생성
    - 중괄호 문법 사용 가능
    
    ```jsx
    import React from "react"
    
    export function Book(props) {
        return (
            <div>
                <h1>{`이 책의 이름은 ${props.name}입니다.`}</h1>
                <h2>{`이 책은 총 ${props.numOfPage}페이지입니다.`}</h2>
                <hr></hr>
            </div>
        )
    }
    ```
    

- Library.jsx
    
    ```jsx
    import React from "react" //react 예약어 임포트
    import { Book } from "./Book" //Book.jsx에 있는 Book() 임포트
    
    function Library() {
        return (
            <div>
                <Book name="props.name에 들어갈 이름1"
                      numOfPage={100}></Book>
                <Book name="props.name에 들어갈 이름2"
                      numOfPage={200}></Book>
                <Book name="props.name에 들어갈 이름3"
                      numOfPage={300}></Book>
            </div>
        )
    }//Library = 부모, Book = 자식
    ```
    

- App.js
    - localhost:3000에서 보이는 애가 App.js구먼
    - App을 import하는 애를 찾아보니 index.js파일이 부르는구먼
        
        ⇒ root.render 안에 있는 <App /> 주석처리
        
    
    ```jsx
    import {Book, Library} from "./Library"
    
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(
      <React.StrictMode>
        {/* <App /> */}
        **<Library/>**
      </React.StrictMode>
    );
    ```
