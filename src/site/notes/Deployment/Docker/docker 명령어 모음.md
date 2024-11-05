---
{"dg-publish":true,"permalink":"/deployment/docker/docker/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-15T23:26:55.327+09:00","updated":"2024-10-30T18:57:02.279+09:00"}
---

- [[Deployment/Docker/docker 명령어 모음#현재 실행중인 컨테이너 확인\|#현재 실행중인 컨테이너 확인]]
- [[Deployment/Docker/docker 명령어 모음#모든 컨테이너 목록확인\|#모든 컨테이너 목록확인]]
- [[Deployment/Docker/docker 명령어 모음#컨테이너 실행\|#컨테이너 실행]]
- [[Deployment/Docker/docker 명령어 모음#컨테이너 멈춤\|#컨테이너 멈춤]]
- [[Deployment/Docker/docker 명령어 모음#로그 확인\|#로그 확인]]




###### 현재 실행중인 컨테이너 확인 
(멈춘 컨테이너나 종료된 컨테이너는 표시되지 않음)
```shell
sudo docker ps
```

###### 모든 컨테이너 목록확인
```shell
sudo docker ps -all
```

###### 컨테이너 실행
```shell
sudo docker start 컨테이너id
```

###### 컨테이너 멈춤
```shell
sudo docker stop 컨테이너id
```

###### 로그 확인
```shell
sudo docker logs 컨테이너id
```


###### 특정 컨테이너 삭제
```shell
docker rm 컨테이너id
```

