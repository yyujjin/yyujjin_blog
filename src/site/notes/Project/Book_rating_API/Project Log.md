---
{"dg-publish":true,"permalink":"/project/book-rating-api/project-log/","dgPassFrontmatter":true,"noteIcon":""}
---



- `.env` 파일을 찾을 수 없다는 오류
	- [[Deployment/Docker/env 파일 포함시키기\|env 파일 포함시키기]]
- 서버가 뻗는 오류 (🤦‍♀️ 인스턴스 재실행만 5번 정도 한듯...히유..) 
	- [[Deployment/Docker/서버 사용량 제한하기\|서버 사용량 제한하기]]
	- [[Deployment/EC2/Swap memory 설정하기\|Swap memory 설정하기]]
- 앱과 db 컨테이너가 서로 연결돼서 실행되도록 설정
	- [[Deployment/Docker/Network 생성하기\|Network 생성하기]]
- Docker로 DB 컨테이너 실행하기
	- [[Deployment/Docker/Database Migration\|Database Migration]]
- 소스 코드 보안 유지하기
	- [[Project/Book_rating_API/환경변수 설정하기\|환경변수 설정하기]]
- CORS 설정하기
	- [[Project/Book_rating_API/CORS 허용 설정하기\|CORS 허용 설정하기]]
- JWT 설정하기
	- [[Project/Book_rating_API/JWT 토큰 생성하기\|JWT 토큰 생성하기]]
	- [[Project/Book_rating_API/JWT토큰 검증하기\|JWT토큰 검증하기]]
	- [[Project/Book_rating_API/JWT 토큰 검증 필터 설정하기\|JWT 토큰 검증 필터 설정하기]]
	- 시큐리티에 적용한 필터 넣기[[Project/Book_rating_API/Spring security 필터 설정#^76601a\|Spring security 필터 설정#^76601a]]
- 로그인 불필요 경로에서 JWT 토큰 검사로 인한 에러 발생
	- [[Project/Book_rating_API/JWT토큰 검증하기#^4cf6b1\|JWT토큰 검증하기#^4cf6b1]]
