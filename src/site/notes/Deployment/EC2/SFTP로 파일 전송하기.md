---
{"dg-publish":true,"permalink":"/deployment/ec-2/sftp_파일_전송하기.md/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-15T01:20:33.058+09:00","updated":"2024-10-15T01:20:33.058+09:00"}
---


## SFTP (Secure File Transfer Protocol)란?
- ==SSH를 기반==으로 한 파일 전송 프로토콜 (22번 포트)
- 모든 전송 데이터를 암호화하여 ==보안을 강화==
- SSH처럼 ==서버와 연결하는 방법의 하나==이지만,  ==파일 전송에 특화==되어 있다

### sftp 연결하기
1. 인스턴스 우클릭-> connect
	![1.png](/img/user/images/1.png)

2. Example 복사 
	![2.png](/img/user/images/2.png)
3. 터미널에 접속해서  pem키가 있는 폴더로 이동
4. `ssh` 부분을 `sftp`로 변경
5. `put`  전송할 파일을 드래그해서 놓기
6. `exit`으로 종료하기
