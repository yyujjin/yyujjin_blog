---
{"dg-publish":true,"permalink":"/docker/","dgPassFrontmatter":true,"noteIcon":""}
---


### 이미지란?
**도커 이미지**는 **소프트웨어 실행 환경**을 담고 있는 ==설치 패키지== 같은 개념

### 만드는 방법

1. 프로젝트에 `Dockerfile`을 생성한다.
	![Pasted image 20241013133625.png](/img/user/images/Pasted%20image%2020241013133625.png)
2.  코드 작성하기
	```java
	FROM openjdk:17-alpine
	CMD ["java","-jar","/build/libs/demo-0.0.1-SNAPSHOT.jar"]
	```

	`FROM` : 도커 이미지의 ==**기본 이미지**==를 설정하는 명령어
	-  java17을 사용하겠다.
	- 도커에서도 **컨테이너 안에서 실행될 운영체제**를 설정해줘야 한다.  그래서 `alpine`은 컨테이너가 ==**Alpine 리눅스**==라는 매우 가벼운 리눅스 운영체제 위에서 동작하도록 설정하겠다는 뜻이다.
	*즉, Java 17 환경을 제공하는 Alpine 리눅스 이미지를 기반으로 하겠다는 의미*

	**`CMD`**: 컨테이너가 시작될 때 ==**실행할 기본 명령어**==를 설정
	- `["java", "-jar", "/build/libs/demo-0.0.1-SNAPSHOT.jar"]`: 이 명령어는 컨테이너가 실행될 때 **Java 애플리케이션**을 실행하겠다는 의미
	    
	- **`java`**: Java 실행 명령어. 도커 이미지의 OpenJDK 환경에서 Java 애플리케이션을 실행한다.
	    
	- **`-jar`**: Java 실행 옵션으로, 특정 **JAR 파일**을 실행하겠다는 뜻
	    
	- **`/build/libs/demo-0.0.1-SNAPSHOT.jar`**: 실행할 **JAR 파일**의 경로를 나타냄.
	    
	
	즉, `/build/libs/demo-0.0.1-SNAPSHOT.jar`라는 이름의 JAR 파일을 실행하여 Java 애플리케이션을 시작하라"는 의미
3. 빌드하기
	1. 터미널 열어서 **Dockerfile**이 있는 디렉토리로 이동
	2. 명령어 입력하기
		```shell
		docker build . -t springbootapp
		```
	- `docker build` :  도커 이미지를 빌드해라
	- `.` : 현재 디렉토리에서
	- `-t` : 태그(tag)를 붙이는 옵션 
	- `springbootapp` : 빌드된 이미지에 붙여질 이름
	즉, 빌드하고 내가 지은 이름으로 태그를 달아 저장하겠다!! 라는 의미 
4. 실행시키기
	- 빌드된 이미지는 도커가 관리하는 로컬 시스템의 이미지 저장소에 저장되어있기 때문에 해당 명령어를 실행하는 위치는 아무데서나 해도 상관없다. 
		```shell
		docker run springbootapp
		```
		⚠️ 에러 발생
		```shell
		park-yujin@bag-yujin-ui-MacBookPro ~ % docker run book-rating-backend
		Error: Unable to access jarfile /build/libs/bookrating-0.0.1-SNAPSHOT.jar
		```
		-  jar 파일의 경로가 잘못되었거나,  존재하지 않는 등의 이유로 jar 파일을 찾을 수 없을 때 발생하는 에러