---
{"dg-publish":true,"permalink":"/project/book-rating-api/jwt/","dgPassFrontmatter":true,"noteIcon":""}
---


==JWT 토큰을 파싱하여 사용자 정보를 추출하는 역할==


👩‍💻 Spring Security는 JWT 토큰을 사용하는 인증 방식을 지원하지만, <u>실제로 JWT를 생성하고 파싱하며 검증하는 과정은 개발자가 구현해야 함</u>


**✨ 작동 과정**
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





- 이후 시큐리티 필터에 추가해서 요청마다 jwt 토큰을 검사한다.
	- [[Project/Book_rating_API/Spring security 필터 설정#^917ac3\|Spring security 필터 설정#^917ac3]]


