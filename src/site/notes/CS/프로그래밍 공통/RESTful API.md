---
{"dg-publish":true,"permalink":"/cs//res-tful-api/","dgPassFrontmatter":true,"noteIcon":""}
---

# 📌 REST 란?

---

- REST(Representational State Transfer)의 약자로 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

1. HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고,
2. HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해해당 자원(URI)에 대한 **CRUD Operation**을 적용하는 것을 의미합니다.

<aside> 🔎 CRUD Operation이란?

CRUD는 대부분의 컴퓨터 소프트웨어가 가지는 기본적인 데이터 처리 기능인 Create(생성), Read(읽기), Update(갱신), Delete(삭제)를 묶어서 일컫는 말로

REST에서의 CRUD Operation 동작 예시는 다음과 같다.

---

Create : 데이터 생성(POST)

Read : 데이터 조회(GET)

Update : 데이터 수정(PUT, PATCH)

Delete : 데이터 삭제(DELETE)

</aside>

### ✔️ **REST 구성 요소**

1. **자원(Resource) : HTTP URI**
2. **자원에 대한 행위(Verb) : HTTP Method**
3. **자원에 대한 행위의 내용 (Representations) : HTTP Message Pay Load**

# 📌 API란?

---

- 다른 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의합니다.
- 웹 API는 클라이언트와 웹 리소스 사이의 게이트웨이라고 생각할 수 있습니다.

<aside> 🔎 **리소스란?**

리소스는 다양한 애플리케이션이 **클라이언트에게 제공하는 정보**입니다. 리소스는 이미지, 동영상, 텍스트, 숫자 또는 모든 유형의 데이터일 수 있습니다. 클라이언트에 **리소스를 제공하는 시스템을 서버**라고도 합니다.

</aside>

# 📌 REST API란? (RESTful API)

---

- **RESPT API란 REST의 원리를 따르는 API를 의미합니다.**

### ✔️ **REST API 설계 예시**

<aside> 🔎 **1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.**

❌**Bad Example [](http://khj93.com/test/)[http://khj93.com/Running/](http://khj93.com/Running/)**

⭕**Good Example  [](http://khj93.com/test/)[http://khj93.com/run/](http://khj93.com/run/)**

</aside>

<aside> 🔎 **2. 마지막에 슬래시 (/)를 포함하지 않는다.**

❌**Bad Example [http://khj93.com/test/**](http://khj93.com/test/**)

⭕**Good Example  [](http://khj93.com/test/)[http://khj93.com/test](http://khj93.com/test)**

</aside>

<aside> 🔎 **3. 언더바 대신 하이폰을 사용한다.**

❌**Bad Example [](http://khj93.com/test/)[http://khj93.com/test_blog](http://khj93.com/test_blog)**

⭕**Good Example  [](http://khj93.com/test/)[http://khj93.com/test-blog](http://khj93.com/test-blog)**

</aside>

<aside> 🔎 **4. 파일확장자는 URI에 포함하지 않는다.**

❌**Bad Example [](http://khj93.com/test/)[http://khj93.com/photo.jpg](http://khj93.com/photo.jpg)**

⭕**Good Example  [](http://khj93.com/test/)[http://khj93.com/photo](http://khj93.com/photo)**

</aside>

<aside> 🔎 **5. 행위를 포함하지 않는다.**

❌**Bad Example [](http://khj93.com/test/)[http://khj93.com/delete-post/1](http://khj93.com/delete-post/1)**

⭕**Good Example  [](http://khj93.com/test/)[http://khj93.com/post/1](http://khj93.com/post/1)**

</aside>

<aside> 🔎 **6. 전달하고자 하는 명사를 사용하되, 컨트롤 자원을 의미하는 경우 예외적으로 동사를 사용한다.**

❌**Bad Example** [http://dev-cool.tistory.com/course/writing](http://dev-cool.tistory.com/course/writing)

⭕**Good Example** [http://dev-cool.tistory.com/course/write](http://dev-cool.tistory.com/course/write)

</aside>

<aside> 🔎 **7. URI에 작성되는 영어를 복수형으로 작성한다.**

⭕**Good Example** [http://api.college.com/students/3248234/courses](http://api.college.com/students/3248234/courses)

</aside>

# 📌 **HTTP Method**

---

- _**GET**_
    - 데이터 조회 시 사용
- _**POST**_
    - 새로운 정보를 추가하는데 사용
- _**PUT**_
    - 정보를 통째로 변경할 때 사용
- _**PATCH**_
    - 정보 중 일부를 특정 방식으로 변경할 때 사용
- _**DELETE**_
    - 데이터 삭제 시 사용

# 📌 **RESTful API 서버 응답** 코드

---

- 1XX : 요청을 성공적으로 받았으며 서버가 해당 작업을 진행 중
    
- 2XX : 요청을 성공적으로 받았으며 요청이 이루어짐
    
    - 200 : 요청이 성공적으로 처리됨. 가장 흔히 사용
    - 204 : 요청이 성공적으로 처리되었지만, 답장에 적어 보낼 내용은 없음
    - 206 : 요청에서 지정한 대로, 일부 콘텐츠만 보냄
- 4XX : 클라이언트 요청에 문제가 있기 때문에 수행할 수 없음
    
    - 401 : Unauthorized - 로그인이 필요한 요청인데 로그인 되어 있지 않음
    - 403 : Forbidden - 로그인 되어 있지만 요청을 보낼 권한이 없음
    - 404 : Not Found - 요청에 해당하는 데이터가 없음 또는 URL이 잘못되었을 때 등에 나타남
- 5XX : 요청에는 문제가 없지만, 서버에 이상이 있어 응답할 수 없음
    
    - 500 : 서버 내부에 오류 발생
    - 502 : 서버 과부하 또는 기타 네트워크 문제로 통신이 제대로 되지 않음

🧐 400대의 오류는 사용자 측, 500 번 대의 오류는 서버 측의 문제