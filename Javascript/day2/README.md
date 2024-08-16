### HTML DOM

- Node(조작) - 최상위 부모 객체
    - 조작(추가, 삭제, 수정) / 관계(부모, 형제, 자식) 정의됨
- HTML Documents (생성)
    - getElement~ : 하나의 엘리먼트 반환
    - getElement**s**~ : 해당하는 엘리먼트 모두 들어있는 배열로 반환 → 인덱스로 접근
- HTML Elements

---

### 교재 - ch3 자바스크립트

[2부-03장-자바스크립트.pdf](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/734cf85f-de10-4a10-a7aa-73580582a7a1/2%EB%B6%80-03%EC%9E%A5-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8.pdf)

- 자바스크립트 특징
    - 스크립트 언어 : 프로그램 실행 시 기계어로 변환 (↔ 컴파일 언어)
    - 이벤트 드리븐 방식 : 이벤트에 반응하여 동작을 변경하거나 명령을 수행
        - 이벤트 = 대상(엘리먼트) + 트리거(이벤트 발생) + 핸들러/리스너(이벤트 내용)
    - ECMAScript
    
- 자바스크립트 연결하기

```jsx
자바스크립트 함수 == 일급객체 
	=> 클래스와 어깨를 나란히 함

* 일급객체의 특성
	1. 변수에 할당할 수 있음
	2. 리턴타입으로 설정할 수 있음
	3. 매개변수로 사용 가능
	
	
* 대신! 익명함수 형태로 사용
	var ureca = function() { }
	호출 : ureca()
```
* HTML 이벤트 속성을 이용한 연결
   ⇒ 높은 결합도 근데 요즘 이걸로 다시 돌아가는 느낌이라고 하셨음 0_0..

* <script></script>요소에서의 연결
html js 분리 ⇒ 낮은 결합도

- 자바스크립트는 명령형 프로그래밍 / 선언형 프로그래밍 모두 지원
    
    - 명령형 : 명령어의 나열
    - 선언형 : 실행할 프로그램 선언
    

### ch4 변수와 데이터 타입

- 변수
    - named storage : 식별자 + 저장소
    - 대소문자 구별
    - 스칼라 변수(변수 하나에 값 하나) ↔ 오브젝트 변수(참조변수 느낌?)

- 호이스팅
    - 변수 선언(값 할당X)이 올라간다는 의미임 **
    
    ```jsx
     console.log(ureca) => undefined 출력 (호이스팅을 통해 선언은 된 상태여서 에러나진 않음)
     var ureca = 'Ureca'
     
     
     console.log(A) => 에러남 (아예 없는 변수)
    ```
    

### 연산자

- ** : 거듭제곱
- === : 엄격한 동등 연산자 → 타입, 내용 비교
- ! == : 엄격한 부정 연산자

### 데이터 타입

- Number : 정수, 소수
- String
- Undefined, Null
- Collection
    - Set
    - Array
    - 연관배열

### 객체

- { } == new Object( )

```jsx
    const myCar = {
      type: '전기차',
      doors: 5,
      color: 'white',
      brand: '슈퍼카',
      start() {//클래스 멤버함수랑 똑같네
        console.log('차량 출발');
      },
      stop: function(msg) {
        console.log(msg + ' 정지');
      }
    }

    console.log(myCar['doors']);
    console.log(myCar.color);
    myCar.start();
    myCar.stop('우리차');
```

---

### ES6 추가 설명

1. let vs. const
- `let`
    - 선언된 블럭에서만 사용 가능한 블록 변수 == 지역변수
    - 호이스팅(선언 끌어올리기) 대상에서 제외 - 블록 안에서만 유효
    
    ```jsx
    <호이스팅>
    var msg = 1;
    if (msg === 1) {
    	var msg = 2;
    	console.log(msg); //msg = 2
    }
    console.log(msg);   //msg = 2
    
    -----------------------------------------------------
    
    <let - 호이스팅 X>
    {
    	let msg = "2";    //호이스팅 X - 블록 안에서만 유효
    	console.log(msg); //msg = 2
    }
    console.log(msg);   //not defined 에러발생함
    ```
    
- `const`
    - 호이스팅(선언 끌어올리기) 대상에서 제외 - 블록 안에서만 유효
    - 상수로 사용 → 값 변경 불가!
    - 선언 시 값을 할당해야함
    
    ```jsx
    const msg1 = '상수';
    msg1 = '상수2'; //에러
    console.log(msg1);
    
    const msg2; //에러
    msg2 = "선언시 값을 할당해야 한다.";
    console.log(msg2);
    ```
    

1. Property 단축

```jsx
const id = ‘ureca',
name = ‘ 유레카',
age = 3;
const member = {
id: id,
name: name,
age: age
}; //==JSON

--> const id = ’ureca',
		name = '유레카',
		age = 3;
		const member = {
			id,
			name,
			age
		}; //변수 이름이 key로 드감

---------------------------------------------
<데이터 저장 방식>
JSON : { pName : '이름' }
XML : <pName>'이름'</pName>
```

1. Concise Method

```jsx
(JSON value에 function 들어갈 수 있음(일급객체))

const member = {
id: id,
name: name,
age: age,
info: function () {
	console.log('info');
	}
};

const member = {
id,
name,
age,

--> info() {
		console.log('info');
	}
}
```

1. For ~ of  (= 자바 for each문) : 자바스크립트 - 3에 정리되어 있음
2. Arrow function(화살표 함수)

```jsx
const func = function ( ) {
console.log("익명 func");
}
호출: func();

--> const func = (파라미터1, ..) => {
	console.log("화살표 func")
}
호출: func();

---------------------------------------
const func = function (num) {
return num * num; 
};
--> 화살표 다음에 return문만 오면 return키워드 생략 가능
	  const func = (num) => num * num;
```

1. Destructuring
- 배열 or 객체에 담겨있는 값들을 개별 변수들에 푸는 방법

```python
const member = {
id: 'aaa',
name: 'bbb',
age: 22,
};

--> let {id, name, age} = member;
```

### Spread Operator : ‘ … ‘

- 반복 가능한(iterable) 객체에 적용할 수 있는 문법
- 배열이나 문자열 등을 요소 하나 하나로 전개시킬 수 있음.

```jsx
let arr1=["길동","라임","주원"];
let arr2=["유신", ...arr1, "감찬"]; // ...arr1 : arr1에 저장된 값을 뿌려줌 => arr2.length = 5
console.log(arr2);

let str1="ssafy";
let str2=[...str1]; //문자 배열
let str3={...str1}; //key = index, value = 문자
```

```jsx
//매개변수에 사용 O -> 가변인자
function sum(...param){
	let i=0;
  for(let num of param){
              i+=num;
  }
  return i;
}
```

---

### 실습 코드

[HTML_DOM.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/87c59545-fbc7-45bb-bbae-0c8f28c00f05/HTML_DOM.html)

[let_const.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/ffb866da-6930-4312-973c-afbd4c3fdfad/let_const.html)

[Spread_operator.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/fc6a3b84-6086-411c-86b4-a66a0d469983/Spread_operator.html)

[object_test.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/d7f666be-beb4-4c59-9622-ba624b978f4a/object_test.html)
