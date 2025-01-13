<10주차수업>

- 훅을 이용한 컴포넌트 생성
- React를 이용한 이벤트 처리
- onChange, input, onKeyPress 이벤트 처리
- -광복절-
- DOM 사용하기, 컴포넌트에 ref 달기

---

# Hooks

컴포넌트 만드는 방법

1. Class Component 생성
    - 생성자에서 **State** 정의
        - setState( )를 통해 상태 업데이트
    - **Life Cycle Methods** 제공 : componentDidMount( ) 얘네들
    
2. 함수 Component 생성
    - State 사용 ❌
    - Life Cycle에 따른 기능 구현 ❌
    
    ***⇒ Hook 사용을 통해 State, Life Cycle 기능 구현 ✅***
    

<aside>
💡 State를 사용하는 이유?
불변성을 가지고 있는 리액트 엘리먼트들을 재랜더링을 통해 변경하고자 사용

</aside>

## 1️⃣ useState

state를 사용하기 위한 훅

- 사용법

```jsx
**const [ 변수명, set함수명 ] = useState( 초기값 )**
```

```jsx
import React, {useState} from "react"

1. 훅 사용 안한 버전
export function Counter(props) {
	var count = 0
	return(
		<div> 
			<p>총 {count}번 클릭</p>
			<button onClick={()=> { count++; console.log(count); }}>클릭</button>
		</div>
	);
} //화면에서 count 변화 없음 -> 불변성으로 인해 노출 안됨!(재랜더링 안돼서 변화 없는 상태임) 
//실제 변수값은 변경됨

2. 훅 사용한 버전
export function Counter2(props) {
    const [count, setCount] = useState(0)
    return (
        <div> 
			<p>총 {count}번 클릭</p>
			<button onClick={() => {setCount(count+1)}}>클릭</button>
		</div>
    )
}
//count++하면 에러남 : 
//	count++ = count = count+1 -> count는 const 상수이기 때문에 에러!
//state가 변경되어 리렌더링 발생하므로 화면에서 count값 변화됨
```

- state 각각에 대해 set함수는 따로 존재함

## 2️⃣ useEffect

리액트의 함수 컴포넌트에서 Side Effect를 실행할 수 있게 해주는 훅

- 사용법
    - 명시된 의존성 배열 State가 mount, update, unmount될때마다 이펙트 함수를 실행
    - 의존성 배열에 빈 배열을 넣으면 → 이펙트 함수의 Mount, unMount 시에 실행됨 (update시에는 호출 ❌)
    - 의존성 배열을 생략하면 → 모든 컴포넌트에 대해 **Mount, Upate, unMount** 될 때마다 호출됨

```jsx
useEffect(이펙트 함수, [의존성 배열1, 의존성 배열2, ..])
useEffect(이펙트 함수, [])
useEffect(이펙트 함수)
```

```jsx
import React, {useEffect, useState} from "react"

export function Counter2(props) {
    const [count, setCount] = useState(0)
    
    useEffect(() => {
        document.title = 'you clicked ${count} times'
    })//의존성 배열 생략 >> 버튼 클릭할 때마다 컴포넌트 변경 -> useEffect함수 실행 -> BOM을 사용해서 타이틀 업데이트됨
    
    
    
    useEffect(() => {
        document.title = 'you clicked ${count} times'
    }, [])//맨 처음 State값 그대로
    
    
    
    useEffect(() => {
        document.title = 'you clicked ${count} times'
    }, [count])//count 변경할 때마다 호출
    
    
    
    return (
        <div> 
			<p>총 {count}번 클릭</p>
			<button onClick={() => {setCount(count+1); console.log(count);}}>클릭</button>
		</div>
    )
}
```

<aside>
💡 React Strict Mode :

React의 Strict Mode는 개발 환경에서만 활성화되며, 여러 가지 잠재적인 문제를 감지하고 경고하기 위해 일부 생명주기 메서드를 두 번 호출합니다. 이 모드에서는 특히 아래와 같은 부분에서 의도적으로 두 번 호출이 발생할 수 있습니다:

1. **Mounting (첫 렌더링)**: 컴포넌트가 처음 마운트될 때 `useEffect`가 실행됩니다. Strict Mode에서는 이때 두 번 실행되어 `console.log`가 두 번 찍히게 됩니다.
2. **Updating**: 특정 상태나 props가 변경될 때도 `useEffect`가 실행되는데, Strict Mode에서는 이 역시 두 번 실행됩니다.

- Strict Mode는 실제 배포 환경에서는 적용되지 않기 때문에, 실제 프로덕션에서는 `useEffect`가 두 번 실행되지 않습니다.

</aside>

- 컴포넌트가 unMount될 때 호출시키기

```jsx
import React, {useEffect, useState} from "react"

export function UserStatus(props) {
	const [isOnline, setIsOnline] = useState(null)
	
	function handleStatusChange(status) {
		setIsOnline(status.isOnline)
	}
	
	useEffect(() => {
		ServerAPI.subscribeUserStatus(props.user.id, handleStatusChange)
		
		return () => {//return문 : 컴포넌트가 unmount되기 직전에 호출 **
			ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange) 
		}
	})
	
	if(isOnline == null) {
		return '대기중..'
	}
	
	
	return isOnline ? '온라인' : '오프라인'
}
```

### useEffect 실습코드
<img width="373" alt="image" src="https://github.com/user-attachments/assets/23d89db9-f605-4e19-aa24-38388c33f1a7" />

: 메뉴 뽑기 버튼 누르면 배열 내 요소가 랜덤하게 뜨도록

```jsx
import React,{useState, useEffect} from "react";

export function Menu() {
    const [menu, setMenu] = useState("")
    
    const menus = ["마라탕", "미정국수", "곤드레나물밥", "베이글", "회덮밥"]

    useEffect(()=> {
        //랜더링 내용 변경시 호출
       document.title= `메뉴 - ${menu}`
    }, [menu])

    //버튼 누르면 랜덤하게 메뉴가 출력되도록
    return(
        <div>
            <p>오늘의 메뉴 : **{menu}** </p>
            <button onClick={() => {
                let num = **parseInt(Math.random() * 5)**
                setMenu(menus[num]) //상태 변경
            }}>메뉴뽑기</button>
        </div>
    )
}
```

## 3️⃣ useMemo

Memoized value를 리턴하는 훅

- 사용법
    - 의존성 배열이 변할때마다 useMemo 내에 있는 연산 실행
    - 의존성 배열을 넣지 않는 경우 → 매 랜더링마다 함수 실행
    - 의존성 비열이 빈 배열일 경우 → 컴포넌트 마운트 시에만 호출 (저장되어 있는 값을 그대로 계속 쓰겠다!)

```jsx
const memoizedValue = useMemo(() => {
		return Compute(의존성 변수1, 의존성 변수2) //연산량이 높은 작업을 수행하여 결과 반환
	},
	[의존성 변수1, 의존성 변수2]
)

const memoizedValue = useMemo(() => {
		return Compute([의존성 배열])
	},
)

const memoizedValue = useMemo(() => {
		return Compute(의존성 변수1, 의존성 변수2)
	},
	[]
)
```

## 4️⃣ useCallback

useMemo와 비슷하지만 값이 아닌 함수를 반환!

<aside>
💡 useCallback(함수, 의존성 배열) == useMemo(() ⇒ 함수, 의존성배열)

</aside>

- 사용법

```jsx
* 자식 컴포넌트한테 함수 전달

const handleClick = (event) => {
	//클릭 이벤트 처리
	}

	return(
	<div>
		<ChildComponent handleClick={handleClick} />
	</div>
)//이러면 재렌더링 될 때마다 매번 함수가 새로 정의됨

const handleClick = useCallback((event) => {
	//클릭 이벤트 처리
	}, [])

return(
	<div>
		<ChildComponent handleClick={handleClick} />
	</div>
)//컴포넌트가 마운트 될 때에만 함수가 정의됨
```

## 5️⃣ useRef

특정 컴포넌트에 접근할 수 있는 객체

<aside>
💡 redObject.current : 현재 참조하고 있는 엘리먼트를 의미

</aside>

- 사용법

```jsx
const refContainer = useRef(초기값)

* 잡아올 태그에 ref={refContainer} 명시해줘야 함
return (
	<>
		<div ref={refContainer}></div>
	</>
)
```

- 예제코드
    - 버튼 클릭하면 input창에 커서 포커싱되도록 하려고 함

```jsx
import React, { useRef } from "react";

export function TextInputWithFocusButton(props) {
    const inputElem = useRef(null)

    const onButtonClick = () => {
        //current는 마운트된 input element를 가리킴
        inputElem.current.focus() //input element에 포커스를 두기
    }

    return (
        <>
        <input type="text" ref={inputElem} />
        <button onClick={onButtonClick}>Focus the input</button>
        </>
    )
}
```

- 내부의 데이터가 변경되었을 때 별도로 알리지 않는다!!
    - 재렌더링❌
    - 상태 변경으로 인해 재랜더링이 일어날때라던가 그런 경우 같이 변경사항으로 반영됨

## Hook 규칙

1. Hook은 무조건 최상위 레벨에서만 호출해야 한다
2. 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다
    - 조건문 / 반복문 내 Hook 사용 불가 - 호출 순서가 달라질 수 있어서
3. 리액트 함수 컴포넌트에서만 훅을 호출해야 한다

## Custom Hook 만들기

- 이름이 꼭 use로 시작해야 함!
- 여러 개의 컴포넌트에서 하나의 커스텀 훅을 사용할 때 컴포넌트 내부에 있는 모든 State와 Effects는 전부 분리되어 있다
- 각 커스텀 훅의 호출은 완전히 독립적임

### Custom Hook 실습코드

```jsx
[useCounter.jsx]
//사용자 정의 훅
//count라는 state를 정의하고, count에 대한 증감 기능의 함수 제공
import React, { useState } from "react";

export function useCounter(initialValue) {
    const [count, setCount] = useState(initialValue)

    //증가함수
    const increaseCount = () => {
        setCount((count) => count + 1) //count state는 0부터 세지니까 1부터 세지게 할라고
    }

    //감소함수
    const decreaseCount = () => {
        **setCount((count) => Math.max(count - 1, 0))** //0 밑으로는 안내려가게!! (음수값 방지)
    }

    return [count, increaseCount, decreaseCount]
}
```

```jsx
[Accommodate.jsx]
//입장한 인원을 체크하는 컴포넌트

import { useEffect, useState } from 'react'
import { useCounter } from './useCounter'

//최대 수용 인원
const MAX_CAPACITY = 10

export function Accommodate(props) {
    //수용 인원 풀 상태 state
    const [isFull, setIsFull] = useState(false)

    
    //수용 인원 count hook
    const [count, increaseCount, decreaseCount] = useCounter(0)

    useEffect(() => {
        console.log('======================');
        console.log('useEffect is called')
        console.log(`isFull: ${isFull}`);
    }) //컴포넌트 변화가 생길 때마다 실행 

    useEffect(() => {
        setIsFull(count >= MAX_CAPACITY)
        console.log(`현재 카운트: ${count}`);
        }, [count]) //count값이 변경되었을 때 실행

    return (
        <>
        <div style={{ padding: 16 }}>
            <p>수용 인원: {count}</p>
            <button onClick={increaseCount} **disabled={isFull}**>Enter</button>
            <button onClick={decreaseCount}>Exit</button>
            **{isFull && <p style={{color:"red"}}>수용 인원이 최대입니다.</p>}**
        </div>
        </>
    )
}
```

<aside>
💡 **{isFull && <p style={{color:"red"}}>수용 인원이 최대입니다.</p>}**
: isFull이 true면 뒷 문장 수행

</aside>

---

## 미션

**<미션1>**

입력input에 글을 입력하고 버튼을 누르면 목록을 보인다.
<img width="657" alt="image" src="https://github.com/user-attachments/assets/48b1722e-8407-4ac9-8c07-25596ca52487" />

- 내 코드
```jsx
import { useEffect, useRef } from 'react';
import { useState } from 'react';
import React from "react";

export function Memo() {
    const [memos, setMemos] = React.useState([]);
    let memo = useRef()

    function addMemo(memo) {
        // memos.push(memo)  : memos를 set함수 없이 직접 수정 불가
        // setMemos(memos)
        let copy = memos.slice();
        copy.push(memo);
        setMemos(copy);
    }
    
    // function addMemo(newMemo) {
    //     setMemos(prevMemos => [...prevMemos, newMemo]); 스프레드 연산자 사용 방법
    // }

    return (
        <div>
            <h1>Memo</h1>
            <h3>메모폼</h3>
            <input type="text" ref={memo}/><button onClick={() => {
                addMemo(memo.current.value)
                // console.log(memo.current.value)
                }}>메모등록</button>
            <h3>메모목록</h3>
            <ul>
                {memos.map((memo, index) => (
                    <li key={index}>{memo}</li>
                ))}
            </ul>
        </div>
    )
}
```

- 답안 코드

```jsx
import { useState } from "react";

export default function MyMemo(props){

   const [memoContent, setMemoContent]=useState("");   //<input type=text>에 입력된 값 저장
   const [memoContents, setMemoContents]=useState([]); 

   function myCallback(event){//<input>에서 입력된 내용이 변경시 호출
    //   setMemoContent('abc');
      setMemoContent(event.target.value);
   }

   function myClickCallback(){
       //1. <input type=text>의 값을 배열에 입력!!
       // ==> state변수 memoContent에 저장되어 있음
       setMemoContents([...memoContents, memoContent] )  //배열값 변경(입력)

       //2. <input type=text>지우기 효과
       setMemoContent('');//입력된 메모텍스트를 삭제
   }

   return (
      <div>
          <h1>마이 메모장</h1>
          <div>
            <h2>메모Form</h2>
            <input type="text" onChange={myCallback} value={memoContent}/>
            <button onClick={myClickCallback}>메모등록</button>
          </div>
          <hr />
          <div>
            <h2>메모 목록</h2>         
            <ul>
                {/* <li>마라탕</li>
                <li>젤리</li>
                <li>탕후루</li> */}
                {memoContents.map(  (value) =>   <li>{value}</li>   )}
            </ul>   
          </div>
      </div>
   );
}

// export default MyMemo;
```

**<미션2>**

특정 초를 카운트하는 컴포넌트 만들기

→ setInterval, useEffect 사용 해 보기

- 이게 첨에 10 state : 10에서 부른 setInterval 동작 → setState(9)
- state 변경되니까 리렌더링되고 → 이전 10 state 컴포넌트 unMount →  state 9에 대한 useEffect가 또 호출
- 10 state에 대한 return( ) ⇒ 얘네 수행됨

https://velog.io/@enjoywater/React-Effect-Hook-Clean-up : 블로그 글 참고!!!

- 답안 코드
    
    <aside>
    💡 useEffect를 사용해서 반복문처럼 수행하도록 만들 수 있다!
    useEffect에서 return 구문 뒤에 반환하는 값은 함수여야 함
    
    </aside>
    
    <aside>
    💡 setInterval(콜백함수, 밀리 시간)
        설정한 시간마다 콜백함수를 호출
        변수에 저장 시 타이머 아이디를 반환함
    
    </aside>
    

```jsx
import React, { useEffect, useState } from "react";

export function Timer(props) {
    const [time, setTime] = useState(10)

    function myTimer() {
        setTime(time-1)
    }

    useEffect(() => {
        const timerId = setInterval(myTimer, 1000)

        if(time <= 0) {
            clearInterval(timerId) //setInterval 정지
        }

        //unMount 시 실행
        return () => clearInterval(timerId)

    }, [time])

    return (
        <div>
            <h3>Timer: {time}</h3>
        </div>
    )
}
```

**if문에서 타이머를 정지하는 코드가 있는데
return () => clearInterval(timerId) 이 문장이 필요한 이유**

- time이 계속 업뎃되니까 useEffect를 빠져나가려면 return문이 필요하다는디?
- if문은 조건에 맞을때만 수행이 됨 → time은 0보다 큰데 unmount되는 경우엔 타이머가 계속 진행이 될 수 있음
⇒ 따라서 혹시나 하는 경우로 해당 구문을 써주는거임
- 언마운트되는 경우

**useEffect의 clearInterval함수 동작 방식 - GPT**

- `return clearInterval(timerId)`처럼 쓰면, `clearInterval(timerId)`가 **즉시 실행**되고, 그 결과(보통 `undefined`)가 반환됩니다. 이렇게 되면 `cleanup function`이 아닌, 그냥 `undefined`를 반환하게 되는 거죠.
- `useEffect`는 함수 전체를 반환받아야 합니다. 이 함수가 나중에 실행되어야 하기 때문입니다. 그래서 `return () => clearInterval(timerId)`처럼 **함수 자체**를 반환해야 합니다. 여기서 반환된 함수는 필요할 때(언마운트 시나 `useEffect` 재실행 시) 호출됩니다.
