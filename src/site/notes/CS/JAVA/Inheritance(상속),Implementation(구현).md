---
{"dg-publish":true,"permalink":"/cs/java/inheritance-implementation/","dgPassFrontmatter":true,"noteIcon":"","created":"2024-10-28T03:09:59.218+09:00","updated":"2024-10-28T04:30:08.210+09:00"}
---

## 상속 (Inheritance)

- 상위 클래스의 속성과 메서드를 자식 클래스가 물려받아 ==재사용하고 확장==하는 것
- `extends` 키워드를 사용하여 클래스를 하위 클래스로 만듦
- 상속을 통해 부모 클래스의 기능을 ==자식 클래스에서 오버라이딩하거나 확장==할 수 있음
- ==단일 상속==만 가능하기 때문에 한 클래스가 여러 클래스를 상속받을 수 없음

```java
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {  // Dog가 Animal을 상속
    void bark() {
        System.out.println("Dog is barking");
    }
}
```

### 상속 시 접근 제어자에 따른 접근 범위


![스크린샷 2024-10-28 오전 3.27.35.png](/img/user/images/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-10-28%20%EC%98%A4%EC%A0%84%203.27.35.png)
[사진 출처](https://joy-baek.tistory.com/10) 

- `public`: 모든 클래스에서 접근 가능, 자식 클래스에서도 무제한 접근 가능
- `protected`: 같은 패키지에서 모든 클래스가 접근할 수 있으며, 다른 패키지에서는 오직 상속받은 자식 클래스만 접근 가능
- `default`: 같은 패키지에서만 접근 가능, 상속 관계와 관계없이 다른 패키지에서는 접근 불가
- `private`: 해당 클래스 내에서만 접근 가능, 상속받는 자식 클래스에서도 직접 접근 불가


## 구현 (Implementation)

- 클래스가 특정 인터페이스의 메서드를 구현하여 ==일관된 동작을 보장==하는 것
- `implements` 키워드를 사용하여 클래스를 인터페이스의 구현체로 만듦
- 인터페이스를 구현하여 여러 클래스들이 ==공통의 메서드 서명을 가지고 각각 다르게 동작==할 수 있게 함
- ==다중 구현이 가능==하므로, 한 클래스가 여러 인터페이스를 구현할 수 있음

```java
interface Movable {
    void move(); // 인터페이스에서 정의한 메서드 서명
}

class Car implements Movable {
    public void move() {
        System.out.println("Car is moving on the road");
    }
}

class Airplane implements Movable {
    public void move() {
        System.out.println("Airplane is flying in the sky");
    }
}
```
- `move()`라는 동일한 메서드 서명을 가짐
- 하지만 각각의 `move()` 메서드는 다르게 동작함


## 차이점 

| 구분    | 상속 (Inheritance)              | 구현 (Implementation)               |
| ----- | ----------------------------- | --------------------------------- |
| 대상    | 클래스 간의 관계 설정                  | 인터페이스를 통해 일관된 동작 구현               |
| 키워드   | `extends`                     | `implements`                      |
| 다중 사용 | 단일 상속만 가능                     | 다중 구현 가능                          |
| 목적    | ==상위 클래스의 기능을 재사용==하거나 ==확장== | ==일관된 메서드 서명 제공==으로 ==다양한 구현== 가능 |
