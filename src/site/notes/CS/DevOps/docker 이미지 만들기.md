---
{"dg-publish":true,"permalink":"/cs/dev-ops/docker/","dgPassFrontmatter":true,"noteIcon":""}
---

## 이미지란?

**도커 이미지**는 **소프트웨어 실행 환경**을 담고 있는 ==설치 패키지== 같은 개념

## 만드는 방법

### 1. 프로젝트에 `Dockerfile` 생성하기


![Pasted image 20241013133625.png](/img/user/images/Pasted%20image%2020241013133625.png)

### 2. 코드 작성하기

```java
FROM openjdk:17-alpine
COPY ./build/libs/demo-0.0.1-SNAPSHOT.jar(빌드파일) /build/libs/demo-0.0.1-SNAPSHOT.jar(빌드파일)
CMD ["java","-jar","/build/libs/demo-0.0.1-SNAPSHOT.jar"(빌드파일)] 
```


**먼저 IntelliJ에서 빌드한 후 Docker 빌드를 실행하는 방식**


`FROM` : 도커 이미지의 ==기본 이미지==를 설정하는 명령어

- `openjdk:17` : java17을 사용하겠다.
- `alpine`
    - 도커에서도 **컨테이너 안에서 실행될 운영체제**를 설정해줘야 한다.
    - 그래서 `alpine`은 컨테이너가 **Alpine 리눅스**라는 매우 가벼운 리눅스 운영체제 위에서 동작하도록 설정하겠다는 뜻이다.

**즉, Java 17 환경을 제공하는 Alpine 리눅스 이미지를 기반으로 하겠다는 의미**


`COPY`

```
//COPY (복사할 파일[컨테이너 외부]) (복사될 위치[컨테이너 내부])

COPY ./build/libs/demo-0.0.1-SNAPSHOT.jar /build/libs/demo-0.0.1-SNAPSHOT.jar
```

- 새롭게 만들었던 폴더나 파일들은 컨테이너 내부가 아닌 **외부**에 존재함
- 빌드한 jar 파일을 COPY 명령어를 사용해서 Docker 컨테이너 내부로 옮겨주는 코드이다.
- 두 위치가 동일해보이지만 그렇지 않다.
- 도커에서는 **호스트 파일 시스템(컨테이너 외부)**과 **컨테이너 내부 파일 시스템**이 분리되어 있기 때문에, 경로가 같아 보이더라도 실제로는 **서로 다른 공간**임


`CMD` : 컨테이너가 시작될 때 ==실행할 기본 명령어==를 설정

- `/build/libs/demo-0.0.1-SNAPSHOT.jar`라는 이름의 JAR 파일을 실행하여 Java 애플리케이션을 시작하라는 의미
- 빌드 과정이 끝난 후 생성된 파일의 이름넣기
- JAR 파일 이름이 변경되거나, 새로운 버전이 빌드될 때마다 그 파일의 이름을 반영해 Dockerfile을 수정해야 함

### 3. 빌드하기

1. 터미널 열어서 **Dockerfile**이 있는 디렉토리로 이동
    
2. 명령어 입력하기
    
    ```
    docker build . -t 이미지이름
    ```
    

- `docker build` : 도커 이미지를 빌드해라
- `.` : 현재 디렉토리에서
- `t` : 태그(tag)를 붙이는 옵션
- `springbootapp` : 빌드된 이미지에 붙여질 _이름_

**즉, 빌드하고 내가 지은 이름으로  저장하겠다!!** 라는 의미

### 4 . 포트 설정 후 실행하기

```shell
docker run -p 로컬 포트번호:컨테이너 내부 포트번호 태그이름
```





참고 사이트  :  [https://ttl-blog.tistory.com/761?category=947731](https://ttl-blog.tistory.com/761?category=947731)


