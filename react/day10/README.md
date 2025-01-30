### 컨택스트 실습코드

- **테마 변경 웹사이트**
    
    ```jsx
    /*
    DarkOrLight : 루트 컴포넌트
    MainContent : 내용 출력 컴포넌트
    ThemeContext : 컨텍스트 객체
    */
    
    [DarkOrLight.jsx]
    import { useCallback, useState } from "react";
    import MainContent from "./MainContent";
    import ThemeContext from "./ThemeContext";
    
    export default function DarkOrLight(props) {
      //테마 상태 관리
      const [theme, setTheme] = useState("light");
    
      //테마 값 변경에 따라 토글하는 콜백 정의
      const toggleTheme = useCallback(() => {
    
        if (theme === "light") setTheme("dark");
        else setTheme("light");
    
      }, [theme]);
    
      return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
          <MainContent />
        </ThemeContext.Provider>
      );
    }
    
    ```
    
    ```jsx
    [MainContent.jsx]
    import { useContext } from "react";
    import ThemeContext from "./ThemeContext";
    
    export default function MainContent(props) {
      const { theme, toggleTheme } = useContext(ThemeContext);
    
      return (
        <div
          style={{
            width: "100vw",
            height: "100vh",
            padding: "1.5rem",
            // backgroundColor: "white",
            backgroundColor: theme === "light" ? "white" : "black",
            // color: "black",
            color: theme === "light" ? "black" : "white",
          }}
        >
          <p>테마 변경이 가능한 웹사이트입니다.</p>
          <button onClick={toggleTheme}>테마 변경</button>
        </div>
      );
    }
    
    ```
    
    ```jsx
    [ThemeContext.jsx]
    import { createContext } from "react";
    
    const ThemeContext = createContext();
    ThemeContext.displayName = "Theme 컨택스트"; //개발자 도구에서! 식별 이름으로 사용됨
    
    export default ThemeContext; // 공유 공간 만들고 변수로 이름 부여
    ```
    

<aside>
💡 CSS width height 단위

rem :  root(최상위 루트노드) 기준 em(배수) 크기

vh = viewport height

vw = viewport width 

현재 실행중인 스크린 크기에 맞춰 상대적 크기를 반환 ⇒ 퍼센티지랑 유사함

100vh, 100vw 가 전체 화면의 기준이 됩니다.

현재 스크린 크기가 height = 1000px, width = 800px 이라면

1vh = 10px

1vw = 8px

</aside>

### 미션
<img width="528" alt="image" src="https://github.com/user-attachments/assets/40990cb9-e18f-40d4-9222-cde74eb922fa" />

- 내 코드
    - state를 human computer result 이렇게 3개로 설정하고 컨택스트로 공유하도록 함
    - Header, Body, Result, GameContext, GameApp 5개의 컴포넌트로 분리
    
    day0821/Mission폴더에 코드 있음
    

---

## React CSS

### 1️⃣ 인라인 스타일

태그 안에 `style = {{ 속성 : “적용값” }}` 추가

- 태그마다 각각 처리해줘야 함
- 비효율적임
- 테스트 용도로 잠깐 사용

### 2️⃣ import CSS

external css : html에서 사용하는 형식과 같음

`import “path/file.css”`

- import CSS의 문제점!!!
    - 같은 클래스명이나 태그 등이 있다면 가장 마지막에 import된 css로 무조건 덮어씌워짐
        
        **⇒ Module을 사용하는 이유가 된닷**
        

### 3️⃣ CSS 모듈

https://velog.io/@kwonh/React-CSS를-작성하는-방법들-css-module-sass-css-in-js

**사용법**

- css파일 이름 설정 : 이름.module.css
- 모듈 사용법 in JS :
    
    ```jsx
    import sty from "./Box1.module.css";
    
    export default function ({ size }) {
      if (size === "big") {
        // return <div className='box big'>큰 박스</div>;
        return <div className={`${sty.box} ${sty.big}`}>큰 박스</div>;
      } else {
        return <div className={`${sty.box} ${sty.small}`}>작은 박스</div>;
      }
    }
    ```
    

### 4️⃣ styled-components

CSS코드를 자바스크립트 파일안에서 작성

사용법

- 프로젝트 폴더에서 `npm i styled-components` 터미널에 입력

```jsx
import styled from 'styled-components'

//styled.div == <div> 태그
const BoxCommon = **styled.div**`
    width : **${props=> (props.isBig? 200:100)}px;**    // (1)
    height : 50px;
    background-color:#aaaaaa;
`; //백틱 안 내용이 스타일 정의 부분

export default function Box({size}) {    //(2)
    const isBig = size==='big';
    const label = isBig? '큰 박스' : '작은 박스'
    return <BoxCommon isBig={isBig}>{label}</BoxCommon>
}
```

(1) 부분을 보면 isBig이라는 props를 받아 삼항식으로 판별합니다.

(2) Box컴포넌트는 size를 props로 받고 'big'인지 판별합니다.

 big일 경우 isBig은 true, 아닐경우 false입니다.

결과적으로 BoxCommon의 width값이 size에 따라 변경됩니다.

<aside>
💡 **위 코드에서 width 속성값 적용 시 함수를 써야하는 이유**
`styled-components`가 `props`에 접근할 수 있게 해주기 위함이다!!

`styled-components`는 스타일이 동적으로 변경될 수 있도록 하는데, 이때 `props`를 기반으로 계산된 값을 스타일 속성에 적용해야 할 때가 있습니다. 예를 들어, `isBig`이라는 prop에 따라 박스의 크기를 동적으로 결정하려면 해당 값을 함수로 전달하여 `props`에 접근해야 합니다.

```jsx
const BoxCommon = styled.div`
  width: function getWidth(props) {
    return props.isBig ? 200 : 100;
  }px;
  height: 50px;
  background-color: #aaaaaa;
`;

일반 함수로도 props 접근 가능
```

</aside>

### 5️⃣ 부트스트랩

라이브러리 설치 필요

`npm install react-bootstrap bootstrap`

- 설치 후 사용법
    - index.js에 `import 'bootstrap/dist/css/bootstrap.css';` 추가
    
    ```jsx
    [Bootstrap.jsx]
    import Button from "react-bootstrap/Button";
    
    function TypesExample() {
      return (
        <>
          <Button variant="primary">Primary</Button>{" "}
          <Button variant="secondary">Secondary</Button>{" "}
          <Button variant="success">Success</Button>{" "}
          <Button variant="warning">Warning</Button>{" "}
          <Button variant="danger">Danger</Button>{" "}
          <Button variant="info">Info</Button>{" "}
          <Button variant="light">Light</Button>{" "}
          <Button variant="dark">Dark</Button>
          <Button variant="link">Link</Button>
        </>
      );
    }
    export default TypesExample;
    ```
    
    ```jsx
    import Form from "react-bootstrap/Form";
    
    function TextControlsExample() {
      return (
        <Form>
          <Form.Group className="mb-3" controlId="exampleForm.ControlInput1">
            <Form.Label>Email address</Form.Label>
            <Form.Control type="email" placeholder="name@example.com" />
          </Form.Group>
          <Form.Group className="mb-3" controlId="exampleForm.ControlTextarea1">
            <Form.Label>Example textarea</Form.Label>
            <Form.Control as="textarea" rows={3} />
          </Form.Group>
        </Form>
      );
    }
    
    export default TextControlsExample;
    ```
