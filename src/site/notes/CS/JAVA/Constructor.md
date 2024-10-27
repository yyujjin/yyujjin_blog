---
{"dg-publish":true,"permalink":"/cs/java/constructor/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-28T02:41:14.592+09:00","updated":"2024-10-28T04:30:14.825+09:00"}
---

## 생성자란(Constructor)?
**생성자**는 클래스의 인스턴스(객체)를 ==초기화==하는 특별한 메서드

## 생성자의 특징
1. 생성자는 반드시 클래스 이름과 같아야 함
2. 반환 타입이 없고, 반환 타입을 명시하지도 않아도 됨. (`void`도 사용하지 않음)
3. 객체가 생성될 때 `new` 키워드와 함께 자동 호출됨
4. 같은 이름의 생성자를 매개변수만 다르게 여러 개 정의할 수 있음(생성자 오버로딩)
5. 생성자를 명시적으로 정의하지 않으면, ==컴파일러가 자동으로 매개변수가 없는 기본 생성자를 추가함==
6. <u>매개변수가 있는 생성자를 정의하면 컴파일러는 기본 생성자를 추가하지 않으므로</u>,  직접 정의해야 함


<font color="#7f7f7f">생성자 오버로딩 코드 예시</font>
```java
public class Person {
    String name;
    int age;
    
    // 기본 생성자
    public Person() {
        this.name = "Unknown";
        this.age = 0;
    }
    
    // 이름만 초기화하는 생성자
    public Person(String name) {
        this.name = name;
        this.age = 0;
    }
    
    // 이름과 나이를 초기화하는 생성자
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

```


## 디폴트 호출
디폴트 호출이란, ==자식 클래스의 생성자에서 부모 클래스의 기본 생성자(`super()`)가 자동으로 호출되는 것을 의미함==

<font color="#7f7f7f">부모 클래스에 기본생성자만 있을 경우</font>
```java
class Animal {
    String name;

    // 기본 생성자
    public Animal() {
        System.out.println("Animal 기본 생성자 호출");
    }
}

class Dog extends Animal {
    int age;

    // 자식 클래스 생성자 (super() 호출 생략)
    public Dog(int age) {
        this.age = age;
        System.out.println("Dog 생성자 호출");
    }
}

```

```java
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog(3);
        // 출력:
        // Animal 기본 생성자 호출
        // Dog 생성자 호출
    }
}
```


<font color="#7f7f7f">부모 클래스에 매개변수가 있는 생성자가 정의됐을 경우</font>
```java
class Animal {
    String name;

    // 매개변수가 있는 생성자만 정의 (기본 생성자 없음)
    public Animal(String name) {
        this.name = name;
    }
}

class Dog extends Animal {
    int age;

    // 자식 클래스 생성자
    public Dog(String name, int age) {
        super(name); // 명시적으로 부모 생성자를 호출해야 함
        this.age = age;
    }
}

```

- 자식 클래스의 생성자에서는 반드시 부모 클래스의 생성자가 호출됨
- 다만, 명시적으로 호출하지 않으면 컴파일러가 자동으로 기본 생성자(`super()`)를 추가하여 부모 클래스의 생성자를 호출함
- 만약 부모 클래스에 기본 생성자(매개변수가 없는 생성자)가 없고, 매개변수가 있는 생성자만 있을 경우, 자식 클래스 생성자에서 반드시 `super(매개변수)` 형태로 부모 생성자를 명시적으로 호출해야함
- 그렇지 않으면 컴파일 오류가 발생함