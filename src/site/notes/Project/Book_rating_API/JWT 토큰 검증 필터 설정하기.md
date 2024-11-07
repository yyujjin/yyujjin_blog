---
{"dg-publish":true,"permalink":"/project/book-rating-api/jwt/","dgPassFrontmatter":true,"noteIcon":""}
---


-  **`OncePerRequestFilter` 클래스**

	- <u>중복 실행 방지</u>를 위해 요청마다 한 번만 실행되도록 설정
	- 즉,<u> 같은 요청에 대해 필터를 한 번만 실행</u>하기 위함
	
	아래와 같은 상황을 가정해 보겠습니다:
	
	1. 사용자 요청 :  사용자가 `/secured-page` 경로로 요청을 보냅니다.
	2. 필터 체인 실행 : 사용자의 요청이 Spring Security 필터 체인을 통과하며, 여러 필터가 실행됩니다.
	3. 인증 검사 및 리다이렉트 :
	    - 만약 사용자가 인증되지 않은 상태라면, `SecurityContext`를 확인하는 필터가 이를 감지하고 `/login` 페이지로 리다이렉트합니다.
	4. 리다이렉트 요청 발생 : 클라이언트가 `/login`으로 리다이렉트되면서 다시 요청을 보냅니다.
	5. 필터 체인 재실행 : `/login` 요청도 필터 체인을 거치므로, 같은 필터가 반복적으로 실행될 수 있습니다.
	
	- 만약 `JwtAuthenticationFilter`가 `OncePerRequestFilter`를 상속하지 않고 <u>일반 필터로 구현되어 있다면</u>
	- `/secured-page`로 요청이 들어올 때 한 번, `/login`으로 리다이렉트될 때 또 한 번,같은 요청에 대해 필터가 두 번 실행됨
	- JWT 인증 필터는 `/secured-page`와 `/login` 요청에 대해 각각 중복으로 JWT 검사를 수행하게 됩니다.
	- `OncePerRequestFilter`를 상속한 `JwtAuthenticationFilter`는 <u>요청당 한 번만 실행</u>되도록 보장되므로, 리다이렉트나 포워드 시에도 중복 검사가 발생하지 않습니다.
	- 예를 들어, `/secured-page` 요청 시 한 번 JWT 검사가 수행되고, `/login`으로 리다이렉트될 때는 동일 요청으로 판단하여 중복 검사를 수행하지 않습니다.


-  **`JwtUtil` 클래스**

	- JWT 토큰을 생성하고, 검증 및 파싱하는 유틸리티 클래스
	- 여기서는 <u>토큰 검증과 파싱에 필요한 기능을 제공</u>하기 위해 쓰였음

- **`doFilterInternal`  : 각 요청마다 JWT 토큰을 검증하고 인증을 설정**
```java
@Override  
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
```

-  **`getJwtFromCookies` : 요청 쿠키에서 "jwt" 이름을 가진 JWT 토큰을 추출**
```java
String token = getJwtFromCookies(request);
```

- "jwt"라는 이름의 쿠키를 찾아 그 값을 반환하는 메서드
- 이 메서드에서 반환된 값을 위해 `token`에 저장함
```java
private String getJwtFromCookies(HttpServletRequest request) {  
    if (request.getCookies() != null) {  
    // 쿠키 배열을 스트림으로 변환
        return Arrays.stream(request.getCookies()) 
                .filter(cookie -> "jwt".equals(cookie.getName()))  // "jwt" 이름의 쿠키를 찾음
                .map(Cookie::getValue)  // 쿠키의 값을 추출
                .findFirst()  // 첫 번째 쿠키 값 선택
                .orElse(null);  // 없다면 null 반환
    }  
    return null;  
}
```


- **`jwtUtil.parseToken(token)` : 서명이 유효한 경우 페이로드에서 사용자 이름(또는 ID)을 추출하여, 해당 요청이 인증된 사용자에게서 온 것임을 확인함**
```java
String username = jwtUtil.parseToken(token);
```


-  **`UsernamePasswordAuthenticationToken` : 인증된 사용자 정보를 담는 객체**
	```java
	new UsernamePasswordAuthenticationToken(username, null, new ArrayList<>());
	```
	- **`username`**: 사용자 이름 또는 사용자 ID로, 인증된 <u>사용자를 식별하기 위한 값</u>입니다. 여기서는 JWT 토큰에서 추출한 <u>사용자 이름이 전달</u>됩니다.
	- **`null`**: 비밀번호 자리입니다. JWT 인증 방식에서는 이미 인증된 토큰이기 때문에 비밀번호가 필요하지 않아 `null`로 설정합니다.
	- `new ArrayList<>()`: 사용자의 권한 목록입니다. 비어 있는 리스트를 설정할 수도 있고, 사용자 권한을 포함하여 설정할 수도 있습니다. 권한이 있다면 이 리스트에 추가하여 Spring Security에서 사용자의 역할이나 권한을 확인할 수 있습니다.

- **`SecurityContext` : 인증된 사용자를 표현하기 위해 사용되는 객체**
	- 인증된 사용자 정보와 권한을 담아 `SecurityContext`에 저장함


- **`doFilter` : 다음 필터로 요청을 넘겨줌**


- **`shouldNotFilter` : 특정 경로에 대해 이 필터를 실행하지 않도록 설정**
    
    - 모든 요청에 대해 JWT 토큰을 검사하지만, 로그인 없이 접근 가능한 경로에 대해서는 `shouldNotFilter` 메서드를 통해 <u>필터가 작동하지 않게 설정</u>함
	- 이를 통해 <u>인증이 필요 없는 경로에서는 토큰이 없어도 에러가 발생하지 않도록처리</u>할 수 있음






### 보완 및 개선 사항
{ #097763}


1. **필요 없는 어노테이션 제거**:
    
    - `@RestController`와 `@CrossOrigin`은 필터에는 불필요한 어노테이션이므로 제거해도 됩니다.
2. **SECRET_KEY 사용**:
    
    - `SECRET_KEY` 필드가 정의되어 있지만 코드 내에서 사용되지 않았습니다. 필요하다면 `JwtUtil`에서 토큰 검증에 사용하도록 하거나, 불필요하다면 제거할 수 있습니다.
3. **예외 처리 개선**:
    
    - 예외 처리에서 `request.setAttribute("error", "exception")`로 설정하는 대신, 에러 응답을 반환하거나 로깅을 추가하여 문제를 추적할 수 있도록 개선할 수 있습니다.
