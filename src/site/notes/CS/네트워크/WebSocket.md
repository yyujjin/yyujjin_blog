---
{"dg-publish":true,"permalink":"/cs//web-socket/","dgPassFrontmatter":true,"noteIcon":""}
---


## 웹소켓이란?

- 클라이언트와 서버 간에 <font color="#d83931">양방향 통신</font>을 가능하게 하는 <font color="#d83931">프로토콜</font>
- <font color="#d83931">연결이 지속</font>되어 실시간으로 데이터를 주고받을 수 있는 특징이 있음
- 실시간 채팅, 금융 서비스, 온라인 게임 등에 사용된다.



## HTTP vs 웹소켓 차이

##### 웹소켓 프로토콜과 HTTP 프로토콜의 차이점
- <font color="#ffc000">HTTP</font>  프로토콜은<font color="#ffc000"> 단방향 통신</font>으로 클라이언트(보통 브라우저)가 서버에 요청(request)을 보내면, 서버는 그 요청에 대한 응답(response)을 반환한다. 이 과정은 요청을 보낼 때마다 이루어진다.
- 하지만 <font color="#92d050">웹소켓</font>은 한 번 연결이 이루어지면 클라이언트와 서버 간에 <font color="#92d050">지속적인 연결을 유지하며 양방향</font>으로 데이터를 주고 받을 수있다.




##### 웹소켓의 핸드셰이크 과정

![스크린샷 2024-10-18 오후 3.43.48.png](/img/user/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-10-18%20%EC%98%A4%ED%9B%84%203.43.48.png)
1. 클라이언트가 웹소켓으로 전환하겠다는 의사를 HTTP 요청으로 보냄
2. 서버가 이를 수락하고 웹소켓 연결로 업그레이드시킴
3. 위 과정을 핸드쉐이크라고 부름(클라이언트와 서버가 HTTP 프로토콜을 사용하여 서로 웹소켓 연결로 전환하는 의사를 교환하는 과정)
4. 핸드셰이크가 완료되면, 더 이상 HTTP 프로토콜을 사용하지 않고 웹소켓 프로토콜을 사용하게됨


**즉, 웹소켓은 핸드셰이크에서 HTTP를 사용하지만, 그 이후에는 WebSocket 프로토콜을 통해 지속적인 양방향 통신을 지원함**





참고 사이트  : 

- https://duckdevelope.tistory.com/19
-  [https://code-lab1.tistory.com/300](https://code-lab1.tistory.com/300) [코드 연구소:티스토리]