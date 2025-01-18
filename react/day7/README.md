## Form (2)

1. **HTML** > form > submit 발생 시 method, action을 찾음
    - input name태그 꼭 써야함
    - method : 디폴트 → GET
    - action : 디폴트 → 현재 페이지

1. **리액트**에서는 페이지 이동없이 값을 변경해야 하기 때문에 preventDefault도 써줘야 하고 name태그도 필요 없음
    - 걍 div로 쓰는게 나음 form쓰면 헷갈림
    - 사용자의 입력을 직접적으로 제어할 수 있음
    
    ```jsx
    //이름 input에 대해 처리하는 콜백함수
    const handleChange = (event) => {
       console.log(event.target.value) //event.target : 이벤트를 발생시킨 엘리먼트 객체 (=input태그)
       setUserName(event.target.value.toUpperCase()) //input태그 입력값을 대문자로 변경
    }
    ```
    

### TextArea

여러 줄에 걸치는 긴 텍스트를 입력받기 위한 HTML 태그

- 리액트에서는 ***value*** 속성이 필요함!

```jsx
1. HTML
<textarea>
	텍스트
</textarea>

2. 리액트
//출석부 컴포넌트

import { useState } from "react"

function NameForm(props) {
    const [userName, setUserName] = useState("")
    const [memo, setMemo] = useState("메모를 입력하세요.")

    //이름 input에 대해 처리하는 콜백함수
    const handleChange = (event) => {
        console.log(event.target.value) //event.target : 이벤트를 발생시킨 엘리먼트 객체 (=input태그)
        setUserName(event.target.value.toUpperCase())
    }

    //submit에 대한 콜백함수
    const handleSubmit = (event) => {
        //제출된 이름을 alert로 출력
        alert(`입력된 이름 state: ${userName}, 입력한 메모 : ${memo}`)
        event.preventDefault() //새로고침 막아줌
    }

    const handleChange2 = (event) => {
        setMemo(event.target.value)
    }

    return (
        <form action="">
            <label>
                이름
                <input type="text" onChange={handleChange} value={userName}/>
            </label>
            <br/>
            <textarea value={memo} onChange={handleChange2} />
            <button onClick={handleSubmit}>제출</button>
        </form>
    )
}

export default function Form(props) {
    return (
        <>
        <NameForm />
        </>
    )
}
```

### Select 태그

드롭다운 목록을 보여주는 HTML 태그

- onChange : 선택 변경 시 실행될 콜백함수
- value : 관리하는 state 값
    - state값을 보여주면 되니까 html에서 쓰이는 selected 속성 필요없음
    - 배열을 넣어서 여러개의 옵션 선택되도록 할 수 있음

```jsx
const [fruit, setFruit] = useState("grape") //관리할 state 설정

const handleChange3 = (event) => {
        setFruit(event.target.value)
}

<select **value={fruit} onChange={handleChange3}**>
     <option value="apple">사과</option>
     <option value="banana">바나나</option>
     <option value="grape">포도</option>
     <option value="watermelon">수박</option>
</select>
```

### File Input 태그

```jsx
<input type="file" />
```

### multiple inputs

여러개의 state를 선언하여 각각의 입력에 대해 사용

### Input Null Value

- 원래 <input value=”val”> 이렇게 설정하면 val이라는 값은 변경이 안됨
- 근데 null or undefined를 사용하면 바꿀 수 있나봄

### 코드 실습

- 이름, 성별 입력받는 폼

```jsx
import { useState } from "react"

function SignUp(props) {
    const [name, setName] = useState("")
    const [gender, setGender] = useState("female")

    const handleChangeName = (event) => {
        setName(event.target.value)
    }

    const handleSubmit = (event => {
        alert(`이름 : ${name}, 성별 : ${gender}`)
        event.preventDefault() //submit 방지 == 다른 페이지로 이동 방지
    })

    const handleChangeGender = (event)=> {
        setGender(event.target.value)
    }

    return (
        <form onSubmit={handleSubmit}>
            <label> 이름 : 
                <input type="text" value={name} onChange={handleChangeName}/>
            </label>
            <br/>
            <label> 성별 : 
                <select value={gender} onChange={handleChangeGender}>
                    <option value="male">남자</option>
                    <option value="female">여자</option>
                </select>
            </label>
            <br/>
            <button>제출</button>
        </form>
    )
}

export default SignUp
```

## State 끌어올리기

### Shared State

자식 컴포넌트들이 가장 가까운 공통 부모 컴포넌트의 state를 공유해서 사용

⇒ 어떤 컴포넌트의 state에 있는 데이터를 여러 하위 컴포넌트에서 공통적으로 사용하는 경우

### 하위 컴포넌트에서 State 공유하기

```jsx
* 물의 끓음 여부를 알려주는 하위 컴포넌트
function Boiling(props) {
	if(props.celsius >= 100) {
		return <p>물이 끓습니다.</p>
	}
	return <p>물이 끓지 않습니다.</p>
}
	
	
* 상위 컴포넌트
function Calculator(props) {
	const [temperature, setTemperature] = useState('')

	<fieldset>
		<legend>온도를 입력하세요</legend>
		<input value={temperature} onChange={handleChange} />
		<Boiling celsius={pareseFloat(temperature)} />
	</fieldset>
```

### 입력 컴포넌트 추출하기

```jsx
const scaleNames = {
	c : '섭씨',
	f : '화씨'
}

function TemperatureInput(props) {
	const [temperature, setTemperature] = useState('')
	
	const handleChange = (event) => {
		setTemperature(event.target.value)

	<fieldset>
		<legend>온도를 입력하세요(단위: {scaleNames[props.scale]})</legend>
		<input value={temperature} onChange={handleChange} />
	</fieldset>

	}

}

------------------------------------------------------------------------
* 상위 컴포넌트
function Calculator(props) {
	return (
		<div>
			<TemperatureInput scale="c" />
			<TemperatureInput scale="f" />
		</div>
	)
}
```

<aside>
💡 const rounded = Math.round(output * 1000) / 1000
output = 10.12345인 경우 round(10123.45) = 10123 / 1000 = 10.123

⇒ 소수점 3자리에서 반올림되도록 함

</aside>

### Shared State 적용하기

하위 컴포넌트의 state를 공통 상위 컴포넌트로 끌어 올리기

→ 상위 컴포넌트가 하위 컴포넌트들의 state를 관리할 수 있도록!

하위 애들은 props로 값 할당받아지도록

틱택토 게임에서 보드-사각형 관계가 떠오르는군

- 온도 계산기

```jsx
import Celsius from "./Cesius"
import Fahrenheit from "./Fahrenheit"

function Calculator(props) {
    return (
        <div>
            <Celsius />
            <Fahrenheit />
        </div>
    )
}

export default Calculator
```

**기존 코드 짠 방식 (Shared State 적용 X)**

- 섭씨 온도 입력창

```jsx
const { useState } = require("react");

function Celsius(props) {
	const [temperature, setTemperature] = useState("")
	const handleChange = (event)=> {
		setTemperature(event.target.value)
	}

	return (
		<fieldset>
			<legend>
				온도를 입력해주세요(단위 : 섭씨)
			</legend>
			<input type="text" value={temperature} onChange={handleChange} />
		</fieldset>
	)
}

export default Celsius
```

- 화씨 온도 입력창

```jsx
const { useState } = require("react");

function Fahrenheit(props) {
	const [temperature, setTemperature] = useState("")
	const handleChange = (event)=> {
		setTemperature(event.target.value)
	}

	return (
		<fieldset>
			<legend>
				온도를 입력해주세요(단위 : 섭씨)
			</legend>
			<input type="text" value={temperature} onChange={handleChange} />
		</fieldset>
	)
}

export default Fahrenheit
```

부모-자식 컴포넌트 적용

- 온도 입력창 - 부모로부터 온도를 입력받아서 섭씨 or 화씨로 변경

```jsx
const scaleNames = {
	c : '섭씨',
	f : '화씨'
}

function TemperatureInput(props) {

    const handleChange = (event)=>{
        props.onTemperatureChange(event.target.value)
    }

	return (
		<fieldset>
			<legend>
				온도를 입력해주세요(단위 : {scaleNames[props.scale]})
			</legend>
			<input type="text" value={props.temperature} onChange={handleChange} />
		</fieldset>
	)
}

export default TemperatureInput
```

- 온도 계산기

```jsx
import { useState } from "react"
// import Celsius from "./Cesius"
// import Fahrenheit from "./Fahrenheit"
import TemperatureInput from "./TemperatureInput"

function Boiling(props) {
    if(props.celsius >= 100) {
        return <p>물이 끓습니다.</p>
    }
    return <p>물이 끓지 않습니다.</p>
}

function Calculator(props) {
    //자식에 있었던 state 데려옴
    const[temperature, setTemperature] = useState("")
    const [scale, setScale] = useState("c")

    //자식컴포넌트에서 입력이있을때 섭씨 state 변경
    const handleCelsiusChange = (temperature) => {
        setTemperature(temperature)
        setScale('c')
    }

    //자식컴포넌트에서 입력이있을때 화씨 state 변경
    const handleFahrenheitChange = (temperature) => {
        setTemperature(temperature)
        setScale('f')
    }

    function toCelsius(fahrenheit) {
        return((fahrenheit - 32) * 5) / 9
    }

    function toFahrenheit(celsius) {
        return(celsius * 9 / 5) + 32
    }

    function convert(temperature, convertTo) {
        const input = parseFloat(temperature)

        if(Number.isNaN(input)) return ''

        const output = convertTo(input)
        const rounded = Math.round(output * 1000) / 1000
        return rounded.toString()
    }

    const celsius = (scale === "f") ? convert(temperature, toCelsius) : temperature
    const fahrenheit = (scale === "c") ? convert(temperature, toFahrenheit) : temperature

    return (
        <div>
            {/* <Celsius />
            <Fahrenheit /> */}

            <TemperatureInput scale="c" onTemperatureChange={handleCelsiusChange} 
                              temperature={celsius} />
            <TemperatureInput scale="f"  onTemperatureChange={handleFahrenheitChange}
                              temperature={fahrenheit} />

            <Boiling celsius={parseFloat(celsius)}/>
        </div>
    )
}

export default Calculator
```

---

## 실습코드

**<미션>**

MyNumberGuess2  ==> MyNumberGuess3

- 카운트에 useEffect사용해보기 (tryCount의 초기값을 0으로 설정)
- 숫자맞추기 끝나기 전(정답맞추기 전) '새게임' 비활성화 ✅
- 숫자맞추기 끝나면(정답맞추면) '새게임' 활성화 ✅, '정답확인' 비활성화 ✅
- 이전게임의 정답 맞춘 히스토리 추가 → useEffect 사용 🔺
- 10번 시도후 정답 알려주기 ✅
