---
{"dg-publish":true,"permalink":"/project/book-rating-api/jwt/","dgPassFrontmatter":true,"noteIcon":""}
---


`AuthenticationSuccessHandler` 인터페이스를 구현하여 ==사용자가 로그인에 성공했을 때 JWT 토큰을 생성==하여 클라이언트에 전달하는 역할
##  JWT 토큰 생성
```java
String token = Jwts.builder()//JWT 생성 빌더를 호출하고, 사용자 정보와 만료 시간 등 JWT 클레임(claim)을 설정함

	.setSubject(authentication.getName()) // 사용자 이름 설정
	.claim("authorities", authentication.getAuthorities()) // 사용자 권한 설정
	.claim("username", userSessionService.getUserName()) // 사용자 이름
	.claim("email", userSessionService.getUserEmail()) // 사용자 이메일
	.claim("avatar", userSessionService.getAvatar()) // 사용자 아바타 이미지
	.setIssuedAt(new Date()) // 토큰 생성 시간
	.setExpiration(new Date(System.currentTimeMillis() + 86400000)) // 토큰 만료 시간 (1일)
	.signWith(SignatureAlgorithm.HS256, jwtSecret.getBytes()) // 서명 알고리즘 및 비밀 키
	.compact();

```


## JWT를 HttpOnly 쿠키에 저장
```java
Cookie jwtCookie = new Cookie("jwt", token);//쿠키 이름을`jwt`로 설정하고,JWT 토큰 값을 쿠키 값으로 설정
jwtCookie.setHttpOnly(true);  // 자바스크립트 접근 방지
jwtCookie.setSecure(true);    // HTTPS에서만 쿠키 전송
jwtCookie.setPath("/");       // 쿠키 유효 경로 설정
jwtCookie.setMaxAge(24 * 60 * 60); // 쿠키 유효 기간 설정 (24시간)

response.addCookie(jwtCookie); //생성된 쿠키를 응답에 추가
```

- jwtCookie.setHttpOnly(true) : ==자바스크립트에서 쿠키에 접근하지 못하도록 제한==
	- 브라우저의 `document.cookie` API를 통해 쿠키를 읽거나 수정할 수 없게 되어, <u>자바스크립트를 통해 쿠키가 노출되는 것을 방지</u>할 수 있음
- jwtCookie.setSecure(true) : ==HTTPS 연결에서만 쿠키가 전송되도록 설정==
	- HTTPS가 아닌 HTTP 요청에서는 JWT 쿠키가 전송되지 않기 때문에, HTTP 연결을 사용하는 경우 서버는 JWT 토큰을 확인할 수 없어 인증이 불가능하게 됨
	- 즉, 토큰을 통한 인증이 필요한 경우, <u>반드시 HTTPS를 통해 통신해야 함</u>
- jwtCookie.setPath("/") : ==모든 경로에서 사용하기 위해 설정==





### ⏳ 쿠키 유효기간 설정의 이유 요약

- **액세스 토큰**: 쿠키 유효기간을 짧게 설정하여, 보안 강화를 위해 사용합니다. 일반적으로 액세스 토큰은 자주 갱신되므로, 유출되더라도 피해를 줄일 수 있습니다.
- **리프레시 토큰**: 쿠키 유효기간을 길게 설정하여, 사용자가 재로그인 없이 지속적으로 서비스를 사용할 수 있도록 합니다.
📝 [[CS/네트워크/Access Token과 Refresh Token\|Access Token과 Refresh Token]]


- ==단일 JWT 토큰을 사용하는 경우==  🙋‍♀️(나의 경우)
	- 단일 JWT 토큰만 사용하는 경우에는 **쿠키와 토큰의 유효기간을 동일하게 설정**하는 것이 일반적입니다. 
	- 이 경우 쿠키 유효기간은 단일 토큰의 만료 시간과 일치하게 설정되며, 쿠키와 토큰이 함께 만료됩니다.