<13주차수업>

- Rest API
- Ajax 를 이용한 비동기 통신, Fetch API, Promise
- React project를 이용한 CORS 처리
- 웹 어플리케이션 보안 향상 가이드
- 보안 아키첵쳐와 비보안 아키텍쳐

---

### 저번 미션

- 로그인
    - 백앤드 : select id, name from member where id = #{id} and pw = #{pw}
    - 프론트 : id, pw POST 요청
    
    ```jsx
    import axios from "axios";
    
    export const postUserInfo = async (user) => {
      try {
        const response = await axios({
          method: "post",
          url: "http://localhost:8080/book/login",
          data: user,
        });
    
        return response.data; //성공하면 truthy값 리턴
      } catch (error) {
        console.log(error);
        return false; //실패하면 false 리턴하게
      }
    };
    ```
    
    ```jsx
      function handleLogin() {
        const member = postUserInfo({ id, pwd });
        if (member) {
          alert("로그인 성공");
          sessionStorage.setItem("isLogin", true);
          sessionStorage.setItem("memberName", member.name);
          navigate("/main");
        } else {
          alert("로그인 실패");
          sessionStorage.removeItem("isLogin"); //혹시 모르니까 없어도 일단 제거하는 코드 넣기
        }
      }
    ```
    
- 파일 업로드
    - 이벤트를 발생시킨 파일 불러오는 법 : `e.target.files` , 여러개면 `e.target.files[idx]`
    
    <aside>
    
    `e.target.value`로 접근 시 파일 이름 string만 반환하기 때문에 파일 내용에 접근할 수가 없음
    
    </aside>
    
    - 백앤드 : `private MultipartFile image; //DTO에 파일 저장 객체 추가`
        
        MultipartFile → 파일을 바이너리 형식으로 받는 클래스
        
        ```jsx
        [Controller]
        public int regist(@RequestBody Book book) 
        	>>> "Content-type : application/json"으로 book 객체를 body에서 받음
        
        public int regist(Book book)
        	>>> "Content-type : application/form-data"로 book 객체를 body에서 받음
        ```
        
        💡 근데 문제점!! 객체 주소값인가.. 암튼 객체가 넘어옴
        
        ⇒ Service에 파일 업로드 전용 메서드 추가
        
        ```java
        public int regist(Book book) throws SQLException {
        	uploadFile(book);
        	return bookDAO.insert(book);
        }
        
        // ServletContext를 주입받는다고 가정
        private ServletContext application;
        
        public void uploadFile(Book book) throws IOException {
            MultipartFile mfile = book.getImage();
            String fileName = mfile.getOriginalFilename();
            System.out.println("파일 이름: " + fileName);
        
            // 파일 이름을 Book 객체에 설정
            book.setImage(fileName);
        
            // 서버의 실제 경로에 'upload' 폴더 생성 및 파일 저장 경로 설정
            String filePath = application.getRealPath("/") + "upload/";
            System.out.println("서버 경로(프로젝트 경로): " + filePath);
        
            // 디렉토리 존재 확인 및 생성
            File uploadDir = new File(filePath);
            if (!uploadDir.exists()) {
                uploadDir.mkdirs();  // upload 폴더가 없다면 생성
            }
        
            // 업로드된 파일을 서버의 지정된 위치에 저장
            File outFile = new File(filePath + fileName);
            byte[] inFile = mfile.getBytes();
        
            // 파일 복사 수행
            FileCopyUtils.copy(inFile, outFile);
        
            System.out.println("파일이 성공적으로 업로드되었습니다: " + outFile.getAbsolutePath());
        }
        ```
        
        - mapper insert문 수정
            
            ```java
            <insert id="insert" parameterType="Book"> insert into book (isbn,title,author,price,`desc`, img) values (#{isbn},#{title},#{author},#{price},#{desc}, #{img}) </insert>
            ```
            
    
    - 프론트 : JSON 데이터가 아니라 formData로 전송하도록
    
    ```jsx
      const registBook = () => {
        if (buttonDisabled) {
          return;
        }
    
        // axios.post
        let formData = new FormData();
    
        formData.append("isbn", isbn);
        formData.append("title", title);
        formData.append("author", author);
        formData.append("price", price);
        formData.append("desc", desc);
        formData.append("file", image);
    
        if (saveBookInfo(formData)) {
          alert("등록 성공");
        } else {
          alert("등록 실패");
        }
      }; //registBook
      
      
      --------------------------------------------------------
      [saveBookInfo.js]
      import axios from "axios";
    
      export const saveImageInfo = async (bookFormData) => {
      try {
        const response = await axios({
          headers: {
            "Content-Type": "multipart/form-data",
          },
          method: "post",
          url: "http://localhost:8080/regist",
          data: bookFormData,
        });
    
        return response.data;
    
      } catch (error) {
        return false;
      }
    };
    ```
    
- 이미지 조회
    - 파일 경로 불러와서 img태그 src로 쏙

---

### 미션

1. 수정화면에 업로드된 이미지 보이도록
2. 로그인 시에만 수정/삭제 가능하도록
3. 목록 테이블 페이징
- SQL문 쿼리에서 select할때 limit 사용해서 뽑아오기
    
    <aside>
    
    limit 숫자1 : 위에서부터 숫자1만큼 행 가져오기
    
    limit 숫자1, 숫자2 : 숫자1 = 시작 위치(offset : 스킵할 행 개수), 숫자2 = 가져올 행의 개수
    
    - 1페이지=> limit  0,10
    - 2페이지=> limit 10,10
    - 3페이지=> limit 20,10
    - 4페이지=> limit 30,10
    </aside>
    
1. 동일한 이름의 이미지 업로드 시 덮어씌워지는 문제 해결
    - 관련키워드 : new Date( ),  UUID
