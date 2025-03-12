fileName 정하기 :`multiPartFileObj.getOriginalFilename()`

전체 파일명 이름 설정 : `UUID.*randomUUID*()` + fileName

<aside>

- 파일을 업로드할 때, 같은 이름의 파일을 업로드하면 기존 파일이 다른 파일로 덮어씌워질 수 있음
- 이를 방지하기 위해 파일의 이름을 UUID로 생성하여 저장
</aside>

### 환경변수 .env

1.반복되고 변경가능한 변수(서버ip/port)

2.개인정보(api키)

- .env는 루트경로(폴더최상단)에 위치해야함(src밑X)
- .gitignore 파일에 추가할 코드
    
    ```jsx
    # .gitignore
    .env
    .env.local
    .env.development.local
    .env.test.local
    .env.production.local
    ```
    
- 변수명 앞에 `REACT_APP_` 접두어 사용 (다른 시스템의 같은 이름과 구분)
    - .env파일
    
    ```java
    REACT_APP_API_URL=http://localhost:8080
    REACT_APP_PAGE_SIZE=10
    ```
    
- 환경변수 적용을 위해 프로젝트 재시작이 필요
    - 확인 : console.log(process.env);
    - 사용 : `process.env.변수명` => import없이 사용가능

### SLF4J 라이브러리를 이용한 로그 출력

- 패키지 org.slf4j 사용
- Logger클래스의 메서드 사용

사용법)

- 클래스내에서(멤버)

```jsx
private static final Logger logger = LoggerFactory.getLogger(로그 기준클래스.class);
```

- 각 메서드 내에서

```jsx
(System.out.println("출력할 내용");요거랑 결국 같다고 보면 됨)

logger.trace("출력할 내용");

logger.debug("출력할 내용");

logger.info("출력할 내용");

logger.warn("출력할 내용");

logger.error("출력할 내용");

* logger를 사용해 출력하면 레벨별로 제어 할 수 있는 장점이 있음.
* 로그 레벨 순서 TRACE > DEBUG > INFO > WARN > ERROR형식)
```

형식)

src/main/resources log4j.xml안에

```jsx
<logger name="적용할 패키지명">
<level value="로그레벨" />
</logger>
```

적용)

<!-- Application Loggers -->

<logger name="com.ureca.hello">

<level value="debug" />

</logger>==> com.ureca.hello패키지내의 클래스의 로깅을 제어하는데

<level value="debug" />

위와 같이 debug선언하였다면 debug부터 info,warn,error메서드로 정의된 로깅을 할 수 있습니다.그외 값 모든로그/로깅해제 : all/off를 사용하는 것도 가능합니다.

-----------------------------------------------------------------------------

##### 추가사항)

logger는 SLF4J 치환문자({})를 사용할 수 있다.

기존 : `System.out.println("debug - "+ name)` ⇒ `logger.debug("debug - {} ", name )`

- 불필요한 문자열 더하기 연산을 안해도 됨

-----------------------------------------------------------------------------

<img width="714" alt="image" src="https://github.com/user-attachments/assets/991f1790-0d39-4765-8f9a-f6ea148329a1" />

Spring > application.properites

```jsx
# log level setting
logging.level.com.ureca=debug
```

: 얘만 넣어주면 debug 선언 부분을 쫘라라락 출력시켜줌

---

**<9/3 미션>**

1.수정폼 이미지 옆에 '변경'버튼을 만들고 클릭하였을때 이미지를 변경한다

```jsx
* useEffect를 써서 이미지 파일 변경되었을 때 재렌더링되도록
  
  const [imgChange, setImgChange] = useState(true)

  // useEffect(() => {
  //   findBookInfoByIsbn(bookIsbn, setBookInfo);
  // }, []);

  //
  useEffect(() => {
    findBookInfoByIsbn(bookIsbn, setBookInfo);
  }, [change]);
  
const handleUpfileChange = (event) => {
    setUpfile(event.target.files[0]);
  };

const updateBookImg = () => {
    //파일 업로드를 통해 이미지 파일 수정
    const formData = new FormData();
    formData.append("isbn", bookIsbn);
    formData.append("upfile", upfile);

    axios.put(`${url}/updateImage`,formData,
      {headers:{'ContentType':'multipart/form-data'}
    });

    alert("이미지 수정")
    setImgChange(!imgChange) //imgChange 상태 변경 -> useEffect 실행
}
  
---------------------------------------------------
return문 내용 추가
<S.Row>
{/* 변경 버튼 누르기 전 */}
{isChangeBtnClicked &&
	<Button
  disabled={false}
  text="변경"
  clickFunc={() => setIsChangedBtnClicked(true)}
/>}

{/* 변경 버튼 누른 후 */}
{isChangeBtnClicked && (<>
            <StyledInput style={{ marginLeft: "10px" }} type="file" 
            onChange={handleUpfileChange}
            />
            <Button
            disabled={false}
            text="변경"
            clickFunc={}
          />
</>)}
</S.Row>
```

2. .env를 사용하여 url과 관련된 전체코드를 변경하고 정상 작동되도록 한다.

함수를 주고 전체사용 가능하도록 한다

3. 검색어추가

=> 테이블 상단

=> 저자/제목(select.option)

=> 검색어(input type=text) 포함되어 있는 값(행)만 출력

<aside>

**💡 MyBatis 동적SQL**
<if test>
<choose> … <when test> .. <otherwise>
<where> : where키워드가 필요하면 써주고 필요없으면 자동 생략시켜줌
<trim>
<foreach item=”item변수명” collection=”list” …>

- mapper에서 SQL문에 parameterType 명시해주면 그 타입만 들어와야 하고, 명시 안하면 암거나 들와도 상관 없다는 의미임

```jsx
  <select id="selectPage" resultType="Book"  parameterType="map">
     select isbn,title,author,price,`desc` 
     from book
     limit #{offset}, #{len}
  </select>
```

</aside>

### SQL문 내 변수

#{ } : 데이터용 변수

${ } : 컬럼도 변수로 대체 가능
