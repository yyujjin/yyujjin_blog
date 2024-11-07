---
{"dg-publish":true,"permalink":"/cors/","dgPassFrontmatter":true,"noteIcon":""}
---


`WebMvcConfigurer`의 `addCorsMappings` 메서드를 오버라이드하여 설정할 수 있다.

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**") // 모든 경로에 대해 CORS 허용
                .allowedOrigins("http://localhost:3000") // 허용할 도메인 설정
                .allowedMethods("GET", "POST", "PUT", "DELETE") // 허용할 HTTP 메서드 설정
                .allowCredentials(true); // 쿠키 허용
    }
}
```