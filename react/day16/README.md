## **@PathVariable 경로변수**

PathVariable을 사용하면 리소스 경로에 식별자를 넣어서 동적으로 URL에 정보를 담을 수 있다.

URL 경로의 **중괄호 { } 안쪽에 변수를** 담고, 그 변수를 **@PathVariable(" ")로 받아서 사용**할 수 있다.

```jsx
@GetMapping ("/order/{orderId}")
public String getOrder(@PathVariable String orderId){
    log.info("orderId : {}", orderId);
    
    return "orderId:"+ orderId;
}
```

**요청** : http://localhost:8080/order/123

**응답** : orderId:123

**여러 개의** PathVariable을 동시에 사용할 수 있다.

```jsx
@GetMapping("/order/{orderId}/{amount}")
public String getOrder(@PathVariable String orderId, @PathVariable String amount){
    log.info("orderID : {}, amount : {}", orderId, amount);
    
    return "orderId:"+ orderId + "amount:" + amount;
}
```

## **@RequestParam 요청 파라미터**

웹에서 쿼리 파라미터를 포함한 url 요청이 있을 때,

컨트롤러에서는 **@RequestParam** 어노테이션으로 파라미터 값을 읽어서 사용할 수 있다.

```jsx
@GetMapping("/order")
public String getOrderRequestParam1(
        @RequestParam("orderId") String id,
        @RequestParam("orderAmount") String amount) {
        
    log.info("orderID : {}, orderAmount : {}", id, amount);
    
    return "orderId:" + id + " amount:" + amount;
}
```

**요청** : http://localhost:8080/order? orderId=123&orderAmount=900

**응답** : orderId:123 amount:900

---

### 로그인 처리

로그인 후 response로 돌아오는 유저의 데이터를 유지시키는 방법

토큰을 저장해서 주기적으로 로그인 유지를 체크해줘야 함

1. Cookie
2. WebStorage -  SessionStorage

---

### 미션

파일 업로드

- 사용자 PC 내 파일 → 서버에 저장

<파일 업로드 규칙>

1. form태그의 method속성은 반드시 post!!
2. form태그의 속성 enctype="multipart/form-data" 추가!!
    - 폼내의 데이터들을 text가 아닌 stream(바이너리)으로 전송!!
    - 참고) enctype="application/x-www-form-urlencoded" (디폴트) : form태그내의 name데이터들을 text(스트링)로 전달!!
3. input type=”file” name=”name1”
    - 서버에서 받을 때 getParameter를 name1로 받아야 함
4. 파일의 real path

<8/30 미션>

- 도서 입력폼에 '도서이미지' 파일 업로드 하기
- 도서 수정폼에 '도서이미지' 출력하기
- 힌트)
    
    <axios>
    
    - FormData
    - multipart/form-data
    
    <springBoot>
    
    - MultipartFile
