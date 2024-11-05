---
{"dg-publish":true,"permalink":"/deployment/docker/env_파일_포함시키기.md/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-30T18:08:48.089+09:00","updated":"2024-11-05T19:34:52.994+09:00"}
---


`.env` 파일은 빌드 시 포함되지 않는다.

기존의 `Dockerfile`을 이렇게 설정해두었다가 
```java
FROM openjdk:17-alpine  
COPY ./build/libs/bookrating-0.0.1-SNAPSHOT.jar /build/libs/bookrating-0.0.1-SNAPSHOT.jar  
CMD ["java","-jar","/build/libs/bookrating-0.0.1-SNAPSHOT.jar"]
```

- `WORKDIR` 를 추가해서 이 경로를 기준으로 실행되게 설정하였고
- `.env` 파일도 java 실행파일이 있는 경로로 옮겨두었다.
- 그래서 컨테이너 실행시에 `/build/libs` 해당 경로에 있는것들이 실행되고 이 경로안에있는 `.env`와 `jar`파일이 실행된거다.
- copy 개념에 대해 이해를 잘 하지 못해서 블로그에 있는 경로로 그대로 복사했는데 개념에 대해서 공부하고 난후에는 <u>copy 경로가 로컬 경로와 똑같이 만들 필요가 없다는걸</u> 깨달았당 😚

```java
FROM openjdk:17-alpine  
WORKDIR /build/libs 
COPY ./build/libs/bookrating-0.0.1-SNAPSHOT.jar /build/libs/bookrating-0.0.1-SNAPSHOT.jar  
COPY .env /build/libs/  
CMD ["java","-jar","/build/libs/bookrating-0.0.1-SNAPSHOT.jar"]
```

