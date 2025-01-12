## Elements란?

리액트 앱을 구성하는 가장 작은 블록들

화면에 보이는 것을 기술해줌

- 리액트 Elements는 자바스크립트 객체(Object) 형태로 존재

```jsx
React.createElement(
	type,  <<객체도 타입으로 들어갈 수 있음
	[props],
	[..자식 요소 children]
	
	

{
	type:"button" *<button>태그를 의미
	type:Button  *Button으로 정의된 객체가 있어야 정상 동작
}

function Button(props) {
	return (
		<button className={`bg-${props.color}`}
			<b> {props.children} </b>
		</button>
	)
}
```

- 불변성
    - Elements 생성 후에는 children이나 attributes를 바꿀 수 없다
    <img width="693" alt="image" src="https://github.com/user-attachments/assets/9f215401-6b6c-4590-aecc-238503d973a9" />

<img width="696" alt="image" src="https://github.com/user-attachments/assets/b490e8e3-1449-43ed-a102-ab294b12bdd2" />

리액트 가상돔에서 중간 연산 or 변경을 우다다 하고 사용자 브라우저 돔에는 최종 상태로만 변경 시켜줌

⇒ 백그라운드에서 연산들이 일어나는 것과 같아서 효율적이고 빠름  

### Elements 렌더링

Root DOM Node : 최종상태를 보여줄 돔 노드

```jsx
<div id="root"></div>
```

- render

```jsx
index.html

const element = <h1>안녕 리액트</h1>
ReactDOM.render(element, document.getElementById("root"))
```

- 렌더링된 Elements를 업데이트하기

```jsx
*불변성 : 기존 엘리먼트를 변경하는게 아니라 새로 생성해서 갈아끼우는거임

function tick() {
	const element = (
		<div>
			<h1>안녕</h1>
			<h2>현재시간 : {new Date().toLocaleTimeString()}</h2>
		</div>
	)
	
	ReactDOM.render(element, document.getElementById("root"))
}	
	
//콜백함수로 호출
setInterval(tick, 1000)
```

- 실습코드 - 시계 만들기

```jsx
[Clock.jsx]
export function Clock() {//컴포넌트는 첫글자를 대문자로 -> HTML태그가 될 애라서 좀 명시할라고
    return (
        <div>
            <h1>안녕 리액트</h1>
            <h2>현재시간: {new Date().toLocaleTimeString()}</h2>
        </div>
    )
}

----------------------------------------------------------------------------------------
[index.js]
import Clock from "./Clock"

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    {/* <App /> */}
    <Clock/>
  </React.StrictMode>
);

//1초 간격으로 render수행하게 해서 현재 시간을 업데이트 시키고 싶음
const root = ReactDOM.createRoot(document.getElementById('root'));

setInterval(()=>
  root.render(
    <React.StrictMode>
      {/* <App /> */}
      <Library/>
      <Clock/>
    </React.StrictMode>
  )
)
//1초 간격으로 render 수행
```

<aside>
💡 export - import할 때
1. named export된 모듈을 임포트할때는 반드시 중괄호에 넣어서 가져와야 함

```jsx
export function f1() { }

-> import {f1} from "path"
```

1. export default로 내보낼때는 중괄호에 넣지 않고 임포트!

```jsx
function f1() { }
export default f1
	-> import f1 from "path"
```

<aside>
💡 구글링

- export default : import할 때 export한 이름이 아니여도 가능하다. but, **이름을 일치시키는 것을 권장**
- named export : **반드시 export한 이름으로만 import** 해야한다.
</aside>

</aside>

### 컴포넌트와 Props

- 컴포넌트
    <img width="536" alt="image" src="https://github.com/user-attachments/assets/e66b56ff-9e4a-4afa-8837-da4f2cb4476f" />

    - 리액트는 컴포넌트 기반 구조
    - 함수 또는 클래스로 컴포넌트를 구현 (일반적으로 함수로 구현)
        
        → props를 변경하면 안되고 동일 props가 들어가면 동일한 결과를 내보내야 함
        
    - 모든 페이지가 컴포넌트로 구성되어 있고, 하나의 컴포넌트는 또 다른 컴포넌트들의 조합으로 구성될 수 있음
        - 컴포넌트 합성 : Component 안에 또 다른 Component를 쓸 수 있다!
        <img width="394" alt="image" src="https://github.com/user-attachments/assets/ef62ea6a-1d30-4ba0-a41d-d86bb2d69479" />

        ```jsx
        function Welcome(props) {//자식
        	return <h1>Hello, {props.name}</h1>
        }
        
        function App(props) {//부모
        	return (
        		<div>
        			<Welcome name="Mike"/>
        			<Welcome name="Mike"/>
        		</div>
        	)
        }
        
        ReactDOM.render( <App/>, document.getElementById('root'))
        ```
        
    
    <aside>
    💡 컴포넌트 이름은 항상 대문자로 시작
    
    </aside>
    
    - 컴포넌트 추출 → 재사용성 ⬆️, 개발속도 ⬆️
    

- Props = 속성
    - 컴포넌트라는 함수에 들어갈 인자라고 생각하면 됨
    - props 속성을 넣으면 해당 속성에 맞춰 화면에 나타날 엘리먼트가 생성됨
    - 리액트 컴포넌트에서 글자 색깔이나 배경 색같은 속성을 바꾸고 싶을 때 사용

### 컴포넌트 실습 코드

world-con > src > 

- comment.jsx

```jsx
export function Comment(props) {
    return (
        <div style={styles.wrapper}>
            {/* 이미지 컨테이너*/}
            <div style={styles.imageContainer}>
                <img src="https://upload.wikimedia.org/wikipedia/commons/8/89/Portrait_Placeholder.png"
                      style={styles.image}/>
            </div>

            {/* 댓글 내용 */}
            <div style={styles.contentContainer}>
                {/* <span>제가 만든 첫 컴포넌트입니다.</span> */}
                <span style={styles.nameText}>**{props.name}**</span>
                <span style={styles.commentText}>**{props.comment}**</span>
            </div>
            
        </div>
    )
}
```

- commentList.jsx

```jsx
import {Comment} from "./Comment"

//배열로 이름&댓글 목록 정의하기
const comments=[
    {
        name:"김현정",
        comment:"ㅎㅇ염"
    },
    {
        name:"김찬별",
        comment:"킥킥"
    },
    {
        name:"이민주",
        comment:"집가고싶다,,"
    },
    {
        name:"최우진",
        comment:"외딴섬 외로워요"
    }
]

export function CommentList(props) {
    return (
        <div>
            {/* <Comment/>
            <Comment/>
            <Comment/> */}

            {//자바스크립트 영역
                //배열.map() : 배열의 요소 수만큼 반복하면서 각 요소를 변환하는 값을 리턴
                comments.map((comment) => {
                    return (**<Comment name={comment.name} comment={comment.comment}/>**)
                })
            }
        </div>
    )
}
```

- index.js

```jsx
import {CommentList} from "./CommentList"

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <CommentList/>
  </React.StrictMode>
)
```
