---
{"dg-publish":true,"permalink":"/docker/","dgPassFrontmatter":true,"noteIcon":""}
---

## 이미지란?

**도커 이미지**는 **소프트웨어 실행 환경**을 담고 있는 ==설치 패키지== 같은 개념

## 만드는 방법

#### 1. 프로젝트에 `Dockerfile` 생성하기

![Pasted image 20241013133625.png](https://yyujjinblog.vercel.app/img/user/images/Pasted%20image%2020241013133625.png)

#### 2. 코드 작성하기

```java
FROM openjdk:17-alpine
CMD ["java","-jar","/build/libs/demo-0.0.1-SNAPSHOT.jar"]
```

`FROM` : 도커 이미지의 ==기본 이미지==를 설정하는 명령어

- `openjdk:17` : java17을 사용하겠다.
    
- `alpine`
    
    - 도커에서도 **컨테이너 안에서 실행될 운영체제**를 설정해줘야 한다.
    - 그래서 `alpine`은 컨테이너가 **Alpine 리눅스**라는 매우 가벼운 리눅스 운영체제 위에서 동작하도록 설정하겠다는 뜻이다.

**즉, Java 17 환경을 제공하는 Alpine 리눅스 이미지를 기반으로 하겠다는 의미**

`CMD` : 컨테이너가 시작될 때 ==실행할 기본 명령어==를 설정

- `/build/libs/demo-0.0.1-SNAPSHOT.jar`라는 이름의 JAR 파일을 실행하여 Java 애플리케이션을 시작하라는 의미

#### 3. 빌드하기

1. 터미널 열어서 **Dockerfile**이 있는 디렉토리로 이동
2. 명령어 입력하기
    
    ```shell
    docker build . -t springbootapp
    ```
    

- `docker build` : 도커 이미지를 빌드해라
- `.` : 현재 디렉토리에서
- `-t` : 태그(tag)를 붙이는 옵션
- `springbootapp` : 빌드된 이미지에 붙여질 _이름_

**즉, 빌드하고 내가 지은 이름으로 태그를 달아 저장하겠다!!** 라는 의미

#### 4. 실행시키기

```shell
docker run springbootapp
```

==⚠️ 에러 발생==

```shell
park-yujin@bag-yujin-ui-MacBookPro ~ % docker run book-rating-backend
Error: Unable to access jarfile /build/libs/bookrating-0.0.1-SNAPSHOT.jar
```

- jar 파일의 경로가 잘못되었거나,  존재하지 않는 등의 이유로 jar 파일을 찾을 수 없을 때 발생하는 에러
- 왜 발생하나?
    - 새롭게 만들었던 폴더나 파일들은 컨테이너 내부가 아닌 **외부**에 존재함
    - 따라서 **컨테이너 내부에는 빌드한 jar 파일이 없어서** jar 파일을 찾을 수 없다는 오류가 발생함

==💡해결 방법===

```shell
//COPY (복사할 파일[컨테이너 외부]) (복사될 위치[컨테이너 내부])

COPY ./build/libs/demo-0.0.1-SNAPSHOT.jar /build/libs/demo-0.0.1-SNAPSHOT.jar
```

- 빌드한 jar 파일을 COPY 명령어를 사용해서 Docker 컨테이너 내부로 옮겨주면 됨
- 두 위치가 동일해보이지만 그렇지 않음
- 도커에서는 **호스트 파일 시스템(컨테이너 외부)**과 **컨테이너 내부 파일 시스템**이 분리되어 있기 때문에, 경로가 같아 보이더라도 실제로는 **서로 다른 공간**임

### 수정된 Dockerfile:

Dockerfile

Copy code

`FROM openjdk:17-alpine WORKDIR /app COPY ./build/libs/bookrating-0.0.1-SNAPSHOT.jar /app/bookrating.jar CMD ["java", "-jar", "/app/bookrating.jar"]`

### 설명:

1. **`WORKDIR /app`**: 컨테이너 내부에서 기본 작업 디렉토리로 `/app` 디렉토리를 설정합니다. 이후 명령어들은 기본적으로 `/app` 디렉토리에서 실행됩니다.
2. **`COPY ./build/libs/bookrating-0.0.1-SNAPSHOT.jar /app/bookrating.jar`**: 호스트 시스템의 `./build/libs/bookrating-0.0.1-SNAPSHOT.jar` 파일을 컨테이너 내부의 `/app/bookrating.jar`로 복사합니다.
3. **`CMD ["java", "-jar", "/app/bookrating.jar"]`**: 복사한 `bookrating.jar` 파일을 **Java**로 실행합니다.

### 올바른 빌드 및 실행 절차:

1. **Dockerfile**이 있는 디렉토리에서 터미널을 열고, 다음 명령어를 실행합니다:
    
    bash
    
    Copy code
    
    `docker build . -t bookratingapp`
    
    이 명령어는 **Dockerfile**을 사용하여 이미지를 빌드하고, `bookratingapp`이라는 이름으로 태그를 달게 됩니다.
    
2. 이미지가 빌드된 후, 다음 명령어로 컨테이너를 실행합니다:
    
    bash
    
    Copy code
    
    `docker run bookratingapp`
    

### 요약:

- **첫 번째 경로**는 **호스트 시스템**에서 복사할 파일의 경로입니다.
    
- **두 번째 경로**는 **컨테이너 내부**에 복사될 위치입니다.
    
- 위와 같이 작성하면 JAR 파일이 컨테이너 내에서 제대로 복사되고, Java로 실행됩니다.
    
- **이미지 이름 확인**:
    
    - `docker run` 명령어에서 사용한 이미지 이름 **`book_rating_backend`**가 올바르게 빌드된 이미지의 이름과 일치해야 합니다.
        
    - 빌드된 이미지의 이름을 확인하려면 다음 명령어로 로컬에 있는 이미지 목록을 확인할 수 있습니다:
        
        bash
        
        Copy code
        
        `docker images`
        
        목록에 **`book_rating_backend`**가 포함되어 있는지 확인하고, 만약 이름이 다르다면 올바른 이름으로 수정해 실행하세요.
        
- **이미지 실행**:
    
    - 이미지를 빌드한 후, 다시 다음 명령어로 컨테이너를 실행합니다:
        
        bash
        
        Copy code
        
        `docker run book_rating_backend`