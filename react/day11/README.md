JS : 문서doc의 일부분을 조작하는 기능을 함

# Router

페이지 이동
<img width="710" alt="image" src="https://github.com/user-attachments/assets/31e80b6e-4c85-4b3b-9350-889d6f97c516" />

- 페이지를 완전히 이동시켜주는 애들
    - <a href=”#”>
    - <form action=”path”>
    - location.href = “path”

- SPA(Single Page Application)은 이동할 페이지가 없음 → 컴포넌트를 갈아끼움으로써 페이지를 이동하는 것처럼 하자
    - 결국 갈아끼워지는 컴포넌트들이 라우터라고 볼 수 있다!

- npm install 라이브러리 설치 필요
    - 설치 안하면 Router랑 style쟤네 못씀

```jsx
npm i react-router-dom styled-components
```

- 사용법

```jsx
<Router>
	// 갈아끼워질 컴포넌트들과 매핑될 Path 열거
	<Route path="/" element={<Main />}></Route>
	<Route path="/product/*" element={<Product />}></Route>
	
	{* 상단에 위치하는 라우트들의 규칙을 모두 확인, 일치하는 라우트가 없는경우 */}
	<Route path="*" element={<NotFound />}></Route>
</Router>
```

<aside>
💡 **BrowserRouter**

- 브라우저 History API를 사용해 현재 위치의 URL을 저장해주는 역할
- 최상위 태그를 감싸준다
- 앱에서 단 하나의 라우터만 사용하여야 한다.

**Routes**

- 여러 Route를 감싸서 그 중 규칙이 일치하는 라우트 단 하나만을 렌더링 시켜주는 역할

**Route**

- path를 통해 URL을 분기시킬 수 있다. 중첩해서 사용할 수 있다.
- <Route>는 path속성에 경로, element속성에는 컴포넌트를 넣어 준다.
- 여러 라우팅을 매칭하고 싶은 경우 URL 뒤에 *을 사용하면 된다.

**Link (< a > 태그와 동일한 역할! 대신 페이지 이동 안하도록)**

- 웹 페이지에서는 원래 링크를 보여줄 때 a태그를 사용한다. 하지만 a태그는 클릭시 페이지를 새로 불러오기때문에 사용하지 않는다.
- Link 컴포넌트를 사용하는데, 생김새는 a태그를 사용하지만, History API를 통해 브라우저
주소의 경로만 바꾸는 기능이 내장되어 있다.
- 문법 :
    - `import { Link } from 'react-router-dom';`
    - `<Link to="Route Path"> 링크명 </Link>`
</aside>

### URL 파라미터 ★

‘ : ‘ 뒤에 있는 애는 경로 변수다!

- /product/:productId
    - 경로변수 productId

- 사용법

```jsx
[RouterApp.jsx]
<Routes>
	<Route path="/product/:productId" element={<Product />}></Route>
</Routes>

-> productId에 URL로 들어오는 인자를 Product 컴포넌트로 전달
```

```jsx
[Product.jsx]
import { useParams } from 'react-router-dom'

const { 경로 변수로 들어오는 파라미터명 } = useParams() 
```

## 쿼리스트링

### useLocation

- hash : 주소의 #문자열 뒤의 값
- pathname : 현재 주소 경로 (쿼리스트링을 제외한 URL)
- search : ? 뒤에 오는 모든 쿼리스트링을 가져옴
- state : 페이지로 이동시 임의로 넣을 수 있는 상태 값
- key : location 객체의 고유 값, 초기값은 default, 페이지가 변경될 때 마다 고유의 값이
생성된다.

```jsx
[Main.jsx]
<Link to="/product/1?search=productName&q=demo&userName=chan#ureca"> 로 라우팅해줬을때

[Product.jsx]
<ul>
   <li>hash : {location.hash}</li>
   <li>pathname : {location.pathname}</li>
   <li>search : {location.search}</li>
   <li>state : {location.state}</li>
   <li>key : {location.key}</li>
</ul>
      
=> hash : #ureca
	 pathname : /product/1
	 search : ?search=productName&q=demo&userName=chan
	 state :
	 key : 5qnjnpv6
```

### useSearchParams

이 훅을 통해서 쿼리스트링을 확인할 수도 있다

- 사용법

```jsx
const [searchParams, setSearchParams] = useSearchParams()
const keyWords = searchParams
const keyword = searchParams.**get("search")**
const userName = searchParams.**get("userName");

=>** keyWords : searchproductNameqdemouserNamechan
	 keyword : productName
	 userName : chan

```

### useNavigate

Link 컴포넌트를 사용하지 않고 다른 페이지로 이동을 해야 하는 경우, 뒤로가기 등에 사용하는 Hook

- replace 옵션을 사용하면 페이지를 이동할 때 히스토리를 남기지 않는다!

```jsx

  const navigate = useNavigate();

<ul>
        <li>
          <button onClick={() => navigate(-2)}>이전이전</button>
        </li>
        <li>
          <button onClick={() => navigate(-1)}>이전</button>
        </li>
        <li>
          <button onClick={() => navigate(1)}>다음</button>
        </li>
        <li>
          <button onClick={() => navigate(2)}>다다음</button>
        </li>
        <li>
          <button onClick={() => navigate("/")}>첫페이지</button>
        </li>
        <li>
          <button onClick={() => navigate("/", { replace: true })}>
            첫페이지 - replace 사용
          </button>
        </li>
</ul>
```

## 교재 마지막장 실습

### 미니 블로그

- 필수 기능
    - 글 목록 조회
    - 글 조회, 작성
    - 댓글 조회, 작성

- 리액트 앱 만들고 src에서 컴포넌트 별 폴더 만들기
    - component
        - list
        - ui
        - page

- Bottom-up 방식으로 작은 부분부터 구현하자!

<aside>
💡 **Top-down   /   Bottom-up**

문제해결이나 의사 결정을 위한 두가지 접근방식

- Top-down 접근방식
    - 큰 그림에서 시작하여 작은 세부 사항으로 내려가는 것. (하향식)
    - 새로운 제품을 개발할 때 제품의 전체적인 목표와 기능을 결정한 다음 세부적인 설계와 구현을 진행
    - 계획과 조직이 잘 이루어진 상황에서 사용
    
- Bottom-up  접근방식
    - 작은 세부 사항에서 시작하여 큰 그림으로 올라가는 것. (상향식)
    - 새로운 아이디어를 창출하거나 기존의 문제에 대한 새로운 해결책을 찾는 상황에서 사용
    - 새로운 마케팅 전략을 개발하는 경우에 고객의 요구/니즈를 파악한 다음, 그에 맞는 전략을 수립
</aside>

<aside>
💡 calc
CSS 코드에서 **`calc()`** 함수를 사용하면 CSS 속성의 값으로 계산식을 지정할 수 있음!!
calc(100% - 80px) : 브라우저 전체 너비에서 80px을 자른 길이 
***** 헉 댑악 연산자 사이에 공백 무조건 넣어줘야 얘가 인식함**

:not
CSS에서 not 연산자를 말하는거군

```jsx
const Wrapper = styled.div`
	* {
		:not(:last-child) {
			margin-bottom : 10px
		}
	}
`

→ :not(:last-child)의 의미는 div의 마지막 자식 요소를 제외한 나머지 엘리먼트들을 지정하는거임
→ 결국 마지막 요소만 빼고 바텀에 마진 10px 들어감
```

</aside>

- Post 컴포넌트가 있고 얘를 리스트로 가지는 PostList가 따로 있어야겠음

<aside>
💡 ※※※ 상위 컴포넌트에서 props로 함수를 넘기면 하위 컴포넌트에서 받을 때 그 이름대로 받아와야 함! ※※※

</aside>
