---
{"dg-publish":true,"permalink":"/deployment/docker/docker/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-15T01:20:33.071+09:00","updated":"2024-11-02T05:46:34.047+09:00"}
---

## 이미지란?

- **도커 이미지**는 **소프트웨어 실행 환경**을 담고 있는 ==설치 패키지== 같은 개념
- **이미지 빌드**는 Docker 컨테이너를 실행할 수 있는 ==실제 실행 파일(이미지)을 생성하는 과정==
- 이미지를 실행하면 ==컨테이너==가 됨


## 만드는 방법

### 1. 프로젝트에 `Dockerfile` 생성하기


### 2. 코드 작성하기

```java
FROM openjdk:17-alpine
COPY ./build/libs/demo-0.0.1-SNAPSHOT.jar(빌드파일) /build/libs/demo-0.0.1-SNAPSHOT.jar(빌드파일)
CMD ["java","-jar","/build/libs/demo-0.0.1-SNAPSHOT.jar"(빌드파일)] 
```
*카피경로 저렇게 만들 필요음슴*


**먼저 IntelliJ에서 빌드한 후 Docker 빌드를 실행하는 방식**


`FROM` : 도커 이미지의 ==기본 이미지==를 설정하는 명령어

- `openjdk:17` : java17을 사용하겠다.
- `alpine`
    - 도커에서도 **컨테이너 안에서 실행될 운영체제**를 설정해줘야 한다.
    - 그래서 `alpine`은 컨테이너가 **Alpine 리눅스**라는 매우 가벼운 리눅스 운영체제 위에서 동작하도록 설정하겠다는 뜻이다.

**즉, Java 17 환경을 제공하는 Alpine 리눅스 이미지를 기반으로 하겠다는 의미**


`COPY`

```shell
//COPY (복사할 파일[로컬]) (복사될 위치[이미지 내부])

COPY ./build/libs/bookrating-0.0.1-SNAPSHOT.jar /book
```

- 로컬 경로와 Docker 이미지 내부 경로는 반드시 동일할 필요는 없음
- **복사**한다는 의미는 내 컴퓨터(로컬)의 특정 파일을 이미지 내부의 특정 위치에 넣어두는 작업

위의 경우:

- Docker 이미지 내부에는 `/book` 디렉터리가 생성됨
- `/book` 디렉터리 안에 `bookrating-0.0.1-SNAPSHOT.jar` 파일이 포함됨
- 이렇게 이미지 내부에 복사된 파일은 컨테이너가 실행될 때 **컨테이너 내부의 `/book` 경로에서 접근**할 수 있음

- 새롭게 만들었던 폴더나 파일들은 컨테이너 내부가 아닌 **외부**에 존재함
- 빌드한 jar 파일을 COPY 명령어를 사용해서 Docker 컨테이너 내부로 옮겨주는 코드이다.
- 두 위치가 동일해보이지만 그렇지 않다.
- 도커에서는 **호스트 파일 시스템(컨테이너 외부)**과 **컨테이너 내부 파일 시스템**이 분리되어 있기 때문에, 경로가 같아 보이더라도 실제로는 **서로 다른 공간**임


`CMD` : 컨테이너가 시작될 때 ==실행할 기본 명령어==를 설정

- 이렇게 설정해두면, 컨테이너를 실행할 때 따로 실행 명령을 입력하지 않아도 `/book/bookrating-0.0.1-SNAPSHOT.jar` 파일이<u> 자동으로 실행</u>됨
- 자바 실행파일 명령어가 `java -jar 실행파일 이름 ` 이라서 => "java","-jar" 이게 붙는거임

### 3. 빌드하기

1. 터미널 열어서 **Dockerfile**이 있는 디렉토리로 이동
    
2. 명령어 입력하기
    
    ```shell
    docker build . -t 이미지이름
    ```
    

- `docker build` : 도커 이미지를 빌드해라
- `.` : 현재 디렉토리에서
- `t` : 태그(tag)를 붙이는 옵션
- `springbootapp` : 빌드된 이미지에 붙여질 _이름_

**즉, 빌드하고 내가 지은 이름으로  저장하겠다!!** 라는 의미

### 4 . 포트 설정 후 실행하기

- 로컬에서 실행하는 방법
- 배포 서버에서 실행하고 싶다면 [여기로 이동]([[Deployment/Docker/docker 배포하기\|docker 배포하기]]) 
```shell
docker run -p 로컬 포트번호:컨테이너 내부 포트번호 태그이름
```





참고 사이트  :  [https://ttl-blog.tistory.com/761?category=947731](https://ttl-blog.tistory.com/761?category=947731)


