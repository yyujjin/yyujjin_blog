---
{"dg-publish":true,"permalink":"/project/book-rating-api/jwt_토큰_검증하기/","dgPassFrontmatter":true,"noteIcon":""}
---


==JWT 토큰을 파싱하여 사용자 정보를 추출하는 역할==


👩‍💻 Spring Security는 JWT 토큰을 사용하는 인증 방식을 지원하지만, <u>실제로 JWT를 생성하고 파싱하며 검증하는 과정은 개발자가 구현해야 함</u>


### ✨ 작동 과정
 * JWT 토큰을 파싱하고, 유효한 경우 토큰에서 'username' 클레임을 추출하여 반환
 * @return 유효한 토큰의 경우 'username' 값, 유효하지 않으면 null 반환


```java
import io.jsonwebtoken.Claims;
import io.jsonwebtoken.Jwts;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Component;

@Component
public class JwtUtil {

    @Autowired
    private Environment env; // Spring의 Environment를 사용하여 애플리케이션 프로퍼티에 접근

    public String parseToken(String token) {
        // 'jwt.secret' 키를 통해 비밀 키를 환경 변수에서 가져옵니다.
        String secretKey = env.getProperty("jwt.secret");

        // 비밀 키가 설정되지 않은 경우 예외를 던집니다.
        if (secretKey == null) {
            throw new IllegalStateException("JWT Secret Key is not configured in environment properties.");
        }

        try {
            // JWT 파서를 생성하고 시크릿 키를 설정하여 서명을 검증할 준비를 합니다.
            Claims claims = Jwts.parser()
                    .setSigningKey(secretKey.getBytes()) // 시크릿 키를 바이트 배열로 변환하여 설정
                    .build() // JWT 파서 객체를 완성
                    .parseClaimsJws(token) // 주어진 토큰을 파싱하여 서명을 검증합니다.
                    .getBody(); // 검증된 토큰의 내용을 가져옵니다.

            // 'username' 클레임을 추출하여 반환
            return claims.get("username", String.class);
        } catch (Exception e) {
            e.printStackTrace(); // 예외 발생 시 스택 트레이스를 출력
            return null; // 유효하지 않은 토큰일 경우 null 반환
        }
    }
}
```


- 이후 시큐리티 필터에 추가해서 매요청마다 jwt 토큰을 검사한다.
	- [[Project/Book_rating_API/Spring security 필터 설정#^917ac3\|Spring security 필터 설정#^917ac3]]




###  🚨 jwt 토큰이 null이라서 생기는 오류
![스크린샷 2024-11-08 오후 4.31.33.png](/img/user/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-11-08%20%EC%98%A4%ED%9B%84%204.31.33.png)


- 이 문제는 token이 null이거나 빈 문자열일 때, `parseClaimsJws(token)`를 호출하려고 하면 CharSequence cannot be null or empty 오류가 발생한다.


###### ✔️ 방법 1: 특정 URL에 대해 `JwtAuthenticationFilter` 비활성화

`JwtAuthenticationFilter` 클래스에서 `shouldNotFilter` 메서드를 오버라이드하여 특정 URL이나 조건에 따라 필터를 비활성화할 수 있습니다.

```java
@Override  
protected boolean shouldNotFilter(HttpServletRequest request) throws ServletException {  
  
    String[] excludePath = {"/books", "/books/*/reviews", "/tags"};  
    String path = request.getRequestURI();  
  
    return Arrays.stream(excludePath).anyMatch(path::startsWith);  
}
```

❌<u>  경로 설정이 먹히지 않아 일단 이 부분은 주석처리 함 </u>



###### 🎉방법2 : token 이 없을경우 null반화해서 밑에 검사로 안내려가게 하기

- `parseClaimsJws(token)`를 호출하기 전에 `token`이 `null` 또는 빈 문자열인지 확인하고 null로 바로 리턴시켜서 밑에<u> 검사로 내려가게 하는 걸 막으면 된다.</u>

```java
public String parseToken(String token) {  


	//👋 이 부분 추가함 
    if (token == null || token.trim().isEmpty()) {  
        return null; // 유효하지 않은 토큰으로 간주하고 null 반환  
    }  
  
    String secretKey = env.getProperty("jwt.secret");  
  
    if (secretKey == null) {  
        throw new IllegalStateException("JWT Secret Key is not configured in environment properties.");  
    }  

    try {  
        Claims claims = Jwts.parser()  
                .setSigningKey(secretKey.getBytes())  
                .build()  
                .parseClaimsJws(token)  
                .getBody();  
        return claims.get("username", String.class);  
    } catch (Exception e) {  
        e.printStackTrace();  // 필요에 따라 로그 또는 사용자 정의 예외로 변경 가능  
        return null;  // 토큰이 유효하지 않으면 null 반환  
    }  
}
```


- 토큰이 없을 경우 null 이 반환되므로  필터 검증에서  username 은 null 이된다.
- null로 반환되면 인증 객체가 생성되지 않으므로 `SecurityContextHolder`에 인증 정보가 설정되지 않는다.
```java
try {  
    String username = jwtUtil.parseToken(token);  
    UsernamePasswordAuthenticationToken authToken =  
            new UsernamePasswordAuthenticationToken(username, null, new ArrayList<>());  
    SecurityContextHolder.getContext().setAuthentication(authToken);  
}
```
