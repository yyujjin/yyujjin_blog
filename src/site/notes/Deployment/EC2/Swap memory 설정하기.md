---
{"dg-publish":true,"permalink":"/deployment/ec-2/swap-memory/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-11-02T02:57:56.346+09:00","updated":"2024-11-05T19:42:21.197+09:00"}
---


#EC2 #Docker
## 스왑 메모리(Swap Memory)란?

- ==물리적 메모리(RAM)가 부족할 때 하드 디스크나 SSD의 일부를 가상 메모리로 사용하는 공간을 말함==
- 스왑 메모리는 주로 리눅스와 같은 운영 체제에서 사용되며, 메모리 관리를 보다 효율적으로 하기 위해 활용된다.
- 컴퓨터가 여러 프로그램을 동시에 실행하면 **물리적 메모리(RAM)가 가득 차는 경우**가 발생할 수 있다. 이때 스왑 메모리가 있으면, RAM이 부족할 때 <u>사용하지 않는 데이터를 하드 디스크의 스왑 영역으로 옮겨 메모리를 확보하고  </u>이후 프로그램을 다시 사용할 때는 스왑에서 데이터를 다시 RAM으로 불러온다. 
- 스왑 메모리는 하드 디스크나 SSD의 일부를 사용하기 때문에, RAM에 비해 **속도가 훨씬 느리다.** 따라서 스왑 메모리를 자주 사용하게 되면 시스템 성능이 저하될 수 있다.



## 😢 내가 겪은 문제점


AWS의 EC2를 사용하면 램 메모리가 1GB 밖에 되지 않기 때문에 메모리가 부족한 현상을 겪을수 있다고 한다.
서버에 프로그램을 돌렸을 때 인스턴스가 계속 응답을 하지 않는 상황때문에 인스턴스 재시작을 5번정도 한것같다..
그래서 찾게 된 첫번째 방법이 하위 방법들이고 블로그를 참고해서 스왑 메모리를 설정해봤다!
1. cpu사용량 제한하기
2. <u>스왑 메모리 설정하기 ☑️</u>



참고한 블로그👍: [https://diary-developer.tistory.com/32](https://diary-developer.tistory.com/32) [일반인의 웹 개발일기:티스토리]