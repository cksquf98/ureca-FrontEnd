## 조건부 렌더링

어떠한 조건에 따라서 렌더링이 달라지는 것

ex. True면 버튼을 보여주고 False면 버튼을 가리도록 헨더링

### 자바스크립트의 Truthy와 Falsy

- Truthy
    - 불리언 true는 아니지만 true로 여겨지는 값
    
    ```jsx
    //truthy
    true
    {} ( = new Object )
    [] ( = new Array )
    number (not zero)
    "0", "false" string (not empty)
    ```
    
- Falsy
    - 불리언 false는 아니지만 false로 여겨지는 값
    
    ```jsx
    //falsy
    false
    0, -0
    0n (BigInt zero)
    '' "" `` (empty string)
    null
    undefined
    NaN (not a number)
    ```
    

### 조건부 렌더링 실습 코드

```jsx
[Greeting.jsx]

function UserGreeting(props) {
    return <h1>회원 등장</h1>
}

function GuestGreeting(props) {
    return <h1>회원가입 하시오</h1>
}

function Greeting(props) {
    const isLoggedIn = props.isLoggedIn
    console.log(isLoggedIn)

    if(isLoggedIn) {
        return <UserGreeting />
    }
    else {
        return <GuestGreeting />
    }
}

export default Greeting

-----------------------------------------------------------
[index.js]

<Greeting isLoggedIn={""}/>
<Greeting isLoggedIn={{}}/>
<Greeting isLoggedIn={"hihi"}/>
<Greeting/> => props에는 undefined가 들어감
```

### Element Variable

리액트 엘리먼트를 변수처럼 사용하는 방법

변수에 컴포넌트를 넣어서 해당 변수를 리턴
<img width="704" alt="image" src="https://github.com/user-attachments/assets/729700da-53a7-49f4-bbb8-f9238f23dbd0" />
- props : 부모가 자식에게 전달
- 이름 정의는 내가 원하는 대로

html태그 내에 onClick이 있다면 얘네는 그 태그의 이벤트 속성

- 속성 이름 정해져 있는 그대로 써야함!

```jsx
import { useState } from "react"
import Greeting from "./Greeting"

function LoginButton(props) {
    return(<button onClick={props.onClickBtn}>로그인</button>) //여기 onClick은 이벤트 속성!! onClick 정확히 기입
}

function LogoutButton(props) {
    return(<button onClick={props.onClickBtn}>로그아웃</button>)
}

function LoginControl(props) {

    const [isLoggedIn, setLoggedIn] = useState(false)
    
    //로그인 콜백함수
    const handleLoginClick = () => {
        setLoggedIn(true)
    }

    //로그아웃 콜백함수
    const handleLogoutClick = () => {
        setLoggedIn(false)
    }

    //엘리먼트 변수 : 렌더링해야될 컴포넌트를 변수처럼 사용   
    let button

    if(isLoggedIn)
		    button = <LogoutButton onClickBtn={handleLogoutClick}/> 
    else
        button = <LoginButton onClickBtn={handleLoginClick}/> //여기 onClick은 컴포넌트의 props!!
    
    
    return (
        <div>
		        <Greeting isLoggedIn={isLoggedIn}/>
            {button}
        </div>
    )
}
```

### Inline Conditions

조건문을 코드 안에 집어넣는 것

- IF문 대신 `&&` 연산자를 사용

<aside>
💡 true && expression → 무조건 expression 수행됨
false && expression → 무조건 false

</aside>

```jsx
if ( A ) {
	if( B ) {
		statement
	}
}
== if ( A && B ) {
		statement
	}
	
-------------------------------------------------------------------------------
	
function MailBox(props) {
	const unreadMessages = props.unreadMessages
	
	return(
		<div>
			{unreadMessages.length > 0 &&
					<h2> 현재 {unreadMessages.length}개의 읽지 않은 메세지가 있습니다. </h2>
		</div>	
```

- IF - ELSE문 대신 `?` 삼항연산자를 사용

<aside>
💡 condition ? true : false

</aside>

```jsx
function MailBox(props) {
	return (
		<div>
			<h2>현재 {props.unreadMessages.length > 0 ? '안읽은 메세지 있음' : '안읽은 메세지 없음'</h2>
		</div>
	)
}
```

### Component 렌더링 막기

**null을 리턴하면 렌더링되지 않음 ****

### 실습코드
<img width="494" alt="image" src="https://github.com/user-attachments/assets/43fea40a-e625-4290-93e8-2340eed5d4ba" />

props에 있는 애들을 할당
<img width="557" alt="image" src="https://github.com/user-attachments/assets/b850ad4b-22aa-422c-a473-7fdb05d3b5e0" />
<img width="467" alt="image" src="https://github.com/user-attachments/assets/5b42b593-97e2-4176-94bf-57675877fa51" />

```jsx
[Toolbar.jsx]
const styles = {
    wrapper: {
        padding: 16,
        display: "flex",
        flexDirection: "row",
        borderBottom: "1px solid grey",
    },
    greeting: {
        marginRight: 8,
    },
}

export default function Toolbar(props) {
    const {isLoggedIn, onClickLogin, onClickLogout} = props

    return (
        <div style={styles.wrapper}>
            {isLoggedIn && <span style={styles.greeting}>환영합니다!</span>}
            {isLoggedIn ? (<button onClick={onClickLogout}>로그아웃</button>) : (<button onClick={onClickLogin}>로그인</button>)
        }
        </div>
    )
}

-------------------------------------------------------------------------------------
[LandingPage.jsx]
import { useState } from "react"
import Toolbar from "./Toolbar"

export default function LandingPage(props) {
    //상태 관리
    const[isLoggedIn, setIsLoggedIn] = useState(false)

    //로그인 콜백함수
    const onClickLogin = () => {
        setIsLoggedIn(true)
    }

    //로그아웃 콜백함수
    const onClickLogout = () => {
        setIsLoggedIn(false)
    }

    return(
        <div>
            <Toolbar onClickLogin={onClickLogin} onClickLogout={onClickLogout}
                isLoggedIn={isLoggedIn}/>
            <div style={{padding:20, color : "blueviolet"}}>-----안뇽-----</div>
        </div>
    )
}
```

## 리스트와 키

### 리스트

- == 배열
- 자바스크립트의 변수나 객체들을 하나의 변수로 묶어놓은 것

### 키

- 각 객체나 아이템을 구분할 수 있는 고유한 값
- 같은 리스트에 있는 Elements사이에서만 고유한 값이면 된다

### map

<aside>
💡 map( ( **element** [, index, array] ) ⇒ { 리턴문 } )

</aside>

- for-each는 리턴값이 없는 반면에 map함수는 배열을 돈 결과로 얻어지는 새로운 배열을 반환함
- map함수 안에 있는 Elements는 꼭 키가 필요하다!
    - 키가 없어도 출력은 됨
    - map에 대해 key로 index를 사용하는 경우는 아이템들의 고유한 ID가 없을 경우에만 불가피하게 사용하도록 하자

```jsx
function NumberList(props) {
	const {numbers} = props
	const items = numbers.map((number)=> {
			<li key={number.toString()}>{number}</li>
	})
	return (
		<ul>{items}</ul>
	)
}

const numbers = [1,2,3,4,5]
ReactDOM.render(
	<NumberList numbers={numbers}/>,
	document.getElementById('root')
)
```

---

## Form

사용자로부터 입력을 받기 위해 사용

### Controlled Components

모든 데이터를 state에서 관리
<img width="729" alt="image" src="https://github.com/user-attachments/assets/21c34c32-4f26-422b-b686-4098e9a697f9" />

- **Input Form Element**
    - 값이 리액트의 통제를 받음
    <img width="530" alt="image" src="https://github.com/user-attachments/assets/ff6b270a-21b8-4ce8-bc32-867a1744c707" />

    
    - onChange 콜백함수를 통해 state를 set
    - 변경된 state를 value를 통해 출력
    - **preventDefault( )** : 페이지 이동을 막는 함수 ⇒ html에서 href=”#”로 걸어주는거랑 같은 역할
    

- **Input Form 실습코드**

```jsx
//출석부 컴포넌트에 Form 추가

import { useState } from "react"

const students = [
    { no:1, name : "고명진" },
    { no:2, name : "고은진"},
    { no:3, name : "고윤정"},
    { no:4, name : "고주희"}
]

function NameForm(props) {
    const [userName, setUserName] = useState("")

    //이름 input에 대해 처리하는 콜백함수
    const handleChange = (event) => {
        console.log(event.target.value) //event.target : 이벤트를 발생시킨 엘리먼트 객체 (=input태그)
        setUserName(event.target.value)
    }

    //submit에 대한 콜백함수
    const handleSubmit = (event) => {
        //제출된 이름을 alert로 출력
        alert(`입력된 이름 state: ${userName}`)
        **event.preventDefault() //submit 기능(새로고침)을 중지시킴**
    }

    return (
        <form action="">
            <label>
                이름
                <input type="text" onChange={handleChange} value={userName}/>
            </label>
            <button onClick={handleSubmit}>제출</button>
        </form>
    )
}

export default function Form(props) {
    return (
        <>
        <ul>
        {
            students.map((student) => {
                return <li key={student.no}>{student.name}</li>
            })
        }
        </ul>
        <hr/>
        <NameForm />
        </>
    )
}
```

## 실습 미션

MyNumberGuess2.jsx
<img width="332" alt="image" src="https://github.com/user-attachments/assets/c34cd470-a927-4334-9ea1-c9691d85357a" />

=> 시도한 번호에 대해 history를 출력

=> '새게임' 버튼을 통해 다시 시작

```jsx
// MyNumberGuess2

import { useState } from "react";

function Guess2(props){
    
    //난수 발생 
   const  [com_num, setCom_num] = useState(Math.floor( Math.random()*100 + 1 ));  //1~100

   //사용자가 입력한 데이터를 (상태)관리
   const  [user_num, setUser_num] = useState("");
   const  [result, setResult] = useState("");

   const  [tryCount,   setTryCount  ]= useState(1); //시도횟수
   const  [tryHistory, setTryHistory]= useState([]); //시도이력

   function checkNum(){
      console.log("com_num=", com_num, ", user_num=", user_num);

      let historyStr = `${tryCount}번째 시도 [${user_num}]: `;
      
      if(user_num > com_num){//사용자 입력값이 높을때
        historyStr=(`${historyStr} 낮춰주세요!`);
      }
      else if(user_num < com_num){//사용자 입력값이 낮을때
        historyStr=(`${historyStr} 높여주세요!`);
      }
      else{//정답
        historyStr=(`${historyStr} 정답입니다^O^`);
      }

      setUser_num("");//사용자 입력값 지우기
      console.log(tryCount) //1
      setTryCount(tryCount+1);//시도 횟수 증가
      console.log(tryCount); //1 state는 재렌더링 시 증가됨 => 2로 나오게 하고 싶으면 useEffect 사용하기
      
      
      setResult(historyStr);
      
      setTryHistory([historyStr, ...tryHistory]); //시도한 결과 문자열을 배열에 저장(히스토리 남김)
   }//checkNum

   function handleChange(event){//HTML마크업의 변경된 값을 state에 반영
      setUser_num(event.target.value);
   }

   function handleKeyDown(event) {
    console.log(event);
    
      if(event.key === "Enter") {
        checkNum()
      }
   }

   function newGame() {
      setUser_num("");//사용자 입력값 지우기

      setTryCount(1);//시도 횟수 증가
      
      setTryHistory([]);

      setCom_num(Math.floor( Math.random()*100 + 1 )) //새로운 난수 적용
   }

    return (
        <div>
            <h1>숫자맞추기</h1>
            <p>1~100사이 컴퓨터의 숫자를 맞춰보세요</p>
            <p>{tryCount}번째 시도</p>

            <input type="number" 
                    min="1" 
                    max="100"
                    value={user_num}
                    onChange={handleChange}
                    onKeyDown={handleKeyDown}/>
            <button onClick={checkNum}>정답확인</button>
            <button onClick={newGame}>새 게임</button>
            <div>
            {/* 첫 시작에는 확인결과 div가 안보이게 하고싶어 => inline if (boolean && 표현식) 활용 */}
            {tryHistory.length > 0 && <>확인결과 &gt;&gt;&gt;  {result} </>} 
            </div>
            
            <div>
                <ul>
                    {tryHistory.map((h)=> <li>{h}</li> )}
                </ul>
            </div>
        </div>
    );
}

export default Guess2;
```

<aside>
💡 onKeyDown 속성

</aside>
