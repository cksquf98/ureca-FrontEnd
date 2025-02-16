### STS 서버 통신

옛날에 했던 SpringPersonMVC2 프로젝트 불러와서 복습

- @ResonseBody 어노테이션 : 컨트롤러에 의해 jsp파일로 넘어가게 하는게 아니라 화면에 반환값으로 데이터를 리턴할 수 있도록 해줌
- 또는 `@RestController //클래스 내의 모든 메서드는 return DATA라고 명시` 어노테이션 사용
- 서버를 통해서 디비에서 데이터 불러오면 DTO에 저장되는데 이때 DTO 객체를 출력해보면 우리가 아는 JSON 형식을 가진 데이터로 뽑힘

```java
	<기존 코드에서 ResponseBody를 통해 데이터를 반환하도록 변경>
	
	@GetMapping("/list") //1.
	@ResponseBody //: data를 리턴할 수 있도록 해줌 -> 여기서 써주면 "list"문자열을 리턴
	public List<Person> list(Model model) { //DB목록출력
		 List<Person> list = null;
		
		try {
			//목록 테이블에 출력할 데이터 얻어오기
			list = service.readAll(); //3.
			
			model.addAttribute("list", list);
			
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		return list;  //5.
	}
```

- 이따가 이 서버에서 ajax로 통신을 해보자!

## AJAX - 비동기 서버 통신

<aside>
💡 ***비동기*** 처리 : 프로세스의 완료를 기다리지 않고 동시에 다른 작업을 처리하는 방식

</aside>

- XMLHttpRequest 객체를 통해서 서버와 통신

```jsx
const xhr = new XMLHttpRequest()

xhr.readyState
xhr.open("서버 경로")
xhr.send("paramName=데이터") //쿼리스트링

근데 넘 과정이 복잡하단 말이쥐
=> 라이브러리) jQuery : ajax()로 한방에! 
	 라이브러리) Axios : axios(), axios.get()/post()/put()/delete()로 한방에!
   바닐라JS) fetch() 사용

```

### Axios

- ajax 라이브러리
- Promise based HTTP client for the browser and node.js
    - 요청 성공/실패에 대해 처리를 나눠줌
    - promise method : then( ), catch( ) 사용 가능
- Request Method 종류
    - GET : `axios.get(url[, config])`
        
        ```jsx
        예)
        axios.get('https://ureca.com/user?userid=3')
          .then(function (response) {
            // 성공 콜백
          })
          .catch(function (error) {
            // 에러 콜백
          })
          .finally(function () {
            // 반드시 실행
          });
          
        * 쿼리스트링 추가 가능
        
        * axios()메소드 사용 버전
        axios({
          method: 'get',
          url: 'http://bit.ly/2mTM3nY',
          responseType: 'stream'
        })
          .then(function (response) {
            response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
          });
        ```
        
    - POST : `axios.post(url, data[, config])`
        
        ```jsx
        axios.post("https://ureca.com/user", 
        		{
                name: '홍길동',
                userid: 100
            })
            .then(function (response) {
        	    // 성공 콜백
            }).catch(function (error) {
        	    // 에러 콜백
            });
            
        * 오브젝트를 요청에 같이 보냄
        
        * 또는 axios() 메소드를 써도 됨
        axios(config)
        // POST 요청 전송
        axios({
          method: 'post',
          url: '/user/12345',
          data: {
            firstName: 'Fred',
            lastName: 'Flintstone'
          }
        });
        ```
        
    
    - PUT : `axios.put(url, data[, config])`
        
        ```jsx
        axios.put("https://ureca.com/user", {
                name: '나길동',
                userid: 100
            })
            .then(function (response) {
                 
            }).catch(function (error) {
                
            });
        ```
        
    
    - DELET : axios.delete(url[, config])
        
        ```jsx
        axios.delete('https://ureca.com/user?userid=3')
          .then(function (response) {
          })
          .catch(function (error) {
          });
        ```
        

### 서버에서 데이터 가져오기

### CORS 에러

- 내 서버는 [localhost:8080](http://localhost:8080) 도메인인데, 자바스크립트로 요청하면 도메인이 localhost:3000에서 하니까 안맞아서 에러가 남
- 해결방법 :
    - 크롬 Allow CORS 설치 → 근본적 해결책 아님. 크롬에서만 CORS 회피
    - 서버쪽에서 전체 컨트롤러에 대해 @CrossOrigin(”*”) 어노테이션 추가

### axios install 후 데이터 불러오기

- npm i axios
<img width="530" alt="image" src="https://github.com/user-attachments/assets/5e1dba6f-6e70-4ee8-90e1-11914a9cfdea" />


- useEffect를 써서 마운트 시에만 데이터를 불러와서 state에 저장하자
- @RequestBody : axios를 통해 POST요청으로 넘긴 JSON 데이터를 DTO(자바 객체)로 변경

- 이전 미션 코드 변경
    - IntelliJ
        - JAVA 코드 수정
            
            ```jsx
            @RestController //클래스 내의 모든 메서드는 return DATA라고 명시
            @RequestMapping("/person")
            @CrossOrigin("*")
            public class PersonController {
            	
            	@Autowired
            	PersonService service;//service=null;기본값
            	
            //	@RequestMapping(value = "/form", method = RequestMethod.POST) //요청URL정의   ==>1.
            	@PostMapping("/form")
            //	@ResponseBody
            	public String regist(@RequestBody Person person) {//DB입력
            		System.out.println(">>> POST form");
            		System.out.println("person>>>"+ person);
            		
            		try {
            			service.add(person);//3.
            			
            		} catch (SQLException e) {
            			e.printStackTrace();
            		}
            		
            //		return "redirect:/person/list";  // 5.	
            		return "SUCCESS";
            	}
            	
            	@GetMapping("/list") //1.
            //	@ResponseBody //: data를 리턴할 수 있도록 해줌 -> 여기서 써주면 "list"문자열을 리턴
                public List<Person> list(Model model) { //DB목록출력
            		 List<Person> list = null;
            		
            		try {
            			//목록 테이블에 출력할 데이터 얻어오기
            			list = service.readAll(); //3.
            		} catch (SQLException e) {
            			e.printStackTrace();
            		}
            		
            		return list;  //5.
            	}
            ```
            
    - VSCode
        - localStorage에서 불러오던 데이터를 디비에서 불러오도록 수정
        
        ```jsx
        [PersonList.jsx]
        
        import React, { useEffect, useState } from "react";
        import { useNavigate } from "react-router-dom";
        import Button from "react-bootstrap/Button";
        import axios from 'axios';
        
        export default function PersonList(props) {
          const [people, setPeople] = useState([])
          const navigate = useNavigate();
        
          // const people = localStorage.getItem("people")
          //   ? JSON.parse(localStorage.getItem("people"))
          //   : [];
        
          
          // axios.get(url)
          // axios.get(url).then(콜백함수)
          // axios.get(url).catch(콜백함수)
          // axios.get(url).then(콜백함수).catch(콜백함수)
          
        
          useEffect(()=>{
            axios.get("http://localhost:8080/person/list")
            .then((response)=>{
              setPeople(response.data)
            })
            .catch((error)=>{
        
            })
          }, [])
        
          const handleRowClick = (personId) => {
            navigate(`/${personId}`);
          };
          const handleBtnClick = () => {
            navigate("/");
          };
        
          return (
            <div
              style={{
                display: "flex",
                flexDirection: "column",
                justifyContent: "center",
                alignItems: "center",
                height: "100vh",
                padding: "20px",
              }}
            >
              <h2 style={{ color: "white", padding : "20px" }}>사람 목록</h2>
              <table class="table table-dark table-hover">
                <thead className="TableHead">
                  <tr>
                    <th>번호</th>
                    <th>이름</th>
                    <th>나이</th>
                    <th>직업</th>
                  </tr>
                </thead>
                <tbody className="TableBody">
                  {people.map((person, index) => (
                    <tr
                      key={person.no}
                      onClick={() => handleRowClick(person.no)}
                      style={{ cursor: "pointer" }}
                    >
                      <td>{person.no}</td>
                      <td>{person.name}</td>
                      <td>{person.age}</td>
                      <td>{person.job}</td>
                    </tr>
                  ))}
                </tbody>
              </table>
              <Button variant="secondary" onClick={handleBtnClick}>
                입력하기
              </Button>
            </div>
          );
        }
        ```
        
        ```jsx
        [InputForm - 데이터 입력 부분]
        
         function handleClick() {
            let people = JSON.parse(localStorage.getItem("people"));
            let person = null;
        
            if (personId) {
              person = { id: personId, name, age, job };
              people[personId - 1] = person;
            } else {
              // const id = people.length > 0 ? people.length + 1 : 1;
              // person = { id, name, age, job }; 디비에 no 컬럼은 auto increment니까 id 빼도 됨
              // people = [...people, person];
        
              person = { name, age, job };
              axios({
                method: "post",
                url: "http://localhost:8080/person/form",
                data: person,
              }).then((response) => {
                //DB입력이 끝마쳐진 후 라우터 변경되도록
                navigate("/list");
              }).catch((error) => {
                console.log(error);
              });
            }
        
            localStorage.setItem("people", JSON.stringify(people));
        
            navigate("/list");
          }
        ```
        

### 미션

수정/삭제도 서버 요청으로 바꿔보기!

- IntelliJ
    
    ```jsx
    	@GetMapping("/upform")//  localhost:8080/person/upform?no=3
    	public Person upform(@RequestParam("no") int no) {//수정폼 보이기
    
    		Person person = null;
    //		model.addAttribute("person", person);
    		
    		try {
    			person = service.read(no);
    		} catch (SQLException e) {
    			e.printStackTrace();
    		}
    		
    		return person;
    	}
    	
    	@PostMapping("/upform")
    	public String modify(@RequestBody Person person) {//DB수정 요청
    		try {
    			System.out.println(person);
    			service.edit(person);
    		} catch (SQLException e) {
    			e.printStackTrace();
    		}
    		return "SUCCESS";//수정 결과를 list페이지로 확인
    	}
    	
    	@GetMapping("/delete")//  localhost:8080/person/delete?no=3
    	public String remove(@RequestParam("no") int no) {//DB삭제 요청
    		System.out.println(no);
    		try {
    			service.remove(no);
    		} catch (SQLException e) {
    			e.printStackTrace();
    		}
    		return "SUCCESS";
    	}
    ```
    
- VSCode
    
    ```jsx
    function handleClick() {
        let person = null;
    
        if (personNo) {
          person = { no: personNo, name: name, age: age, job: job };
          console.log(person);
          axios({
            method: "post",
            url: "http://localhost:8080/person/upform",
            data: person,
          })
            .then((response) => {
              console.log(response.data);
              //DB입력이 끝마쳐진 후 라우터 변경되도록
              navigate(-1);
            })
            .catch((error) => {
              console.log(error);
            });
        } else {
          // const id = people.length > 0 ? people.length + 1 : 1;
          // person = { id, name, age, job };
          // people = [...people, person];
    
          person = { name, age, job };
          axios({
            method: "post",
            url: "http://localhost:8080/person/form",
            data: person,
          })
            .then((response) => {
              //DB입력이 끝마쳐진 후 라우터 변경되도록
              navigate("/list");
            })
            .catch((error) => {
              console.log(error);
            });
        }
      }
    
      function handleDelete() {
        axios({
          method: "get",
          url: `http://localhost:8080/person/delete?no=${personNo}`,
        })
          .then((response) => {
            navigate("/list");
          })
          .catch((error) => {
            console.log(error);
          });
      }
    ```
