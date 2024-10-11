---
{"dg-publish":true,"permalink":"/java/security/cors-cross-origin/","dgPassFrontmatter":true,"noteIcon":""}
---


👀 CORS 가 뭐지? [[CS/Web/Security/CORS 정책\|CORS 정책]]


# @CrossOrigin  어노테이션이란?

`@CrossOrigin` 애너테이션은 ==Spring Framework에서 제공하는 간단한 CORS 설정 방법==이다.



# 사용 방법

```java
@CrossOrigin //전체허용
@CrossOrigin(origins = "*") //전체허용
@CrossOrigin(origins = "http://localhost:3000"(허용할 출처), allowCredentials = "true") //인증정보를 포함하는 요청
```


# 설정 방법


## 컨트롤러 클래스에 적용

해당 컨트롤러의 모든 메서드에 대해 CORS 정책이 적용된다.

```java
@RestController  
@CrossOrigin 
public class ReviewController {
...
}
```


## 개별 메서드에  적용

특정 메서드에만 CORS 정책이 적용된다.
```java
@RestController
public class AnotherController {
	@CrossOrigin(origins = "https://anotherapp.com") // 특정 메서드에만 CORS 정책 적용@GetMapping("/info")
	public String getInfo() { 
	...
	}
}
```