---
{"dg-publish":true,"permalink":"/cs//webhook/","dgPassFrontmatter":true,"noteIcon":""}
---


##  웹훅이란?

-  <font color="#00b0f0">HTTP 기반의 콜백 함수</font>
- 이벤트가 발생했을 때 서버로부터 실시간으로 데이터를 받을 수 있음 
- 이벤트 기반으로 서버가 클라이언트에 데이터를 전송하는 방식

## 작동 방법

- 클라이언트는 서버에게 웹훅을 받을 URL과  받고 싶은 이벤트를 등록한다.
- 등록한 이벤트가 발생하면 서버는 클라이언트가 제공한 URL로 HTTP 요청을 통해 이벤트 데이터를 전송한다.


## 웹훅과 API폴링의 차이점


###### 폴링(Polling)이란?

폴링은 클라이언트가 서버에<u> 주기적으로 요청을 보내서</u>, 새로운 데이터나 업데이트가 있는지 확인하는 방식

![스크린샷 2024-10-18 오후 3.25.13.png](/img/user/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-10-18%20%EC%98%A4%ED%9B%84%203.25.13.png)


 - 친구와 전화하는 비유를 들자면 API <font color="#d83931">폴링</font>은 친구가 받을 때까지 계속 <font color="#d83931">전화</font>📞하는 것과 같고
 - <font color="#245bdb">웹훅</font>은 친구에게 "시간 나면 전화 줘"라고<font color="#245bdb"> 문자</font>💌를 남기는 것과 같다.

`폴링` : 클라이언트가 **서버 API**를 호출해서 이벤트가 발생했는지 계속 확인해야 함

`웹훅` : 한 번 설정하면 클라이언트가 추가 요청을 보내지 않아도 됨


**즉,폴링은 주기적인 확인 요청이 필요하고, 웹훅은 이벤트 등록 이후에는 주기적인 확인 요청이 필요 없다.**


✅ API 폴링은 주기를 60초에서 120초로 설정하는 것을 가장 추천하는데,  그렇다면 실시간으로 이벤트 데이터를 받기 어렵다. 하지만 웹훅을 사용하면 이벤트가 발생한 즉시 데이터를 받을 수 있다.



출처 : https://docs.tosspayments.com/resources/glossary/webhook