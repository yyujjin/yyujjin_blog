---
{"dg-publish":true,"permalink":"/access-token-refresh-token/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-11-06T19:51:28.490+09:00","updated":"2024-11-06T19:59:19.728+09:00"}
---



jwt 토큰은 유효기간을 정해줘야한다.
그런데 유효기간을 짧게 두면 사용자가 로그인을 자주 해야하므로 사용자 경험적으로 좋지 않고, 유효기간을 길게 두면 보안상 탈취 위험에서 벗어날 수 없다.
이런 문제점을 해결하기 위해 Access Token과 Refresh Token이 필요하다.

- Access Token
	- **유효기간은** **짧다**. 
- Refresh Token
	-  **유효기간은** **길다**.
	
평소에 ==API 통신할 때는 Access Token을 사용==하고, Refresh Token은 ==Access Token이 만료되어 갱신될 때==만 사용한다.

즉, 통신과정에서 <u>탈취당할 위험이 큰 Access Token의 만료 기간을 짧게 두고 Refresh Token으로 주기적으로 재발급</u>함으로써 피해을 최소화한 것이다.

## ☑️ 동작 방식

1. 로그인 인증에 성공한 클라이언트는 `Refresh Token`과 `Access Token` 두 개를 **서버로부터 받는다**.
2. 클라이언트는 `Refresh Token`과 `Access Token`을 **로컬**에 저장해놓는다.
3. 클라이언트는 **헤더**에 Access Token을 넣고 API 통신을 한다. **(Authorization)**
4. 일정 기간이 지나 `Access Token`의 **유효기간이 만료**되었다.  
    4.1. Access Token은 이제 유효하지 않으므로 **권한이 없는 사용자**가 된다.  
    4.2. 클라이언트로부터 유효기간이 지난 Access Token을 받은 서버는 [401 (Unauthorized)](https://www.rfc-editor.org/rfc/rfc6750#section-6.2.2) 에러 코드로 응답한다.  
    4.3. `401`를 통해 클라이언트는 `invalid_token` (유효기간이 만료되었음)을 알 수 있다.
5. **헤더**에 Access Token 대신 `Refresh Token`을 넣어 **API를 재요청**한다.
6. Refresh Token으로 사용자의 권한을 확인한 서버는 **응답쿼리 헤더**에 **새로운 Access Token**을 넣어 응답한다.
7. 만약 `Refresh Token`도 **만료**되었다면 서버는 동일하게 **401 error code**를 보내고, 클라이언트는 **재로그인**해야한다.


## ☑️ Refresh Token 탈취 위험

`Refresh Token`의 통신 빈도가 적기는 하지만 탈취 위험에서 완전히 벗어난 것은 아니다. `Refresh Token`이 탈취당하는 건 예방할 수 있을까? [OAuth](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)에서는 **Refresh Token Rotation**을 제시한다.

Refresh Token Rotation은 클라이언트가 Access Token를 재요청할 때마다 Refresh Token도 새로 발급받는 것이다.

이렇게 되면 탈취자가 가지고 있는 Refresh Token은 더이상 만료 기간이 긴 토큰이 아니게 된다. 따라서 불법적인 사용의 위험은 줄어든다.




---
출처 : [Access-Token과-Refresh-Token이란-무엇이고-왜-필요할까](https://velog.io/@chuu1019/Access-Token%EA%B3%BC-Refresh-Token%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%99%9C-%ED%95%84%EC%9A%94%ED%95%A0%EA%B9%8C)
