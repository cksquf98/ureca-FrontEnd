### Cookie

- 클라이언트가 웹사이트에 접속할 때 사용하게 되는 기록 파일
- Key Value 형식의 문자열 형태로 저장
- 유효기간 설정 가능

**로그인 과정**

1. 서버가 응답 헤더에 저장하여 클라이언트에 전달
2. 클라이언트는 서버에 요청을 보낼 때마다, 매번 쿠키를 요청 헤더의 쿠키에 담아서 전달
3. 서버는 쿠키를 통해 클라 식별

단점

- 노출되기 때문에 민감 정보 저장 XXX
- file로 저장되어 조작이 가능하고 사이즈가 제한됨
- 서로 다른 브라우저간 공유X
- 쿠키 사이즈가 커지면 네트워크 부하 가중

### **Session**
<img width="699" alt="image" src="https://github.com/user-attachments/assets/d6a39759-b75f-416e-ae1a-13400a406bc4" />

- 서버 메모리에 저장하는 느낌 → 클라이언트 인증 정보를 서버측이 저장하고 관리
- 민감 정보 노출을 막기 위함

**로그인 과정**

1. ID, PWD로 서버에 로그인 요청
2. 인증 후 유니크한 id를 만들어서 세션 저장소에 저장한 후 SESSION ID 발행
3. SESSION ID를 클라에 반환
4. 인증정보 요청 시 SESSION ID를 쿠키에 담고 헤더에 담아서 서버에 전달
5. 서버는 클라가 보낸 세션 아이디랑 서버 세션저장소에 있는 세션 아이디 비교

**단점**

- 서버 메모리 부담
- 쿠키에 저장되는 세션 아이디는 중요 정보가 없어서 안전하지만, 그 쿠키를 사용해서 클라인 척 위장 가능
- 매 요청마다 서버 세션저장소 조회해야 함

### Token

- 클라이언트가 서버에 접속해서 인증하면 서버가 유일값인 토큰을 발급해줌
- 서버에 요청 시 헤더에 토큰을 넣어서 보냄
- 클라가 보낸 토큰이 서버에서 발급한 토큰과 같은지 체크
- 토큰은 앱과 서버가 통신 및 인증할때만 사용

단점

- 토큰 자체의 데이터 길이가 김 → 요청 많아지면 네트워크 부하
- Payload자체는 암호화되지 않아서 중요 정보 저장 부라
- 토큰 탈취 우려
    
    ⇒ 그래서 나온게 JWT!!
    

## JWT(JSON Web Token)

- 인증에 필요한 정보를 암호화 시킨 JSON 토큰
- JSON데이터를 인코딩해서 직렬화
- 토큰 내부에 개인키를 통한 전자서명 포함 → 보안 ⬆️⬆️⬆️

### 구성
<img width="719" alt="image" src="https://github.com/user-attachments/assets/93ba63fb-df3f-4d53-9f56-ef534bc1b766" />

Header - Payload - Signature

**1️⃣ Header**

- JWT에서 사용할 토큰의 타입과 암호화 알고리즘 정보
- key-value

**2️⃣ Payload**

- 서버로 보낼 사용자 권한 정보와 데이터
- key-value
- 토큰에 담을 클레임 정보
    - 클레임 : payload에 담는 정보로, 토큰에 여러개 넣을 수 있음

**3️⃣ Signature**

- 서버 개인키를 포함해서 암호화
- 토큰의 유효성을 검증하기 위한 문자열

Refresh Token

- Access Token을 탈취당했을 경우에 대한 대비책(해결책은 아님)
- Access Token의 유효기간을 짧게 설정하고 Refresh Token을 통해서 다시 발급받아 사용
- 인증정보를 담고있지않음
- 온리 재발급 용도

로그인 과정

1. 클라이언트가 인증 요청 시 서버가 확인하여 Access Token, Refresh Token 발급
2. 클라가 받아서 Refresh Token을 **세션 스토리지**에 저장하고 엑세스 토큰으로 서버에 요청
3. 요청 중 엑세스 토큰 만료 시 리프레시 토큰을 서버로 전달하여 새로운 엑세스 토큰 발급을 요청
4. 서버는 Refresh Token을 받아서 서버 Refresh Token Storage에 해당 토큰이 있는지 확인하고, 있으면 엑세스 토큰 생성하여 전달

### JWT 토큰 생성

- JWT 사용을 위한 dependency 추가
- DB에 refresh token 저장 컬럼 추가
- JAVA
    
    ```jsx
    //토큰 생성
    Claims claims = Jwts.claims().setSubject("토큰 이름")
                                .setIssuedAt(new Date()) //생성일
                                .setExpiration(new Date(System.currentTimeMillis() + expireTime)) //유효기간
    
    claims.put("key", value) //저장할 데이터의 키-값
    
    String jwt = Jwts.builder()
                    .setHeaderParam("typ", "JWT")
                    .setClaims(claims)
                    .signWith(SignatureAlgorithm.HS256, this.generateKey())
                    .compact()
    
    //application.properties에 토큰 만료 유효기간 설정
    jwt.access-token.expiretime = 밀리세컨
    jwt.refresh-token.expiretime = 밀리세컨
    
    ```
    

<aside>

ResponseEntity

```
ResponseEntity란, httpentity를 상속받는, 결과 데이터와 HTTP 상태 코드를 직접 제어할 수 있는 클래스이다.
ResponseEntity에는 사용자의  HttpRequest에 대한 응답 데이터가 포함된다.
```
<img width="661" alt="image" src="https://github.com/user-attachments/assets/21f97c10-943e-4e75-b80c-015fa67458b1" />

</aside>
