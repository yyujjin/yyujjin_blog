---
{"dg-publish":true,"permalink":"/cs//dns/","dgPassFrontmatter":true,"noteIcon":""}
---


![Untitled.png](/img/user/images/Untitled.png)
1. 브라우저에서 naver.com을 검색하고, 사용하고 있는 ==ISP DNS 서버==에게 도메인 주소에 해당하는 IP 주소를 요청함

	❓ISP(Internet Service Provider)서버란 <u>인터넷 서비스 제공자</u> (SKT, KT, LG U+와 같은 통신사들)
	
2.  ISP 서버에선 캐시 데이터가 없다는 걸 확인하고 ==루트 DNS 서버==에게 어디로 가야 하는지 요청함(캐시가 있다면 8로 건너 뜀.)

	❓Root DNS란? <u>DNS 시스템의 최상위 서버</u>로, 인터넷에 존재하는 모든 도메인 이름의 출발점 역할
	
3. . 루트 서버는 .com 도메인을 보고는 COM도메인을 관리하는 TLD DNS 서버 주소를 안내함.

	❓ TLD(Top-Level Domain)  DNS 서버란? 최상위 도메인을 관리하는 DNS 서버로, <u>도메인 이름의 최상위 부분을 담당</u>( .com, .org, .net) 

4. . COM 서버는 네이버 DNS 서버에서 해당 도메인이 관리되고 있는 걸 확인하고 안내함.

5. 네이버 DNS 서버는 naver.com = 12.123.123.123 이라는 정보를 확인하고 이 IP를 알려줌. 동시에 ISP 서버는 해당 정보를 캐시로 기록해 둠.

6. ISP 서버는 브라우저에게 힘들게 알아 낸 12.123.123.123 주소를 안내함.

7. 브라우저는 12.123.123.123 IP 주소를 갖고 있는 호스팅 서버에게 웹사이트를 출력하라고 요청함.

8. 드디어 보임.