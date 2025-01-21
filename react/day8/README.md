<11주차수업>

- ~~엘리멘트 변수, 인라인 조건, 컴포넌트 렌더링 막기~~
- ~~리스트, 키 개념, 다수의 컴포넌트 렌더링~~
- ~~폼의 개념, 제어 컴포넌트, 사용자 입력 다루기~~
- ~~Shared State, State 공유하기~~
- **합성 개념, Card 컴포넌트**

---

## 지난 수업 미션

- **내 코드**
    
    ```jsx
    import React, { useEffect, useRef, useState } from "react"
    
    export default function Guess3(props) {
        const [number, setNumber] = useState(parseInt(Math.random()*100))
        console.log(number)
        
        const [history, setHistory] = useState([])
        const [count,setCount] = useState(0)
        const [success, setSuccess] = useState(false)
        const [record, setRecord] = useState([])
        const [value, setValue] = useState("")
    
        const input = useRef()
        
    
        useEffect(()=>{
            setCount(count+1)
        },[])
        
        function answer() {
            console.log("val : ", value)
            if(number > value) {
                const his = { id : value, ans : "Up" }
                setHistory([...history, his])
            } 
            else if(number < value) {
                const his = { id : value, ans : "Down" }
                setHistory([...history, his])
            } 
            else {
                const his = { id : value, ans : "정답입니다" }
                setHistory([...history, his])
                setSuccess(true)
                
            }
    
            //정답 입력 후 인풋 칸 클리어 해주는 용도
            setValue("")
        }
    
        function enter(e) {
            if(e.key === "Enter") {
                answer()
            }
        }
    
        const handleChangeValue = (event)=>{
            console.log(event.target.value)
            setValue(event.target.value)
        }
    
        function newGame() {
            setHistory([])
            setCount(count+1)
            setNumber(parseInt(Math.random()*100))
            setSuccess(false)
        }
    
        useEffect(()=>{
            if(success) {
                let prev = [...record, {try : history.length, ans : number}]
                setRecord(prev)
            }
        },[success])
    
        return(
            <div>
                <h1>숫자 맞추기</h1>
                <p>1 ~ 100사이 숫자 맞춰보세요</p>
                <p>{count}번째 도전중</p>
                <p>{history.length > 10 && <>답은!!! {number}입니다</>}</p>
                <input type="number" value={value} onChange={handleChangeValue} onKeyDown={enter}/> 
                <button onClick={answer} disabled={success || !value}>정답확인</button>
                <button onClick={newGame} disabled={!success}>새 게임</button>
                
                <p>
                {/* 첫 시작에는 확인결과 div가 안보이게 하고싶어 => inline if (boolean && 표현식) 활용 */}
                {history.length > 0 && (<>확인결과 &gt;&gt;&gt; {count}번째 시도 : [
                    {history[history.length - 1].id}] {history[history.length - 1].ans} </>)}
                </p>
                <p>
                    <ul>
                        {
                            history.map((obj, index) => {
                                // console.log(obj)
                                return <li key={index}>{index+1}번째 시도 : [{obj.id}] {obj.ans}</li>
                        })}
                    </ul>
                </p>
                <p>
                {record.length > 0 && <>
                이전 시도 :
                <ul>
                    {
                        record.map((obj, index) => {
                            return <li key={index}>{obj.try}번째에 성공 : {obj.ans} </li>
                        })
                    }
                </ul>
                </>}
                </p>
            </div>
        )
    }
    ```
    
- count를 게임 시도 누적 횟수로 변경
    - useEffect 사용하여 마운트 시 +1, 새게임 수행할 때 setCount(count+1)
- input을 ref로 설정하니까 바로바로 업데이트가 안되는 단점이 있었음 새게임하고 정답 확인하려 하면 undefined로 떴음 흠.. 그래서 그냥 state로 설정함
    - onChange 설정
- 성공 여부에 따라 버튼 활성화를 하게 하려고 success state 추가
- 시도 횟수가 10번 이상이면 답 노출되도록 하려고 `<p>{history.length >= 10 && <>답은!!! {number}입니다</>}</p>` 코드 추가

```jsx
<p>
{/* 첫 시작에는 확인결과 div가 안보이게 하고싶어 => inline if (boolean && 표현식) 활용 */}
{history.length > 0 && 
	(<>확인결과 &gt;&gt;&gt; {count}번째 시도 : [{history[history.length - 1].id}] 
													{history[history.length - 1].ans} </>)}
</p>
```

- 여기서 기존 코드는 history[count-1] 이런식으로 count를 사용했는데, 이제 카운트는 누적 횟수니까 새게임으로 돌아가면 없는 인덱스를 참조하려 하니까 에러 났었음
    - 배열 길이를 사용해서 참조하도록 변경

- **강사님 답안 코드**
    
    ```jsx
    import { useEffect, useState } from "react";
    
    function MyNumberGuess4(props){
        
        //난수 발생 
       const  [com_num, setCom_num] = useState(Math.floor( Math.random()*100 + 1 ));  //1~100
    
       //사용자가 입력한 데이터를 (상태)관리
       const  [user_num, setUser_num] = useState("");
       const  [result, setResult] = useState("");
    
       const  [tryCount,   setTryCount  ]= useState(0); //시도횟수
       const  [tryHistory, setTryHistory]= useState([]); //시도이력
       
       const [newGameDisable, setNewGameDisable ] = useState(true); //새게임 유무
       const [gameDone, setGameDone ] = useState(false); //정답확인버튼 (게임끝 여부)
       
       const [answerHistory, setAnswerHistory]= useState([]); //정답이력
       const [gameCnt, setGameCnt] = useState(1);//게임 횟수
    
       useEffect(()=>{
         setTryCount(tryCount+1);
       },[]);
       
        
    
       function checkNum(){
          console.log("com_num=", com_num, ", user_num=", user_num);
    
          let historyStr = `${tryCount}번째 시도 [${user_num}]: `;
          //값비교  ==결과==> 낮춰주세요!/높여주세요!
          //기준값 ==> 난수를 발생
          if(user_num > com_num){//사용자 입력값이 높을때
            historyStr=(`${historyStr} 낮춰주세요!`);
          }else if(user_num < com_num){//사용자 입력값이 낮을때
            historyStr=(`${historyStr} 높여주세요!`);
          }else{//정답
            historyStr=(`${historyStr} 정답입니다^O^`);
            setNewGameDisable(false);//새게임버튼 활성화
            setGameDone(true); //게임끝(정답맞춤)
            setAnswerHistory([...answerHistory,`${gameCnt}번째 정답: ${com_num} `]);
            setGameCnt(gameCnt+1);
          }
          setUser_num("");//사용자 입력값 지우기
          setTryCount(tryCount+1);//시도 횟수 증가
          setResult(historyStr);
          setTryHistory([historyStr, ...tryHistory]); //시도한 결과 문자열을 배열에 저장(히스토리 남김)
    
          if(tryCount===11){
            alert('10번의 시도가 끝났습니다.');
    
            setAnswerHistory([...answerHistory,`${gameCnt}번째 정답: ${com_num} `]);
            setGameCnt(gameCnt+1);
            newGame();
          }
       }//checkNum
    
       
    
       function handleChange(event){//HTML마크업의 변경된 값을 state에 반영
          setUser_num(event.target.value);
       }
    
       function handleKeyDown(event){ //콜백함수 ==> 매개변수에 이벤트 관련정보가 default 전달됨!!
         console.log("handleKeyDown called", event);
    
        //  key: "Enter"
        //  keyCode : 13
    
        // if(event.keyCode === 13)
        if( !gameDone  &&  event.key === 'Enter'){//게임을 진행중이라면  &&  'Enter'
            checkNum();
        }
       }//handleKeyDown
    
       function newGame(){
            setUser_num("");//사용자 입력값 지우기
            setTryCount(1);//시도 횟수 초기화
            setTryHistory([]); //히스토리 배열 초기화
    
            setCom_num(Math.floor( Math.random()*100 + 1 ));   //새로운 com숫자 적용
    
            setNewGameDisable(true);//새게임버튼 비활성화
            setGameDone(false);//정답확인 버튼 활성화
    
       }//newGame
    
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
                <button onClick={checkNum} disabled={gameDone}>정답확인</button>
                <button onClick={newGame}  disabled={newGameDisable} >새게임</button>
                {/* <인라인 if>
                형식) 조건식 && 표현식
                      true  ==> 표현식
                      false ==> false */}
                <div>{ (tryHistory.length > 0 ) && (<> 확인결과▶ {result} </>)}</div>
                <div>
                    <ul>
                        {tryHistory.map((h,idx)=> <li key={idx}>{h}</li> )}
                    </ul>
                </div>
                <div style={{color:"blue"}}>{
                    (answerHistory.length >0) && (<>
                    <h2>정답히스토리</h2>
                    <ul>
                        {answerHistory.map( (answer,idx)=> <li key={idx}>{answer}</li>)}
                    </ul>
                    </>)
                   }
                </div>
            </div>
        );
    }
    
    export default MyNumberGuess4;
    ```
    

---

# Composition vs. Inheritance

## 합성

여러개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것

### 1️⃣ **Containment (= 포함)**

- sidebar같은 Box형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없다
    
    ⇒ Props 내 존재하는 **Children 프로퍼티**를 사용해서 조합!
    

```jsx
React.createElement(
	type,             //type = 태그명
	[props],          //속성 정의
	**[...children]**     //type 내 하위 태그들
}
```

```jsx
function FancyBorder(props) {
	return (
		<div className={'FancyBorder FancyBorder-' + props.color}>
			{props.children}
		</div>
	)
}

* props.color : 개발자가 정의한 속성
* children : 디폴트 속성
```

### **Containment** 실습코드

FancyBorder컴포넌트 안에 있는 하위 태그들은 children 속성으로 전달됨!

- children으로 하위컴포넌트 전달
    
    ```jsx
    function FancyBorder(props) {
        return (
            <div className={'FancyBorder FancyBorder-'+props.color}>
                {props.children}
            </div>
        )
    }
    
    export default FancyBorder
    ```
    
- 상위 컴포넌트
    
    ```jsx
    import FancyBorder from "./Children_test";
    import "./welcome.css"
    
    function WelcomeDialog(props) {
        return (
            <FancyBorder color="blue">
                <h1 className="Dialog-title">h1 child</h1>
                <p className="Dialog-mesage">p child</p>
            </FancyBorder>
        )
    } 
    
    export default WelcomeDialog
    ```
    
- CSS
    
    ```jsx
    .FancyBorder {
        padding: 10px 10px;
        border: 10px solid;
      }
      .FancyBorder-blue {
        border-color: blue;
      }
      .Dialog-title {
        margin: 0;
        font-family: sans-serif;
      }
      .Dialog-message {
        font-size: larger;
      }
    ```
    

### **※ Props로 컴포넌트를 전달할 수 있다!**

```jsx
import "./day0819.css"

function Contacts() {
    return <div className="Contacts" />;
  }
  
function Chat() {
    return <div className="Chat" />;
}

function SplitPane(props) {
    return (
      <div className="SplitPane">
        <div className="SplitPane-left">
          {props.left}
        </div>
        <div className="SplitPane-right">
          {props.right}
        </div>
      </div>
    );
}

function App2(props) {
    return (
      <SplitPane
        left={ <Contacts /> }
        right={ <Chat />    } 
      />
    );
}

export default App2;
```

### 2️⃣ **Specialization**

범용적인 개념을 구별이 되게 구체화 하는 것

- 기존 객체지향 언어라면 상속을 통해 구현했겠지만, 리액트에서는 **합성을 사용하여 구현**

### 3️⃣ Containment + Specialization 함께 사용
<img width="726" alt="image" src="https://github.com/user-attachments/assets/7dd65514-2c77-4397-b716-c7be5f3f2c2f" />

- 그냥 용어 구별만 하고 props를 통해 하위 컴포넌트를 전달하거나 속성값을 설정한다는 거만 이해하기

## 상속

다른 컴포넌트로부터 상속을 받아서 새로운 컴포넌트를 만드는 것

- 복잡한 컴포넌트를 쪼개서 여러개의 컴포넌트로 만들고, 걔네를 조합해서 새로운 컴포넌트로 만들자!
- 결국엔 상속도 합성이구만
