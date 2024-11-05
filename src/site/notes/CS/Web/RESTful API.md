---
{"dg-publish":true,"permalink":"/cs/Web/res-tful-api/","dgPassFrontmatter":true,"noteIcon":""}
---

## REST(Representational State Transfer)란?

- 자원의 상태를 주고받는 모든 것을 의미함

### REST 구성 요소

1. 자원(Resource) : HTTP URI
2. 자원에 대한 행위(Verb) : HTTP Method
3. 자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load

####  예시: 블로그 포스트 관리 시스템

1. **자원(Resource)**: `HTTP URI`
    
    - 자원은 블로그 시스템에서 관리되는 항목, 예를 들어 **블로그 글**을 나타냅니다.
    - **예시**: `http://example.com/posts`
2. **자원에 대한 행위(Verb)**: `HTTP Method`
    
    - 자원에 대해 수행할 수 있는 동작입니다. HTTP Method로 표현됩니다.
    - **예시**:
        - `GET /posts` : 모든 블로그 글을 조회
        - `POST /posts` : 새로운 블로그 글을 생성
        - `PUT /posts/1` : ID가 1인 블로그 글을 수정
        - `DELETE /posts/1` : ID가 1인 블로그 글을 삭제
3. **자원에 대한 행위의 내용(Representations)**: `HTTP Message Pay Load`
    
    - 행위의 결과로 전송되는 데이터입니다. 주로 JSON이나 XML 형식으로 자원을 표현합니다.
    - **예시**:`POST`로 새로운 블로그 글을 생성할 때의 요청 본문
    ```json
    {
	     "title": "REST 구성 요소에 대한 설명",
	      "content": "REST는 자원, 행위, 행위의 내용을 기반으로 설계됩니다."
	}
	```

### HTTP Method


- `GET`
    - 데이터 조회 시 사용
- `POST`
    - 새로운 정보를 추가하는데 사용
- `PUT`
    - 정보를 통째로 변경할 때 사용
- `PATCH`
    - 정보 중 일부를 특정 방식으로 변경할 때 사용
- `DELETE`
    - 데이터 삭제 시 사용


## API(Application Programming Interface)란?


- 다른 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙
	- 소프트웨어란?
		- 컴퓨터나 다른 기기에서 특정 작업을 수행하거나 기능을 실행하기 위해 작성된 ==프로그램이나 명령어==들을 의미
- 웹 API는 클라이언트와 웹 리소스 사이의 게이트웨이라고 생각할 수 있음

## REST API란? (RESTful API)


- RESPT API란 ==REST의 원리를 따르는 API==를 의미한다.



