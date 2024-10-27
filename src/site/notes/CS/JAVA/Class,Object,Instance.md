---
{"dg-publish":true,"permalink":"/cs/java/class-object-instance/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-28T02:28:34.433+09:00","updated":"2024-10-28T03:32:10.598+09:00"}
---


```java
public class Animal { //클래스

}

public class main {
	public static void main(String[] args){
		Animal cat; // 객체
		cat = new Animal(); // 인스턴스
	}
}
```

- 클래스(Class) : 객체를 만들어내기 위한 설계도 혹은 틀
- 객체(Object) : 클래스 기반으로 선언된 대상, 클래스의 인스턴스라고도 불림
- 인스턴스(Instance) : 객체에 <u>메모리가 할당되어 실제로 활용</u>되는 실체



- 객체 선언: `스택` 영역에 참조 변수가 생성됨.
- 객체의 실제 할당: `힙` 영역에 객체가 생성되고, 스택의 참조 변수가 힙 객체의 주소를 참조함.

---
출처 : https://31daylee.tistory.com/m/entry/CS%EA%B8%B0%EC%88%A0-%EB%A9%B4%EC%A0%91-%EC%A7%88%EB%AC%B8-%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EC%9E%90%EB%B0%94