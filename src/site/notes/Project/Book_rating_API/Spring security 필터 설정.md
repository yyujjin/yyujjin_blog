---
{"dg-publish":true,"permalink":"/project/book-rating-api/spring-security_필터_설정/","dgPassFrontmatter":true,"noteIcon":""}
---


## ☑️ 인증되지 않은 사용자에게 401 상태코드 반환하기


- Spring Security는 기본적으로 인증이 필요한 URL에 인증되지 않은 사용자가 접근하면는 <u>로그인 페이지로 리다이렉트</u>한다.

```java
.exceptionHandling((exceptions) -> exceptions
        .authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED))
)
```

- **exceptionHandling** : 예외 처리 규칙을 정의하는 데 사용
- **authenticationEntryPoin** : 인증되지 않은 사용자가 보호된 리소스에 접근할 때 호출되는 엔트리 포인트를 지정
- **HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED)** : 
	- HttpStatusEntryPoint는 지정된 <u>HTTP 상태 코드를 응답으로 보내는</u> 엔트리 포인트
	- HttpStatus.UNAUTHORIZED는 <u>HTTP 401 Unauthorized 상태 코드를 의미</u>


## ☑️ OAuth2 로그인 설정
{ #0ca42e}


```java
.oauth2Login(oauth2 -> oauth2
    .defaultSuccessUrl(frontendUrl + "/loginSuccess", true) 
    .failureUrl(frontendUrl + "/loginSuccess?error=true") 
    .successHandler(customLogInSuccessHandler)
)
```
{ #4d6918}


- **defaultSuccessUrl** : OAuth2 <u>로그인 성공 후</u> 사용자를 리다이렉트할 기본 URL을 설정
- **failureUrl** : OAuth2 <u>로그인 실패 시</u> 리다이렉트할 URL을 설정
- **successHandler** : 
{ #0b235d}

	- 로그인 성공 후 추가적인 동작을 수행할 수 있음
	- 예를 들어, JWT를 생성하여 프론트엔드로 전달하거나, 로그인 성공 이벤트를 기록하는 로직을 추가할 수 있음
	- 이 핸들러는 `defaultSuccessUrl` 설정보다 <u>우선시되어 호출</u>되며, 핸들러가 리다이렉트를 처리할 수도 있습니다.
		- 즉,`successHandler`를 설정하면 `defaultSuccessUrl`과 `failureUrl`을 설정하지 않아도 됨  
	

## ☑️ 로그아웃 설정

```java
.logout(logout -> logout
        .logoutUrl("/logout") 
        .invalidateHttpSession(true)  
        .deleteCookies("JSESSIONID", "jwt")  
        .logoutSuccessHandler((request, response, authentication) -> {
            response.setStatus(HttpServletResponse.SC_OK);  // 200 상태 코드 설정
            response.getWriter().flush();  // 응답 본문 비우기
        })
)
```

- **logoutUrl** 
	- <u>로그아웃 요청</u>을 처리할 URL을 설정
	- Spring Security는 기본적으로 `/logout` URL로 로그아웃 요청이 들어오면 자동으로 <u>서버 측 세션 무효화와 사용자 브라우저에 세션 식별 쿠키(JSESSIONID 쿠키)를 삭제해줌</u>
		- 🤔 JSESSIONID 쿠키란? 사용자가 로그인하면, 서버는 고유한 세션 ID를 생성합니다.서버는 생성된 세션 ID를 `JSESSIONID`라는 이름의 쿠키에 저장하고, 이를 브라우저에 전달합니다.
- **deleteCookies**
	- `JSESSIONID`와 `jwt` 쿠키삭제됨
	- 해당 쿠키로 저장된 이름을  로그아웃 시 자동으로 삭제
- **logoutSuccessHandler**
	- 로그아웃이 성공적으로 완료되었을 때의 동작을 정의하는 핸들러
	- 기본적으로 Spring Security는 로그아웃 성공 시 `/login?logout` 경로로 리다이렉트됨
		- 로그인 페이지로 이동하면서 `logout` 쿼리 매개변수를 통해 클라이언트가 로그아웃이 성공적으로 이루어졌음을 알 수 있도록 하기 위함
- **response.setStatus(HttpServletResponse.SC_OK)**
	- HTTP 응답 상태 코드를 200 OK로 설정하여 로그아웃 성공을 나타냄
	- 직접 HTTP 응답을 설정하고 있는거임
- **response.getWriter().flush()**
{ #d60b08}

	- 응답 본문에 메시지를 작성하고 싶을 때 쓰면 됨
		- ex) "Logout successful" 을 클라이언트에게 보낼 때
	- 응답 본문에 메시지를 작성할 필요가 없고, <u>단순히 상태 코드만 설정하려는 경우에는 호출하지 않아도 됨</u>



## ☑️ JWT 필터 적용하기
{ #76601a}



##### JwtAuthenticationFilter로 요청마다 JWT 토큰을 검증하여 인증 정보를 설정하기 [[Project/Book_rating_API/JWT 토큰 검증 필터 설정하기\|JWT 토큰 검증 필터 설정하기]]
```java
.addFilterBefore(new JwtAuthenticationFilter(jwtUtil), UsernamePasswordAuthenticationFilter.class);
```


- **addFilterBefore** : 
	- Spring Security의 필터 체인에서 <u>특정 필터를 다른 필터 앞에 추가</u>하는 메서드

- **왜 `UsernamePasswordAuthenticationFilter` 앞에 배치하는가?**
	- Spring Security가 기본적으로 세션을 사용하는 방식이라서, 요청마다 세션을 검사하려고 하기 때문임.JWT를 사용하는 경우에는 세션이 아닌 JWT 토큰을 통해 인증을 유지하기 때문에, 모든 요청에서 세션 대신 토큰을 검사하도록 `JwtAuthenticationFilter`를 추가하는 것임

- `UsernamePasswordAuthenticationFilter`는 기본적으로 로그인 요청 시 사용자 이름과 비밀번호를 통해 인증을 처리하는 필터인데 이 앞에 쓰는 이유는 <u>로그인 시에도 JWT 검증이 우선되도록 하기위함임</u>


##### SessionCreationPolicy.STATELESS 사용으로 서버가 세션을 생성하거나 관리하지 않도록하기
```java
http  
        .sessionManagement((session) -> session  
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS));
```

- `SessionCreationPolicy.STATELESS` 설정을 통해 세션을 생성하지 않고, 요청마다 JWT로 인증 상태를 확인하는 무상태 인증 방식을 사용하게 된다.
