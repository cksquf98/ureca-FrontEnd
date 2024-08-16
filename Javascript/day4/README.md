<5주차수업>
-문자열 배열 BOM
-비동기 처리 Ajax
-DB와 DBMS이 개념, SQL 기본
-SQL 함수- 단일행 함수, 다중행 함수,그룹함수, Join
-SubQuery,  DDL , 제약조건

---

### 배열과 문자열

- Array
    - 하나의 변수에 여러값을 저장하는 데이터 구조
    
    <aside>
    💡 cf.
    스칼라변수 = 속성값을 저장하는 변수
    - a=10, b=a일때 a=11로 변경 시 b의 값은 변하지 않음
    
    오브젝트 변수 = 주소 저장 변수
    - 가리키는 주소에 담긴게 바뀌면 참조변수들 조회 시 다 바뀐애로 나옴
    
    </aside>
    
    - 배열 생성
    
    ```jsx
    1. 생성자 이용
    	let arr = new Array(data)
    	
    2. 심볼 이용
    	let arr = [data]
    	
    ※ { } : Object
    	 const ob = new Object()
    	 const ob = { }
    ```
    
    - 배열 접근
    
    ```jsx
      const myArray = [
        '문자열',
        25,
        function() {//얘 실행하려면? => myArray[2]()로 호출
          alert('함수');
        },
        function() {
          return '함수';
        },
        setTimeout(function() {//배열 선언과 동시에 콜백함수로 동작 => 실행
          alert('setTimeout');
        }, 3000),
      ];
      
      console.log(typeof myArray); //object
      console.log('myArray[0]', myArray[0]);
      console.log('myArray[1]', myArray[1]);
      console.log('myArray[2]', myArray[2]()); //alert들어있는 함수 호출
      console.log('myArray[3]', myArray[3]()); //'함수' 리턴
      console.log('myArray[4]', myArray[4]);
    ```
    
    - Object와 비교
    
    ```jsx
    const myObject = {
        "user-name": '홍길동',
        age: 25,
        myFunc1: function() {
          alert('함수1');
        },
        myFunc2: function() {
          return '함수2';
        },
        timingEvent: setTimeout(function() {//배열 선언과 동시에 콜백함수로 동작 => 실행
          alert('setTimeout');
        }, 1000)
      }
      
      console.log(typeof myObject);  
      
      //json 데이터값 접근 시 key에 인용부호 붙여야 함 -> "key" ***
      console.log("myObject['name']", myObject['user-name']);
      console.log("myObject['name']", myObject.user-name); //error! 인식못함
      
      console.log("myObject['myFunc1']", myObject['myFunc1']());   //undefined : 리턴값이 없어서
      console.log('myObject.myFunc2', myObject.myFunc2());         //'함수2'리턴값 출력
      
      console.log('myObject.timingEvent', myObject.timingEvent); //이미 배열 선언 시 콜백함수 예약되었으니까 예약 걸지 않고 무시 
      
    
    GPT)  
    **setTimeout(함수, 0)**은 콜백 함수를 다음 이벤트 루프 사이클에서 실행하므로 
    **alert('함수1')**과 **alert('setTimeout')**의 경우 '함수1'이 먼저 실행됩니다.
    setTimeout이 0ms로 설정되더라도 즉시 실행되지 않으며, 다음 이벤트 루프 사이클에서 실행됩니다.
    ```
    
    - 배열 메서드
        - toString() : 자바 toString처럼 배열을 문자열로 반환, 콤마 디폴트로 구분해서 출력
            
             → 오버라이딩 가능
            
             → 그냥 냅다 배열만으로는 (dc.write(arr) 이런식?) 문자열로 출력할 수 없음
            
        - join(’구분자’) : 배열에 구분자를 추가해서 문자열로 출력      ( ↔ split( ))
            
             → 구분자 없이 쓰는 경우 콤마 디폴트
            
    
    ```jsx
    const pets = ['개', '고양이', '앵무새', '햄스터'];
    
      console.log('속성', pets.length)
      console.log('메서드', pets.toString()) //개,고양이, ..
      console.log('메서드', pets.join(';'))  //개;고양이; ..
      console.log('메서드', pets.join(''))  //개고양이앵무새..
    ```
    

---

- 콜백함수 헷갈리는거

### **배열 원소 호출 시 `setTimeout` 콜백 함수의 동작**

배열에서 `setTimeout`의 콜백 함수를 예약한 후 배열의 원소를 호출하면, **콜백 함수는 2번 실행되지 않습니다**. 아래는 이 과정에 대한 설명입니다:

1. **배열의 선언 시 `setTimeout` 예약**:
    - `setTimeout`의 콜백 함수는 **다음 이벤트 루프 사이클에서** 실행되도록 예약됩니다.
2. **배열 원소 호출**:
    - `myArray[4]()` 호출 시 `setTimeout`의 **타이머 ID**가 반환됩니다.
    - `setTimeout`의 **콜백 함수**는 **이미 예약된 상태**로, **단 한 번**만 실행됩니다.
3. **다음 사이클에서**
    - `alert('setTimeout')`이 **한 번만** 나타납니다.
4. **배열 원소 호출과 `setTimeout`의 관계**:
    - 배열 원소의 호출은 **콜백 함수의 실행**과는 무관합니다.
    - `setTimeout` 콜백 함수는 **예약 시점**에서만 실행됩니다.

---

### 배열의 연산 - 내장함수

- Push  /  Pop
    
    [push_pop.html](https://prod-files-secure.s3.us-west-2.amazonaws.com/90f0cea1-2c0a-45ef-8fdd-d99b6da3fa09/9f2881db-8c08-4ef3-be1a-76f2d3223718/push_pop.html)
    

** 코드 직접 짜보기 **

<aside>
💡 <label>태그 쓰는 이유

label for = “id이름” 속성을 통해서 input checkbox와 텍스트를 연결시켜주는 용도

</aside>

- Splice  /  Slice
    - Splice : 기존 요소를 삭제, 교체 또는 추가하는 메서드
    
    ```jsx
    arr.splice(start, delete개수, 추가할요소1, 요소2 ..)
    -----------------------------------------------------------
    
    const pets = ['개', '고양이', '곰', '말'];
    
      pets.splice(2)  //개, 고양이
    
      pets.splice(0, 1);  //고양이 - 0부터 시작해서 1개 삭제
    
      pets.splice(0, 0, '햄스터'); //delete개수 0 = 지우지 않는단말임 & start위치에 '햄스터' 추가
    															 //햄스터, 고양이
    															 
      pets.splice(1, 1, '거북이', '앵무새'); //햄스터, 거북이, 앵무새
    ```
    
    - Slice : 시작부터 마지막 요소를 복사해서 새로운 배열 반환 ⇒ **원본 배열 변경X**
    
    ```jsx
    arr.slice(begin, end) : begin ~ ***(end-1)***번 인덱스까지 복사
    -----------------------------------------------------------
    
    const weeks = ['월', '화', '수', '목', '금']
    
      console.log('[slice()]', weeks.slice())   //전체복사
      
      console.log('[slice()]', weeks.slice(2))  //2인덱스부터 복사 : 수,목,금
      
      console.log('[slice()]', weeks.slice(2, 4)) //수,목
      
      console.log('[slice()]', weeks.slice(-1))   //금
      
      console.log('[slice()]', weeks.slice(1, -1)) //화,수,목(-1번째 직전 원소)
    ```
    
    ```jsx
    07.array-splice-slice2.html
    
     let cities = [];
      const checkboxes = document.querySelectorAll('[name="city"]');
      for (let checkbox of checkboxes) {
        checkbox.onchange = () => {
          if (checkbox.checked) {
            cities.push(checkbox.value);
          } else {
            let index = cities.**indexOf**(checkbox.value);
            if(index !== -1) { //해당값의 인덱스를 못찾은 경우 -1반환 
            // == 체크표시 해제하는 경우
              cities.splice(index, 1);
            }
          }
          setListGroup();
        }
      }
    ```
    
    ```python
    내가 추가한 코드
        let checkedList = [];
    
        function print(checkbox) {
            let index = checkedList.indexOf(checkbox.value);
            if (index !== -1) {
                checkedList.splice(index, 1); // 배열에서 값 삭제
            } else {
                checkedList.push(checkbox.value); // 배열에 값 추가
            }
            
            let str = '<ul>';
            for (let city of checkedList) {
                str += `<li>${city}</li>`;
            }
            str += '</ul>';
    
            document.querySelector('div').innerHTML = str;
        }
    ```
    

- Sort  /  Reverse - 정렬 메서드
    - **Reverse** : 배열의 역방향 순서로 출력하는거군. ***내림차순 정렬이 아님 *****

- ForEach  /  Map
    - forEach() : 배열의 요소를 한 번씩 순회
    - map() : 배열의 요소 순회 + 그 결과로 새로운 배열 생성
    
    ```jsx
     let arr = [1,2,3,'four']
     arr.forEach(function(value, index) {
        console.log('index: '+index+', val: '+value)
     }); 
    
     ------------------------------------------------------------
     
     let newArray1 = numbers.map(function(value, index, numbers) {
        return 10 + value; 
      }); //여기서 number는 array를 말함
      console.log(newArray1);
    
      let newArray2 = numbers.map(loopNumber2);
      //  let newArray2 = numbers.map(loopNumber2()); => 에러!
    
      function loopNumber2(item, index) {
        return 10 * item;
      }
    ```
    
- Filter
    - 배열의 요소를 한 번씩 순회하면서 특정 조건을 통과하는 원소로만 구성된 새로운 배열을 생성
    
    ```jsx
    const remains = arr.filter(function(value, index) {
        return 조건식(ex. value % 2 == 0);
      })
    ```
    
- Reduce
    - 배열의 요소를 각각 한 번씩 순회하면서 실행하는 연산의 결과를 반환
    
    ```jsx
    * 해당 순회 회차에 대한 소계로서 total 변수가 있음
    * 초기값이 있어 다른 연산의 결과를 기초로 계속 연산할 수 있음
    
    const numbers = [1, 3, 5, 7, 9];
    const sum = numbers.reduce(makeSum, 10); //초기 누적값 (total) = 10으로 설정
    
      function makeSum(total, item, index) {
        console.log('total, item, index', total, item, index);
        return total + item;
      }
      
    => sum = 35(10 + 1+3+5+7+9)
    ```
    

---

### 문자열(String)  

- CharAt  /  IndexOf
    - charAt( ) : 특정 위치에 있는 문자 반환
    - indexOf( ) : 파라미터로 들어온 문자가 처음 발견되는 위치 반환

- Include**s** / start**s**With / end**s**With
    - 파라미터로 들어온 문자가 포함되어 있는지 여부를 T/F로 반환
    
    ```jsx
    let poem = `
        그 꽃 (고은)
        
        내려갈 때 보았네
        올라갈 때 보지못한
        그 꽃
      `;
    
      console.log(poem.includes('고은')) //true
      console.log(poem.startsWith('그 꽃')) //false <- whitespace때문에
      console.log(poem.endsWith('그 꽃')) //false <- whitespace
    ```
    

- trim
    - 문자열 앞뒤 공백 제거해줌
    - 문자열 사이의 공백은 제거X
    
    ```jsx
     poem = poem.trim() //앞뒤 공백 제거 후 기존 String에 저장~~~!
      
     console.log(poem.startsWith('그 꽃')) //true
     console.log(poem.endsWith('그 꽃')) //true
    ```
    

- Match  /  Search  /  Replace
    - match : 파라미터로 들어온 문자열이나 정규표현식의 조건으로 매칭되는 값을 배열로 반환
    - search : 파라미터로 들어온 문자열이나 정규표현식의 조건으로 가장 먼저 매칭되는 문자의 위치를 인덱스로 반환
    
    <aside>
    💡 정규표현식
    
    A : A문자가 반드시 한 번 나옴
    A? : 0번 or 1번
    A+ : 1번 ~ 무한
    A* : 무수히 많은 별~ ⇒ 0번 ~ 무한
    
    정규표현식 메서드:  new RegExp(패턴, 탐색조건)
    
    </aside>
    ![Untitled](https://github.com/user-attachments/assets/449c262c-a557-44c7-9400-914d86041296)

    ```jsx
    const anthem = '동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라만세 (후렴)무궁화 삼천리 화려강산 대한사람 대한으로 길이 보전하세, 남산위에 저 소나무 철갑을 두른듯 바람서리 불변함은 우리기상 일세 (후렴)무궁화 삼천리 화려강산 대한사람 대한으로 길이보전하세, 가을하늘 공활한데 높고 구름없이 밝은달은 우리가슴 일편단심일세 (후렴)무궁화 삼천리 화려강산 대한사람 대한으로 길이보전하세, 이 기상과 이 맘으로 충성을 다하여 괴로우나 즐거우나 나라사랑하세 (후렴)무궁화 삼천리 화려강산 대한사람 대한으로 길이보전하세';
    
    console.log(anthem.match(/대한민국/)); //null
    console.log(anthem.match(/후렴/g)); //['후렴', '후렴', '후렴', '후렴']
    console.log(anthem.match(/후렴/gi)); //['후렴', '후렴', '후렴', '후렴']
    ```
    
    - replace : 원본 변경X
    
    ```jsx
    문자열.replace(탐색문자열 | 패턴, 새로운 문자열 | 콜백함수)
    ```
    

- Substring / Substr
    - substring() : slice()의 쓰임과 같지만, 매개변수에 음수를 사용할 경우 0으로 간주되어 처리
    - == substr() : 첫 번째 매개변수는 인덱스, 두 번째는 추출할 문자열의 길이
    
    ```jsx
    문자열.substring(start, end)
    
    문자열.substr(start, **length**)
    -----------------------------------------------------------------
    
      const txt = 'HTML, CSS, 자바스크립트'
    
      console.log(txt.slice(1, 7)) //1번째 인덱스부터 6번째 인덱스까지 -> TML, C
      console.log(txt.slice(7)) //인덱스 7번부터 끝까지 -> SS, 자바스크립트
      console.log(txt.slice(-6)) //자바스크립트
      console.log(txt.slice(-6, -2)) //자바스크
      
      
      console.log(txt.substring(1, 7))
      
      console.log(txt.substring(7, 1)) 
      //start > end 인 경우, start 값과 end 값을 바꾸어서 처리
      
      console.log(txt.substring(7))
      console.log(txt.substring(-7)) //음수는 0으로 취급
      
      
      console.log(txt.substr(1, 3))
      console.log(txt.substr(-6))
      console.log(txt.substr(-6, 4))
    ```
    
- Split
    - 구분자로 구분된 배열 반환( ↔ join)
    
    ```jsx
    문자열.split(구분자: string | RegExp, limit(생략ㅇ)) <-- limit = split결과를 몇개 가져올지
    -----------------------------------------------------------------
    
    const txt = 'HTML, CSS, 자바스크립트';
    
      console.log(txt.split(',')) //쉼표 다음 띄어쓰기도 문자열에 포함돼서 분리됨
      console.log(txt.split(', '))
      
      console.log(txt.split(', ', 1)) //limit = split결과 1개 ['HTML']
      console.log(txt.split(', ', 0)) //[]
      
      console.log(txt.split('')) //**['H', 'T', 'M', 'L', ',', ' ', 'C', 'S', 'S', ',', ' ', '자', '바', '스', '크', '립', '트']**
      console.log(txt.split('').reverse().join('')) //트립크스바자 ,SSC ,LMTH
    ```
    

---

### BOM(Browser / Object Model)

![Untitled](https://github.com/user-attachments/assets/7354d62b-7723-4504-8706-983d79de7ab1)

- 최상위 객체 : Window

- HTML 요소/속성 접근 메서드
![Untitled (1)](https://github.com/user-attachments/assets/9246dc71-14cb-40cb-a1c1-32ce849486a6)

- BOM메서드

```jsx
< outer = URL+툴바+스크롤바 등등 다 포함한 웹브라우저 영역 >
window.outerWidth
window.outerHeight 

< inner = 브라우저창 내부 콘텐트 영역 >
window.innerWidth
window.innerHeight

< screen = 모니터 픽셀 단위 크기 > : 값 변하지 않음
screen.width
screen.height
```

- Navigator
    - 브라우저 정보 관련 메서드
    - geolocation( ) : 사용자 위치정보(위도, 경도)가 담긴 geolocation 객체를 반환

- Location
    - location.href  (== URL)
    - location.hostname
    - location.pathname
    - location.protocol
    - location.assign()
    
    ```jsx
    	 	document.querySelector('#href').innerText = location.href;
        document.querySelector('#hostname').innerText = location.hostname;
        document.querySelector('#pathname').innerText = location.pathname;
        document.querySelector('#protocol').innerText = location.protocol;
    
        document.querySelector('.btn').onclick = function() {
          * location.assign('https://www.booksr.co.kr'); //히스토리 유지 => 이동한 페이지에서 뒤로가기 or 앞으로 가기 가능
    	    
    	    * location.href = 'https://www.booksr.co.kr'
    	    
    	    * location.reload(); //현재 페이지 재호출(새로고침)
    	    
    	    
          * location.**replace**('https://www.booksr.co.kr'); //**히스토리를 제거**하고 명시된 URL로 변경
        }
    ```
    ![Untitled (2)](https://github.com/user-attachments/assets/18d65a41-9924-437d-a02a-05cb6e16b128)

    +++ 조작할 데이터(JSON, XML) 가져오는 방법
    
    XMLHttpRequest 
    
    →  fetch( )
    
    jquery ajax( )
    
    axios
    
    ---
