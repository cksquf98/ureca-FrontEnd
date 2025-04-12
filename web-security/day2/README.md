<JWT서버할일>

1. 가장 먼저 할일

DB테이블 => token 컬럼 추가

```
alter table member add column `token` varchar(100) null;
```

2. DTO(Member)에 refreshToken속성 추가 (생성자,getter,setter,toString)

```java
public class Member {
	private String id;
	private String pwd;
	private String name;
	private String refreshToken;
	
	//getter setter
}
```

3. member.xml 수정/추가 => refreshToken관련된 조회와 수정,  사용자 정보만 얻는 sql 분리

```java
Mappers/member.xml

	<select id="selectLogin" parameterType="com.ureca.member.dto.Member" 
	          resultType="com.ureca.member.dto.Member">
	    select id,name, token
	    from member 
	    where id=#{id} and pwd=#{pwd}
	</select>
	
	
*** 이렇게 쓰면 token != refreshToken 변수명이 안맞아서 매핑이 안됨

해결법>
	1. 변수명 맞춰주기 : DB에서 컬럼 변경 token -> refreshToken
		alter table member drop column `token`;
		alter table member add column `refreshToken` varchar(1000) null;
	
	2. resultMap 사용해서 변수끼리 매핑시켜주기
		<resultMap type="member" id="memberMap">
			<result column="token" property="refreshToken" />
		</resultMap>
```

4. MemberDAO수정/추가  : member.xml의 id와 일치시키기

5. MemberService, MemberServiceImpl 수정/추가

```java
	<update id="saveRefreshToken" parameterType="map">
		update member
		set refreshToken = #{token}
		where id = #{userId}
	</update>
	
	<select id="getRefreshToken" parameterType="string" resultType="string">
		select refreshToken
		from member
		where id = #{id}
	</select>
	
	<update id="deleteRefreshToken" parameterType="map">
		update member
		set refreshToken = #{token}
		where id = #{userId}
	</update>
```

6. MemberController 수정

```java
//	 기존 리턴값 => 클라이언트 자바스크립트에게 전달하는 데이터({}, [])
//	 변경 리턴값 => ResponseEntity : 데이터 + 서버의 status
	@PostMapping("/login")
	public ResponseEntity<Map<String, Object>> findMember(@RequestBody Member member){

		Map<String, Object> resultMap = new HashMap<String, Object>();
		HttpStatus status = null;

	  try {
		Member loginMember = memberService.login(member);
//		System.out.println("loginMember>>>" + loginMember);

		//요청 처리 후 서버의 상태
		 status = HttpStatus.ACCEPTED;

		if (loginMember != null) {
			String accessToken = jwtUtil.createAccessToken(loginMember.getId());
			String refreshToken = jwtUtil.createRefreshToken(loginMember.getId());
			System.out.println("access token : " + accessToken);
			System.out.println("refresh token : " + refreshToken);

//			발급받은 refresh token 을 DB에 저장.
			memberService.saveRefreshToken(loginMember.getId(), refreshToken);

//			JSON 으로 token 전달.
			resultMap.put("access-token", accessToken);
			resultMap.put("refresh-token", refreshToken);
			status = HttpStatus.CREATED; //201

		} else {
			status = HttpStatus.UNAUTHORIZED; //요청 처리 후 서버의 상태 501
			resultMap.put("message", "아이디 또는 패스워드를 확인해 주세요.");
		}
	  }
	  catch (Exception e) {
//		e.printStackTrace();
		System.out.println("로그인 에러 발생 : " + e);
		resultMap.put("message", "현재 시스템에 문제가 발생했으니 잠시 후 시도해주십시오.");
		status = HttpStatus.INTERNAL_SERVER_ERROR; //500
	  }
	  return new ResponseEntity<Map<String, Object>>(resultMap, status);
	}
```

- `ResponseEntity` : 데이터 + 서버의 status
- status : 서버 상태 표시 (404, 405, …)

<aside>
<img src="/icons/cloud_blue.svg" alt="/icons/cloud_blue.svg" width="40px" />

로그 디버깅 용도
application.properties

`#log level`

`logging.level.com.ureca.dao = debug`

</aside>

---

### XSS

웹사이트에 악성코드를 삽입하는 공격 방법

웹 응용 프로그램의 결함을 이용하여 악성 코드를 사용자에게 보냄

### XSS 방지

전처리 없이 input에 들어오는 값을 파라미터로 냅다 쓰는 코드의 경우

- <script> … </script> 코드 내용을 적어서 제출한 후 브라우저를 조작할 수 있단 말이지. 로컬 스토리지에도 접근할 수 있단 말이지
- 이런 위험을 없애기 위해 <script> 태그를 쓰지 못하도록

해결법

- 정규 표현식 사용해서 유효성 검사를 하면 되겠다!
    - 정규 표현식
        - 암것도 안쓰면 그 문자열 1번 등장
        - ? : 0 또는 1번 등장
        - * : 0 ~ infinite 등장
        - + : 1 ~ infinite 등장
        - - : 범위를 설정

eval, setTimeout, setInterval : 얘네는 보안이 취약하니까 사용하지 말랭
