## 리액트 보충수업

```
1. app 생성 만들기
	npx create-react-app 이름(소문자)
	
2. 컴포넌트 만들기
	* 컴포넌트를 만드는 이유 : 재사용을 위하여

3. 브라우저에 '안녕' 찍어보기

4.버튼 만들기 <<이벤트의 기본

5. 버튼 클릭 시 '안녕'문자열 옆에 숫자 출력하기
su = su + 1;
console.log('su=',su);
	* 콘솔은 변하지만 브라우저(컴포넌트)는 변경없음

6. 상태 변수를 만들어야 즉각적으로 값 변경을 인식할 수 있음
const [su2,setSu2] = useState(0);
function handleClick(){
  setSu2(su2+1);
}

7. input text
	<input type="text" />
	
8. state변수의 값을 input text에 전달하기
	<input type="text" value={state변수} />
	
9. input text 값을 state에 저장하기
	<input type="text" onChange={handleChange} /> -> 해당 함수에서 (event)=>setState(event.target.value)

10. 자식 컴포넌트 만드는 방법
export default function Child1(props) { return <div>자식</div> }
	
11. 부모 컴포넌트의 su를 자식 컴포넌트에 출력하기
	<Child parentSu={su} /> : props로 전달

11. 자식 컴포넌트의 버튼 클릭 시 부모의 su 증가시키기
	* 

12. 자식1 컴포넌트에 증가 감소한 값이 부모에는 전달되지만 자식2 컴포넌트한테는 전달되지 않게 하기
(부모는 여러 자식의 수를 합산한 값을 출력)

예)
도서 대출 반환
총도서 대출 수
```

### 미션 - CRUD

- localStorage 사용
- RouterApp
    
    ```jsx
    [RouterApp.jsx]
    import { BrowserRouter, Routes, Route } from "react-router-dom";
    import InputForm from "./pages/InputForm";
    import PersonList from "./pages/PersonList";
    
    export default function RouterApp(props) {
      return (
        <div style={{ backgroundColor: "black" }}>
          {/* header는 고정 요소니까 밖으로 뺴주기 */}
    
          <BrowserRouter>
            <Routes>
              {/* URL 가상 경로와 매핑할 컴포넌트 */}
              <Route path="/" element={<InputForm />} />
              <Route path="/:personId" element={<InputForm />} />
              <Route path="/list" element={<PersonList />} />
            </Routes>
          </BrowserRouter>
        </div>
      );
    }
    
    ```
    
- InputForm
    
    ```jsx
    [InputForm.jsx]
    import { useEffect, useState } from "react";
    import { useNavigate, useParams } from "react-router-dom";
    import Button from "react-bootstrap/Button";
    import Form from "react-bootstrap/Form";
    
    export default function InputForm(props) {
      const [name, setName] = useState("");
      const [age, setAge] = useState("");
      const [job, setJob] = useState("");
      const { personId } = useParams();
      console.log(personId);
    
      const navigate = useNavigate();
    
      useEffect(() => {
        if (personId) {
          const people = JSON.parse(localStorage.getItem("people"));
          const person = people[personId - 1];
    
          setName(person.name);
          setAge(person.age);
          setJob(person.job);
        }
      }, []);
    
      function handleChange(event) {
        switch (event.target.id) {
          case "name":
            setName(event.target.value);
            break;
          case "age":
            setAge(event.target.value);
            break;
          case "job":
            setJob(event.target.value);
            break;
          default:
        }
      }
    
      function handleClick() {
        let people = JSON.parse(localStorage.getItem("people"));
        let person = null;
    
        if (personId) {
          person = { id: personId, name, age, job };
          people[personId - 1] = person;
        } else {
          const id = people.length > 0 ? people.length + 1 : 1;
          person = { id, name, age, job };
          people = [...people, person];
        }
    
        localStorage.setItem("people", JSON.stringify(people));
    
        navigate("/list");
      }
    
      const handleDelete = () => {
        const people = JSON.parse(localStorage.getItem("people"));
        people.splice(personId - 1, 1);
        localStorage.setItem("people", JSON.stringify(people));
        navigate("/list");
      };
    
      return (
        <div
          style={{
            display: "flex",
            justifyContent: "center",
            alignItems: "center",
            height: "100vh",
          }}
        >
          <Form
            name="form"
            onSubmit={handleClick}
            style={{
              width: "80%",
              padding: "20px",
            }}
          >
            <Form.Label style={{ color: "white" }}>이름</Form.Label>
            <Form.Control
              type="text"
              id="name"
              value={name}
              onChange={handleChange}
              style={{ marginBottom: "20px" }}
            />
    
            <Form.Label style={{ color: "white" }}>나이</Form.Label>
            <Form.Control
              type="text"
              id="age"
              value={age}
              onChange={handleChange}
              style={{ marginBottom: "20px" }}
            />
    
            <Form.Label style={{ color: "white" }}>직업</Form.Label>
            <Form.Control
              type="text"
              id="job"
              value={job}
              onChange={handleChange}
              style={{ marginBottom: "20px" }}
            />
    
            <div id="btn">
              <Button type="submit" variant="light" style={{ margin: "10px" }}>
                {personId != null ? `수정` : `입력`}
              </Button>
              {personId != null && (
                <Button
                  variant="light"
                  style={{ margin: "10px" }}
                  onClick={handleDelete}
                >
                  삭제
                </Button>
              )}
            </div>
          </Form>
        </div>
      );
    }
    
    ```
    
- PersonList
    
    ```jsx
    import React from "react";
    import { useNavigate } from "react-router-dom";
    import Button from "react-bootstrap/Button";
    import Table from "react-bootstrap/Table";
    
    const styles = `{
      .TableHead {
        background-color: gray;
      }
    }`;
    
    export default function PersonList(props) {
      const navigate = useNavigate();
    
      const people = localStorage.getItem("people")
        ? JSON.parse(localStorage.getItem("people"))
        : [];
    
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
                  key={person.id}
                  onClick={() => handleRowClick(person.id)}
                  style={{ cursor: "pointer" }}
                >
                  <td>{person.id}</td>
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
