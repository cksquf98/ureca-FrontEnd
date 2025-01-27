**컴포넌트 간 데이터 공유 방법**

1. **Props : 부모 컴포넌트 데이터를 자식 컴포넌트가 쓰고싶어!**
    
   <img width="398" alt="image" src="https://github.com/user-attachments/assets/a0e7e947-3341-4e05-927c-762b85ba8110" />

    
2. **Context : Props로 공유할 수 없는 경우에 사용 → 전역 변수 느낌으로 공유됨**
    <img width="398" alt="image" src="https://github.com/user-attachments/assets/502b1a56-5a3e-407d-beb7-f5834cea1b9a" />


---

## 컨텍스트

여러 컴포넌트들이 접근해야 하는 데이터에 대해 사용

(ex. 로그인 여부, 로그인 유저 정보, UI테마 등등)

<aside>
💡 **<Context>**

1. 리액트 컴포넌트간에 어떠한 값을 공유할수 있게 해주는 기능
2. 전역적(global)으로 필요한 값을 다룰 때 사용 (꼭 전역적일 필요는 없음)
3. 컴포넌트간에 값을 전달하는 (props가 아닌) 또 다른 방법

**<props로만 데이터를 전달했을때 발생할 수 있는 문제>**

1. 깊숙히 위치한 컴포넌트에 데이터를 전달해야 하는 경우에는 여러 컴포넌트를 거쳐 연달아서 Props를 설정해주어야 하기 때문에 불편하고 실수할 가능성이 높아짐.
2. 여러단계를 거친(4단계 이상) 데이터 전달 => **Props Drilling**
    - 여러 단계를 거쳐야 해서 불편
    - 어디서 오는 값인지 파악하기 위해 거슬러 올라가는 것이 불편
    - 네이밍을 value에서 msg로 변경할시 통일성을 맞추기 위해서 또 여러 컴포넌트들을 수정하는 것이 불편 ( ⇒ 유지보수 힘듬 )
</aside>

### Context API

1️⃣ React.createContext

Context 생성

```jsx
const MyContext = React.createContext(기본값)
```

2️⃣ Context.Provider

**Provider**: Context에서 값을 설정해주는 컴포넌트입니다. `Provider`는 `value`라는 속성을 통해 하위 컴포넌트에 전달할 값을 지정합니다.

```jsx
function App(props) {
	return (
		<MyContext.Provider value={공유할 데이터값}>
			<Toolbar />
		</MyContext.Provider>
	)
}
			
* Provider 컴포넌트가 재렌더링될 때마다 모든 하위 Consumer 컴포넌트도 재렌더링 됨
	-> 전부 재렌더링 된다면 불필요한 재렌더링이 일어날 수 있음
	
function App(props) {
	const [value, setValue] = useState(공유데이터)
	return (
		<MyContext.Provider value={value}>
			<Toolbar />
		</MyContext.Provider>
	)
}
	
	
	-> **state를 사용하고 useCallback 또는 useMemo를 통해 불필요한 재렌더링 방지!!!**
	
	
	
```

3️⃣ Context.Consumer

컨텍스트 데이터 사용

```jsx
<MyContext.Consumer>
	{value => /* */}
</MyContext.Consumer>
```

4️⃣ function as a child

children 프로퍼티는 props에서 선언하지 않아도 그냥 사용 가능했던거처럼 

Context에서 함수를 children처럼 쓰는 방법

- 직접 선언하거나 컴포넌트로 감싸서 children으로 만들면 된다는디?
    
    ```jsx
    //직접 선언
    <Compo children={name => <p> 이름 : {name} </p>}
    
    //감싸기
    <Compo> {name => <p>이름 : {name} </p>} </Compo>
    ```
    

### Props와 비교

```jsx
1. props 사용
function App(props) {
	return <Toolbar theme="dark" />
}

function Toolbar(props) {
	return <ThemeButtton theme={props.theme} />
}

function ThemeButton(props) {
	return <Button theme={props.theme} />
}

2. Context 사용

* Provider를 통해 Provider로 설정한 컴포넌트의 하위 컴포넌트들은 그 데이터 사용 가능
**const ThemeContext = React.createContext('light')** //디폴트 테마 = light로 컨텍스트 생성

function App(props) {
	return (
		<ThemeContext.Provider **value="dark"**>
			<Toolbar />
		</ThemeContext.Provider>
	)
}

function Toolbar(props) {
	return <ThemeButtton />
}

* Consumer를 통해 가장 가까운 상위 ThemeContext의 Provider를 찾아서 사용
function ThemeButton(props) {
	return (
		<ThemeContext.Consumer>
			{value => <Button theme={**value**} />} //사용
		</ThemeContext.Consumer>
	)
}
```

### 컨텍스트 사용 전 고려할 점

- 엘리먼트 변수를 통해서 컴포넌트를 전달할 수 있음
    - 엘리먼트 변수 : 컴포넌트 엘리먼트들을 담고있는 변수
- 하위 컴포넌트를 여러개의 변수로 나눠서 전달할 수 있음

### 여러개의 Context 사용하기
<img width="518" alt="image" src="https://github.com/user-attachments/assets/8c66feff-74d5-4273-b3fa-6fa784a7dee7" />

컨텍스트 theme, user를 사용하려고 저 형식 (변수) ⇒ (실행문)을 가지는거군

### 실습코드1

- Props

```jsx
MyApp에 정의된 값이 전달전달됨
근데 중간 전달 지점에서 변경되면? -> 어디서 바뀐건지 찾아 헤매야함

export default function MyApp(props) {
  //부모

  //전달 데이터 -> "쉬는 시간"
  return <GrandParent value="쉬는 시간" />;
}

//Props의 단점 : 포함 관계가 많아질수록 Props가 여러 단계를 거쳐서 전달되어야 함
function GrandParent(props) {
  return <Parent value={props.value} />;
}
function Parent(props) {
  return <Child value={props.value} />;
}
function Child(props) {
  return <FirstChild value="메렁" />;
}
function FirstChild(props) {
  return <Message value={props.value} />;
}
function Message(props) {
  return <div>전달 받은 데이터 : {props.value}</div>;
}

```

- Context
    - **`useContext`** Hook을 사용해서 컨텍스트 접근 가능!
    - 최상위 돔 MyApp에서 컨텍스트 프로바이더로 데이터 저장

```jsx
import { createContext, useContext } from "react";

***//create Context
const myContext = createContext();***

export default function MyApp(props) {//부모
  //전달 데이터 -> "점심 시간"
  return (
    <myContext.Provider value="점심 시간">
      <GrandParent />
    </myContext.Provider>
  );
}

//Context 사용
function GrandParent() {
  return <Parent />;
}
function Parent() {
  return <Child />;
}
function Child() {
  return <FirstChild />;
}
function FirstChild() {
  return <Message />;
}
function Message() {
  ***const contextValue = useContext(myContext);***
  return <div>전달 받은 데이터 : {contextValue}</div>;
}

```

### 실습코드2 - Props To Context

- Props 코드를 Context로 변경
    
    ```jsx
    //Props 사용
    function MyApp3(){
       return(
          <UrecaComponent value="오늘은 화요일입니다!"/>
       );
    }
    
    function UrecaComponent({value}){
       return(
          <div>
              <First value={value}/>
              <Second value={value}/>
              <Third value={value}/>
          </div>
       );
    }
    
    function First({value}){
        return (<div>첫번째 컴포넌트: {value}</div>);
    }
    function Second({value}){
        return (<div>두번째 컴포넌트: {value}</div>);
    }
    function Third({value}){
        return (<div>세번째 컴포넌트: {value}</div>);
    }
    
    export default MyApp3;
    ```
    

- 변경 코드

```jsx
import { createContext, useContext } from "react";

//create Context
const MyContext = createContext();

export default function MyApp3() {
  return (
    <MyContext.Provider value="123">
      <UrecaComponent />
    </MyContext.Provider>
  );
}

function UrecaComponent() {
  return (
    <div>
      <First />
      <Second />
      <Third />
    </div>
  );
}

function First() {
  return (
    <MyContext.Consumer>
      {(value) => <div>첫번째 컴포넌트 : {value}</div>}
    </MyContext.Consumer>
  );
}
function Second() {
  const value = useContext(MyContext);
  return <div>두번째 컴포넌트 : {value}</div>;
}
function Third() {
  const value = useContext(MyContext);
  return <div>세번째 컴포넌트 : {value}</div>;
}

```

### 실습코드3 - 불필요한 재렌더링 방지

- 몽땅 재렌더링
    
    ```jsx
    
    import { createContext, useContext, useMemo, useState } from "react";
    
    const CounterContext = createContext(); //카운터 컨텍스트
    
    function CounterProvider({children}){
        const counterState = useState(0);// 카운터 상태 관리 = counterState안에는 [counterState, setCounterState] 담겨있음
        // const [counterState, setCounterState] = useState(0)과 같음
        return (
           <CounterContext.Provider  value={counterState}>
            {/* [,]이케 받아왔으면 <CounterContext.Provider value={[counterState, setCounterState]}> 형태로 넘겨야 함 */}
              {children}    
           </CounterContext.Provider> 
        );
    
    }
    
    //숫자출력 컴포넌트
    function CounterPrint(){
    //    const [counter,setCounter] =  useContext( CounterContext );
       const [counter] =  useContext( CounterContext );
       return (
             <h1>{counter}</h1>
       );
    }
    
    //버튼(액션) 컴포넌트
    function Buttons(){
        // const [counter, setCounter] =  useContext( CounterContext );
        const [,setCounter] =  useContext( CounterContext );
    
        ***// 이렇게 사용할 경우 counter가 바뀌어도 버튼까지 싹 다 재렌더링됨
        const increase = ()=> setCounter((prev)=> prev +1) ;
        const decrease = ()=> setCounter((prev)=> prev -1) ;***
    
        return (
          <div>
            <button onClick={increase}>더하기</button>
            <button onClick={decrease}>빼기</button>
          </div>
        );
    }
    
    //앱 컴포넌트
    function MyButtonCounter(){
        return (
            <CounterProvider>
                <div>
                     <CounterPrint/>
                     {/* <Buttons/> */}
                </div>   
            </CounterProvider>
        );
    }
    
    export default MyButtonCounter;
    ```
    

⇒  *** useMemo를 사용해서 변경사항이 있는 컴포넌트만 재렌더링 되도록 **

**useMemo**: `useMemo`는 메모이제이션을 통해 성능을 최적화하는 Hook입니다. 특정 연산을 필요할 때만 실행하고, 값이 변경되지 않으면 이전 결과를 재사용합니다. `useMemo`는 값의 불필요한 재계산을 방지하여 렌더링 성능을 향상시킵니다.

- `useMemo`는 의존성 배열(`[]`)이 변경되지 않는 한 `actions` 객체를 재사용합니다. 아래 코드의 경우, 의존성 배열이 빈 배열(`[]`)이므로, 컴포넌트가 처음 렌더링될 때 한 번만 객체가 생성되고, 이후에는 재사용됩니다.

```jsx
// MyButtonCounter.jsx

import { createContext, useContext, useMemo, useState } from "react";

//컨텍스트 2개를 만드는 이유 : 서로 독립된 영역을 갖기 위하여 (재랜더링,재배열 방지)
const CounterContext = createContext(); //카운터 컨텍스트
const ButtonActionContext = createContext(); //버튼액션 컨텍스트

function CounterProvider({children}){
    const [counter, setCounter] = useState(0);// 카운터 상태 관리

    const actions = useMemo(
        ()=>({
            increase(){
                setCounter((prev)=> prev +1);
            },
            decrease(){
                setCounter((prev)=> prev -1);
            },
        }),
        []
    );

    return (
       <CounterContext.Provider  value={counter}>
         <ButtonActionContext.Provider value={actions}>
              {children}    
          </ButtonActionContext.Provider> 
       </CounterContext.Provider> 
    );

}

//숫자출력 컴포넌트
function CounterPrint(){
    console.log('CounterPrint');
//    const [counter,setCounter] =  useContext( CounterContext );
   const counter =  useContext( CounterContext );
   return (
         <h1>{counter}</h1>
   );
}

//버튼(액션) 컴포넌트
function Buttons(){
    console.log('Buttons');
    // const [counter, setCounter] =  useContext( CounterContext );
    const actions =  useContext( ButtonActionContext );

    // const increase = ()=> setCounter((prev)=> prev +1) ;
    // const decrease = ()=> setCounter((prev)=> prev -1) ;

    return (
      <div>
        <button onClick={actions.increase}>더하기</button>
        <button onClick={actions.decrease}>빼기</button>
      </div>
    );
}

// function CounterProvider({children}){
//     const counterState = useState(0);// 카운터 상태 관리
//     return (
//        <CounterContext.Provider  value={counterState}>
//           {children}    
//        </CounterContext.Provider> 
//     );
// }

//앱 컴포넌트
function MyButtonCounter2(){
    return ( 
        <CounterProvider> 
            <div>
                 <CounterPrint/>
                 <Buttons/>
            </div>   
        </CounterProvider>
    );
}

export default MyButtonCounter2;
```

### 실습 미션
<img width="294" alt="image" src="https://github.com/user-attachments/assets/b60d35be-5712-4cc7-bfc9-ff1438f3f6d6" />

- 내 코드 : 위 실습 코드3처럼 useMemo 써서 하려고 하니까 대빵 복잡해짐
    
    ```jsx
    import { createContext, useContext, useMemo, useState } from "react";
    
    const NumberContext = createContext(); //숫자 컨텍스트
    const OperationContext = createContext(); //연산 액션 컨텍스트
    const ResultContext = createContext();
    
    function CalProvider({ children }) {
      const [firstNum, setFirstNum] = useState(0);
      const [secondNum, setSecondNum] = useState(0);
      const [oper, setOper] = useState("+");
      const [result, setResult] = useState(0);
    
      const operations = useMemo(
        () => ({
          "+": () => setResult(parseInt(firstNum) + parseInt(secondNum)),
          "-": () => setResult(parseInt(firstNum) - parseInt(secondNum)),
          "*": () => setResult(parseInt(firstNum) * parseInt(secondNum)),
          "/": () => setResult(parseInt(firstNum) / parseInt(secondNum)),
        }),
        [firstNum, secondNum] // 의존성 배열에 firstNum과 secondNum을 추가
      );
    
      return (
        <NumberContext.Provider
          value={{ firstNum, setFirstNum, secondNum, setSecondNum }}
        >
          <OperationContext.Provider value={{ oper, setOper, operations }}>
            <ResultContext.Provider value={result}>
              {children}
            </ResultContext.Provider>
          </OperationContext.Provider>
        </NumberContext.Provider>
      );
    }
    
    function Header(props) {
      return <h1>{props.title}</h1>;
    }
    
    function Body() {
      const { setFirstNum, setSecondNum } = useContext(NumberContext);
      const { oper, setOper, operations } = useContext(OperationContext);
    
      return (
        <>
          <input
            type="text"
            onChange={(event) => setFirstNum(event.target.value)}
          />
          <select onChange={(event) => setOper(event.target.value)} value={oper}>
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
          </select>
          <input
            type="text"
            onChange={(event) => setSecondNum(event.target.value)}
          />
          <button onClick={operations[oper]}>계산</button>
        </>
      );
    }
    
    function CalcResult() {
      const { firstNum, secondNum } = useContext(NumberContext);
      const { oper } = useContext(OperationContext);
      const result = useContext(ResultContext);
      console.log(result);
      return (
        <div>
          결과 : {firstNum} {oper} {secondNum} = {result}
        </div>
      );
    }
    
    export default function CalApp() {
      return (
        <>
          <Header title="초간단 계산기" />
          <CalProvider>
            <Body />
            <CalcResult />
          </CalProvider>
        </>
      );
    }
    
    ```
    
- 강사님 코드
    - 계산기 App / header / body/ result 컴포넌트로 쪼갬
    - Provider를 App 파일에서 정의해서 사용
    
    ```jsx
    [CalcHeader.jsx]
    export default function CalcHeader(props) {
      return <h1>초간단계산기</h1>;
    }
    ```
    
    ```jsx
    [CalcBody.jsx - 초기 상태!! 컨택스트 추가 전]
    import { useContext } from "react";
    import CalcContext from "./CalcContext";
    
    export default function CalcBody(props) {
      //submit 이벤트 발생 시 실행되는 콜백 함수 - 파라미터로 이벤트 정보가 자동 전달됨
      function handleSubmit(event) {
        console.log("submit event: ", event);
        // console.log("첫번째 폼 구성원 input: ", event.target[0].value);
        console.log("첫번째 폼 구성원 input: ", event.target.firstNum.value);
        console.log("두번째 폼 구성원 select: ", event.target.oper.value);
        console.log("세번째 폼 구성원 input: ", event.target.secondNum.value);
    
        //폼 제출 후 action 동작(현재페이지 리로드)을 막고 event 콘솔 로그 확인
        // event.preventDefault();
    
        const firstNum = event.target.firstNum.value;
        const oper = event.target.oper.value;
        const secondNum = event.target.secondNum.value;
    
        let result;
    
        if (oper === "+") result = firstNum + secondNum;
        else if (oper === "-") result = firstNum - secondNum;
        else if (oper === "*") result = firstNum * secondNum;
        else result = firstNum / secondNum;
    
        const resultStr = `${firstNum} ${oper} ${secondNum} = ${result}`; //result를 스트링으로 설정
    
      }
    
      return (
        <div>
          <form onSubmit={handleSubmit}>
            <input type="text" size="4" name="firstNum" />
            <select name="oper">
              <option value="+">+</option>
              <option value="-">-</option>
              <option value="*">*</option>
              <option value="/">/</option>
            </select>
            <input type="text" size="4" name="secondNum" />
            <button>계산</button>
          </form>
        </div>
      );
    }
    
    ```
    
    ```jsx
    [CalcResult.jsx - 초기 상태]
    export default function CalcResult(props) {
      return <div>결과 : </div>;
    }
    ```
    
    - 계산 결과에 대해서 어떠한 컴포넌트에도 종속되지 않는 컨택스트를 만들기 위해 파일 따로 파서 생성
    
    ```jsx
    //계산기에 필요한 Context
    //어떠한 컴포넌트에도 종속되지 않는 컨택스트를 만들거얍
    
    import { createContext } from "react";
    
    const CalcContext = createContext(); //공유 컨택스트명 : CalcContext
    
    export default CalcContext;
    
    ```
    
    - Body부분에 컨택스트 사용 코드 추가 + App 파일에 Provider 추가
    
    ```jsx
     [CalcBody.jsx]
     //컨택스트 사용
      const printResult = useContext(CalcContext); //Provider에 전달되는 value는 함수니까!!
    
    const resultStr 아래에
    printResult(resultStr); //컨택스트 : printResult 함수
    
    [MyCalcApp.jsx]
    import CalcHeader from "./CalcHeader";
    import CalcBody from "./CalcBody";
    import CalcResult from "./CalcResult";
    import CalcContext from "./CalcContext";
    import { useState } from "react";
    
    export default function MyCalcApp(props) {
      const [result, setResult] = useState(""); //result는 string!
    
      function printResult(resultStr) {
        setResult(resultStr);
      }
    
      return (
        <div>
          <CalcHeader />
          <CalcContext.Provider value={printResult}>
            <CalcBody />
            <CalcResult result={result} />
          </CalcContext.Provider>
        </div>
      );
    }
    
    ```
