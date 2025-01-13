# Event Handler

어떤 사건이 발생하면 사건을 처리하는 역할

### Event

사용자가 일으킨 특정 사건

- DOM
    
    ```jsx
    <button onclick="func()"></button>
    ```
    

- React
    
    ```jsx
    <button onClick={func()}></button>
    ```
    
    - 차이점:
        1. onClick 스펠링
        2. 함수 호출 방식 : { 함수이름 }

### Event Listener

- 클래스 컴포넌트

```jsx
import React from "react"

class Toggle extends React.Component {
    constructor(props) {
        super(props)

        //토글을 표현하는 상태 변수 : isToggleOn (초기값 true)
        this.state = { isToggleOn : true }

        //콜백에서 this를 사용하려 함 => 바인딩 필수
        this.handleClick = this.handleClick.bind(this)
    }

    handleClick() {//콜백함수 - 바인딩을 통해 this 사용 가능
        this.setState(
	        (currentState) => ({ isToggleOn: !currentState.isToggleOn })
        )
    }

    render() {
        return  (
            <button onClick={this.handleClick}>
                {this.state.isToggleOn ? 'ON' : 'OFF'}
            </button>
        )
    }
}

export default Toggle
```

<aside>
💡 두 코드의 차이

```
handleClick() {
    this.setState((prevState) => **({isConfirm : !prevState.isConfirm})**)
}//정상 동작

handleClick() {
    this.setState((prevState) => **{isConfirm : !prevState.isConfirm}**)
}//구문 오류
```

객체를 반환하기 위해 소괄호 `()`를 사용하여 객체 리터럴을 감싸야 합니다. 그렇지 않으면 자바스크립트는 객체 리터럴이 아닌 코드 블록으로 해석합니다.

</aside>

- 함수형 컴포넌트
    - 생성자 ❌ → useState 사용
    - 렌더 함수 ❌

```jsx
import React, { useState } from 'react'

export default function Toggle(props) {
    const [isToggleOn, setIsToggleOn] = useState(true)

    function handleClick() {
        setIsToggleOn(!isToggleOn)
    }
    
    return (
        <button onClick={handleClick}>
            {isToggleOn ? 'ON' : 'OFF'}
        </button>
    )
}
```

### Argument

함수에 전달할 데이터

= 이벤트 핸들러에 전달할 데이터

### Parameter

함수에서 쓰는 용으로 전달된 데이터

---

### 이벤트 핸들러 실습코드

'확인하기' 버튼 클릭 시 '확인됨' 버튼 with 비활성화 변경시키기

- 클래스 컴포넌트

```jsx
import React from "react";

export class ConfirmButton extends React.Component {
    constructor(props) {
        super(props)
        this.state = { isConfirm : false }
        this.handleClick = this.handleClick.bind(this)
    }

    handleClick() {//콜백함수
        this.setState({isConfirm : !this.state.isConfirm})
    }

    render() {
        return (
            <button onClick={this.handleClick} disabled={this.state.isConfirm}>확인하기</button>
        )
    }
}
```

<aside>
💡 Disabled 속성 없애고 콘솔에 현재 버튼에 출력되는 문자열 찍어보기 - 매개변수 사용

※ event 를 넘기면 이벤트 객체 자체가 넘어감 

   → [event.target](http://event.target) = 해당 이벤트를 일으킨 태그 엘리먼트 = <button>이름</button>

※ event.target.innerText를 통해서 버튼태그 내 Content를 뽑을 수 있음

```jsx
import React, { useRef, useState } from "react";

export function ConfirmButton() {
    const [isConfirm, setIsConfirm] = useState(false)

    function handleClick(event) {
        //콘솔에 현재 버튼에 출력되는 문자열 찍어보기 - 매개변수 사용
        console.log(event.target);
        console.log(event.target.innerText);
        setIsConfirm(!isConfirm)
    }

    return(
        // <button onClick={(event) => handleClick(event)} disabled={isConfirm} >{isConfirm?"확인됨":"확인하기"}</button>
        <button onClick={(event) => handleClick(event)}>{isConfirm?"확인됨":"확인하기"}</button>
    )
}
```

</aside>

### 왜 로그가 다를까?

React에서 상태 변경은 비동기적으로 처리될 수 있습니다. 그러나 `event.target`과 `event.target.innerText`는 둘 다 **동기적**으로 이벤트가 발생한 시점의 값을 가져와야 합니다.

**실제로 발생하는 현상**:

- `console.log(event.target)`은 DOM 요소를 그대로 출력하기 때문에, 상태가 바뀌면서 이미 DOM이 업데이트된 상태를 반영한 `<button>` 요소를 보여줄 수 있습니다.
- 반면 `console.log(event.target.innerText)`는 그 시점에서의 텍스트 값을 출력합니다. 이 텍스트 값은 상태 변경 전에 있었던 값을 반영할 수 있습니다.

### 실습코드2
![Uploading image.png…]()

```jsx
import React, { useRef, useState } from "react"

export default function Guess() {
    const [number, setNumber] = useState(parseInt(Math.random()*100))
    console.log(number)
    const [guess, setGuess] = useState("")
    const input = useRef()
    
    function answer(value) {
        console.log(value);
        
        if(number > value) {
            setGuess("Up")
        } 
        else if(number < value) {
            setGuess("Down")
        } 
        else {
            setGuess("정답입니다")
        }
    }

    return(
        <div>
            <h1>숫자 맞추기</h1>
            <p>1 ~ 100사이 숫자 맞춰보세요</p>
            <input type="number" ref={input}/> <button onClick={()=>answer(input.current.value)}>정답확인</button>
            <p>확인 결과: {guess}</p>
        </div>
    )
}
```

- useRef를 썼는데 이거 안쓰고 input값 넘기도록 할라면?
    
    → input에 대한 onChange함수, button에 대한 onClick함수 필요
    
    → 인풋값 state에 저장해야 하나봄
    

```jsx
import React, { useRef, useState } from "react"

export default function Guess() {
    const [number, setNumber] = useState(parseInt(Math.random()*100))
    console.log(number)
    
    const [guess, setGuess] = useState("")
    const [value, setValue] = useState(null)
	   
    
		function handleInput(event) {
        setValue(event.target.value)
    }

    function handleBtn() {
        if(number > value) {
            setGuess("Up")
        } 
        else if(number < value) {
            setGuess("Down")
        } 
        else {
            setGuess("정답입니다")
        }
    }

    return(
        <div>
            <h1>숫자 맞추기</h1>
            <p>1 ~ 100사이 숫자 맞춰보세요</p>
            {/* <input type="number" ref={input}/> <button onClick={()=>answer(input.current.value)}>정답확인</button> */}
            <input type="number" onChange={(event)=>handleInput(event)}/> <button onClick={handleBtn}>정답확인</button>
            <p>확인 결과: {guess}</p>
        </div>
    )
}
```
