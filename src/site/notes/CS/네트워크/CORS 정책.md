---
{"dg-publish":true,"permalink":"/cs/네트워크/cors/","dgPassFrontmatter":true,"noteIcon":""}
---


##  CORS가 뭘까?

- CORS(Cross-Origin Resource Sharing)는 출처가 다른 자원들을 공유한다는 뜻으로 한 출처에 있는 자원에서 ==다른 출처에 있는 자원에 접근==하도록 하는 개념을 말한다. 
- ==HTTP 헤더==를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 ==권한을 부여==하도록 브라우저에 알려주는 체제이다.


## 출처란?

![Pasted image 20241011144754.png](/img/user/images/Pasted%20image%2020241011144754.png)
사진 출처 : https://beomy.github.io/tech/browser/cors/


**위의 구성요소 중에서 Protocol + Host + Port 3가지가 같으면 동일 출처(Origin)라고 한다.**


## CORS 정책이 없을 경우 생기는 문제

 *예시 : 사용자 계좌에서 무단 이체*

1. A 사용자가 `mybank.com`에 로그인한 상태로 `malicious.com`에 접속합니다.
    
2. `malicious.com` 웹사이트는 A 사용자가 `mybank.com`에 로그인한 상태라는 것을 알아차립니다.
    
3. `malicious.com`은  `mybank.com`의 계좌 이체 API를 호출합니다.

4. **CORS 정책이 없는 브라우저**는 `malicious.com`의 이 요청을 `mybank.com`으로 허용합니다.
    
5. `mybank.com`은 이 요청이 `malicious.com`에서 온 것임을 모른 채, 로그인된 A 사용자의 계좌에서 돈을 1000원 이체합니다.
    
6. 결과적으로, A 사용자는 자신도 모르는 사이에 `malicious.com`에서 지정한 공격자의 계좌로 돈을 이체하게 됩니다.



##  동일 출처 정책(Same-Origin Policy)

- 기본적으로 브라우저에서 보안상의 이유로 동일 출처 정책(Same-Origin Policy)을 적용하여, ==브라우저가 다른 출처의 요청을 자동으로 차단==한다.
- 이 설정을 변경하려면 서버에서 CORS 정책을 *명시적으로 허용*해주어야 한다.




## CORS 허용 설정 방법 3가지

1. **응답 헤더를 통해 허용 설정** (서버 측에서 `Access-Control-Allow-Origin` 등 설정)
2. **프로그래밍 언어 또는 프레임워크에서 CORS 설정** (Java Spring, Express.js, Django 등)
3. **웹 서버 설정 파일을 통해 CORS 설정** (Nginx, Apache 등)

이 3가지중 나는 첫 번째 방법을 적용시켜보았다.

👉 [[CORS 허용하기\|CORS 허용하기]]




참고 사이트 : https://escapefromcoding.tistory.com/724
