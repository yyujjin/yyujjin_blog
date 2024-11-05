---
{"dg-publish":true,"permalink":"/deployment/docker/docker_배포하기/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-15T22:32:52.408+09:00","updated":"2024-10-30T23:20:24.906+09:00"}
---


### 1. Docker 이미지 태그 설정하기

- 빌드된 이미지에 **식별자**를 붙여 사용하기 쉽게 만드는 과정 

```bash
docker tag [이미지해쉬값] [DockerHub사용자명]/[이미지이름]:[태그]
```

*예시*
```shell
 docker tag sha256:9dcd173770be56258d1f467b6a7ccd77e1c121e36105ee7451ff9f161faa42a3 yyujjin1204/book:latest
```


####  🤫 레포지토리와 태그 이해하기

```shell
docker tag 이미지해쉬값 yyujjin1204/book-rating-backend:latest
docker tag 이미지해쉬값 yyujjin1204/book-rating-backend:v1.0
```

- `yyujjin1204/book-rating-backend`: 레포지토리 이름으로, `book-rating-backend` 애플리케이션을 의미합니다.
- `latest`와 `v1.0`: 태그로, 같은 애플리케이션이라도 버전별 또는 배포 상태별로 구분할 수 있습니다.



### 2. Docker 이미지 푸시
```shell
docker push [DockerHub사용자명]/[이미지이름]:[태그]
```

*예시*
```shell
docker push yyujjin1204/book:latest
```


### 3. Hub에 업로드 확인하기
Docker Hub에 업로드된 이미지가 정상적으로 올라간걸 확인할 수 있다. 

**이미지를 EC2 인스턴스에 내려받아 컨테이너로 실행하는 과정**

`이미지화`  :  애플리케이션을 Docker 이미지로 만들고 저장.
`이미지를 EC2에 올린다`  :  EC2에 이미지를 다운로드하지만 아직 실행되지 않음.
`컨테이너로 실행`  :  이미지를 기반으로 컨테이너가 실행되며, 이때 애플리케이션이 EC2에서 구동.



🐶 ==EC2에 접속한다.==


### 4. EC2 인스턴스에 Docker 설치

EC2 인스턴스에 Docker가 설치되지 않았다면 먼저 설치한다.

`sudo`는 뭐지?
- superuser 권한으로 명령을 실행하기 위해 사용됩니다.
- Docker는 시스템의 리소스를 관리하고 접근하는 작업을 하기 때문에 루트 권한이 필요하기때문


*Docker 설치 (Ubuntu 기준)*
```shell
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

*Docker 설치 확인*
```shell
docker --version
```


여기서부터 하면 됨

### 5. Docker Hub에서 이미지 Pull하기

Docker Hub에 이미지를 EC2 인스턴스로 가져온다.
```shell
sudo docker pull [DockerHub사용자명]/[이미지이름]:[태그]
```

*예시*
```shell
sudo docker pull yyujjin1204/book:latest
```

### 6. 컨테이너 실행하기

*포트가 8080일 경우 예시*
```shell
sudo docker run -d -p 8080:8080 [DockerHub사용자명]/[리포지토리명]:[태그]
```

*예시*
```shell
sudo docker run -d -p 8080:8080 yyujjin1204/book-rating-backend:latest
```
`-d` :  백그라운드로 실행
`-p` : 포트설정

실행에 성공하면 컨테이너 ID 값이 나타난다.

### 7. 배포 확인하기

```shell
http://[EC2-퍼블릭-IP]:8080
```



