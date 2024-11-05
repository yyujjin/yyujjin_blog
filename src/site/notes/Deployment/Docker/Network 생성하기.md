---
{"dg-publish":true,"permalink":"/deployment/docker/network/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-11-02T03:57:20.648+09:00","updated":"2024-11-05T19:35:14.160+09:00"}
---


#EC2 #Docker 

<u>앱과 db는 서로 다른 컨테이너에서 실행</u>되기에 서로 통신할 수 있어야 한다. 
그러기위해 네트워크를 생성해서 해당 ==네트워크로 두 컨테이너를 실행하면 서로 연결==되어서 앱이 디비에 접근할 수 있다. 


==🍎 먼저, ec2 인스턴스에 접속하기==


##### 네트워크 생성하기
```shell
docker network create app-network
```

##### 컨테이너 실행 시 네트워크에 연결하기

☑️cpu는 30%로 걸어둠
☑️ 컨테이너 이름을 지정하면 다른 컨테이너에서 설정한 `name`으로 접근할 수 있음 
☑️db url은 꼭 `""`붙이기

**MySQL 컨테이너**
```shell
sudo docker run --cpus=".3" --name 컨테이너이름지정 --network app-network -e MYSQL_ROOT_PASSWORD=[비밀번호] -e MYSQL_DATABASE=[스키마이름] -p 3306:3306 -d 이미지이름:태그
```

```shell
sudo docker run --cpus=".3" --name db --network app-network -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=bookrating -p 3306:3306 -d mysql:latest
```


**스프링 애플리케이션 컨테이너**
```shell
sudo docker run --cpus=".3" --name 컨테이너이름지정 --network app-network -p 8080:8080 -e "DB_URL=jdbc:mysql://MySQL컨테이너name:3306/bookrating?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true" -d 이미지 id
```

```shell
sudo docker run --cpus=".3" --name app --network app-network -p 8080:8080 -e "DB_URL=jdbc:mysql://db:3306/bookrating?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true" -d sha256:786fed5d90bafb5a40afe4366fbc811c736d1c8b1700278d2322683f2c8bc8d2
```




