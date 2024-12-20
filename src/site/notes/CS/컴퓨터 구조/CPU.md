---
{"dg-publish":true,"permalink":"/cs/컴퓨터_구조/cpu.md/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-20T04:13:28.369+09:00","updated":"2024-11-05T05:24:18.578+09:00"}
---


![IMG_23083A39166C-1.jpeg](/img/user/images/IMG_23083A39166C-1.jpeg)
# ✔️ CPU(Central Processing Unit, 중앙처리장치)란?
프로그램의 명령어를 처리하는 핵심 부품이다.

## ☑️ ALU (계산하는 부품)

#### ALU가 받아들이는 정보
- 레지스터를 통해 **피연산자**를 받아들이고 제어장치로부터 수행할 연산을 알려주는 **제어 신호**를 받아들인다
- 받아들인 피연산자와 제어 신호로 산술 연산, 논리 연산 등 <u>다양한 연산을 수행</u>함

#### ALU가 내보내는 정보 
- **연산 결과**
	- 연산을 수행한 결과값은 바로 메모리에 저장되지 않고 일시적으로 레지스터에 저장됨
	- <u>CPU가 메모리에 접근하는 속도는 레지스터에 접근하는 속도보다 훨씬 느림</u>
	- 만약 ALU가 연산할 때 마다 결과를 메모리에 저장한다면 CPU는 메모리에 자주 접근하게 되고 프로그램 실행 속도를 늦추게 됨
	- 그래서 <u>ALU의 결괏값을 메모리가 아닌 레지스터에 우선 저장함</u>
	
- **플래그** (연산 결과에 대한 추가적인 상태 정보)
	- ![IMG_2EAFF5128938-1.jpeg](/img/user/images/IMG_2EAFF5128938-1.jpeg)
	- 플래그들은 **플래그 레지스터**라는 레지스터에 저장됨

## ☑️ 제어장치

- 제어 신호를 내보내고 (일종의 전기 신호)
- 명령어를 해석하는 부품

#### 제어장치가 받아들이는 정보

- 클럭 신호
	- 컴퓨터의 모든 부품을 움직일 수 있게 하는 <u>시간 단위</u>
- 현재 수행할 명령어
	- 명령어 레지스터로부터 해석할 명령어를 받아들이고 해석한 뒤, 제어신호를 발생시켜 컴퓨터 부품들에 수행할 내용을 알려줌
- 플래그
	- 플래그 레지스터 속 플래그 값을 받아들이고 참고해서 제어신호 발생시킴
- 제어 신호
	- 제어 신호는 CPU뿐만 아니라 입출력장치를 비롯한 CPU외부 장치도 발생시킬 수 있다
	- <u>제어 버스</u>를 통해 외부로부터 전달된 제어 신호를 받아들인다.

#### 제어장치가 내보내는 정보

- **CPU 외부에 전달** (= 제어 버스로 제어 신호를 내보낸다)
	- 크게 메모리에 전달하는 제어신호, 입출력장치에 전달하는 제어신호가 있음
	- 메모리에 저장된 값을 읽거나 메모리에 새로운 값을 쓰고 싶다면 메모리로 제어신호를 내보내고
	- 입출력장치의 값을 읽거나 새로운 값을 쓰고 싶을 때 입출력장치로 제어 신호를 내보냄
	
- **CPU 내부에 전달** 
	- ALU에 수행할 연산 지시
	- 레지스터간에 데이터를 이동시키거나 레지스터에 저장된 명령어를 해석하기 위해 레지스터에 제어신호를 내보냄

## ☑️ 레지스터

#### 메모리에 저장된 프로그램을 실행하는 과정에서 사용되는 레지스터 8개

- **프로그램 카운터**
	- 메모리에서 가져올 명령어의 주소를 저장
- **메모리 주소 레지스터**
	- 주소 버스로 내보내기 위해 위의 주소가 여기 저장됨
	- 메모리 읽기 제어신호와 메모리 주소 레지스터 값이 각 각 제어 버스와 주소 버스를 통해 메모리로 보내짐
- 메모리 버퍼 레지스터
	- 메모리에 저장된 값은 데이터 버스를 통해서 메모리 버퍼 레지스터로 전달됨
	- 프로그램 카운터 증가함
- **명령어 레지스터**
	- 메모리 버퍼 레지스터에 저장된 값
