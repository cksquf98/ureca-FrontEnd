셀프공부!!!

axios함수는 비동기 함수여서 실행 순서를 생각하기가 어려웠다

```jsx
import axios from "axios";

export const saveUsersToStorage = (user) => {
  let rt = false;

  const name = user.name;
  const age = user.age;
  const job = user.job;

  axios
    .post("http://localhost:8080/person/form", { name, age, job })
    .then((response) => {
      console.log(response.data);
      rt = true;
    })
    .catch((error) => {
      console.log(error);
      rt = false;
    });
  console.log(rt);
  return rt;
};
```

내가 의도했던건 post 요청 끝나면 then 또는 catch 구문을 수행해서 성공 여부를 보는거였는데 실제로는 데이터 insert에 성공해도 rt가 계속 false로 떴음

### 문제의 원인

- 자바스크립트의 비동기성: `axios.post`는 비동기적으로 동작합니다. 즉, `.then()`이나 `.catch()` 블록 내부의 코드가 실행되기 전에 `return rt;`가 먼저 실행됩니다.
- `rt` 값이 `axios.post`의 결과에 따라 설정되기 때문에, `axios.post` 호출이 완료되지 않은 상태에서는 여전히 `false`로 설정되어 있습니다.

### 해결 방법

비동기 작업을 올바르게 처리하려면 `async/await`를 사용하여 `Promise`가 완료될 때까지 기다려야 합니다.

```jsx
import axios from "axios";

export const saveUsersToStorage = async (user) => {
  try {
    const name = user.name;
    const age = user.age;
    const job = user.job;

    const response = await axios.post("http://localhost:8080/person/form", {
      name,
      age,
      job,
    });

    console.log(response.data);
    return true; // 성공적으로 요청이 완료되면 true 반환
  } catch (error) {
    console.log(error);
    return false; // 요청이 실패하면 false 반환
  }
};
```

### 주요 변경 사항

1. **`async` 키워드 사용**: `saveUsersToStorage` 함수를 비동기 함수로 만들었습니다.
2. **`await` 키워드 사용**: `axios.post` 호출 전에 `await` 키워드를 사용하여 비동기 요청이 완료될 때까지 기다립니다.
3. **에러 핸들링**: `try-catch` 블록을 사용하여 요청 성공 시 `true`, 실패 시 `false`를 반환합니다.

### 함수 사용 방법

이 함수는 이제 `Promise`를 반환하므로, 호출할 때 `await`를 사용하거나 `.then()`과 `.catch()`를 사용할 수 있습니다.

```jsx
javascript코드 복사
// 사용 예시
async function handleSubmit(user) {
  const result = await saveUsersToStorage(user);
  console.log(result ? "저장 성공" : "저장 실패");
}
```

위와 같이 수정하면 `saveUsersToStorage` 함수가 올바르게 작동하여, 성공적으로 `post` 요청이 완료되면 `true`를 반환하고, 그렇지 않으면 `false`를 반환합니다.

---

### 미션

효린님 코드 기반

1. Person  ==> Book 변경

[no,name,age,job]  ==>  [isbn, title, author, price, desc]

2. DB테이블 생성

book테이블

=> 컬럼: isbn, title, author, price, descmember테이블

=> id,pwd,name

=> 직접 데이터  1~3 row 입력 (MySQL Workbench이용)

3. SpringBoot

Person  ==> Book 변경

MemberController 추가

4. 로그인 폼 만들기

로그인을 하여 아래의 두개 중 한가지 메시지를 화면에 표시

1) 로그인

2) 홍길동님 반갑습니다. 로그아웃
