# Override vs Overload

## 한 줄 요약

- Overload: 같은 이름의 메서드를 여러 개 만들되, 파라미터를 다르게 정의하는 것.
- Override: 부모 클래스의 메서드를 자식 클래스에서 다시 정의하는 것.

## 먼저 구분하기

두 개념은 모두 "메서드 이름"과 관련이 있지만 기준이 다르다.

- Overload는 입력값의 차이를 다룬다.
- Override는 상속 관계에서 동작의 재정의를 다룬다.

즉, 오버로딩은 "같은 기능을 여러 입력 방식으로 제공하는 것"이고, 오버라이딩은 "부모가 가진 기능의 실제 동작을 자식에 맞게 바꾸는 것"이다.

또한 오버로딩은 컴파일 시점에 메서드 시그니처로 결정되고, 오버라이딩은 실행 시점의 실제 객체 타입으로 결정된다.

---

## Overload

### 설명

오버로딩은 같은 이름의 메서드를 여러 개 선언하는 것이다. 단, 메서드 이름만 같고 파라미터 목록은 달라야 한다.

파라미터가 달라지는 방식은 다음과 같다.

- 개수
- 타입
- 순서

반환 타입만 다른 것은 오버로딩이 아니다.

### 느낌 먼저 이해하기

리모컨에는 "볼륨 올리기"라는 기능이 하나 있지만, 사용할 수 있는 방식은 여러 가지일 수 있다.

- 한 칸 올리기
- 원하는 숫자만큼 올리기
- 음성 명령으로 올리기

기능의 이름은 같지만 입력 방식이 다르다. 이 느낌이 오버로딩이다.

### 코드 예시

```java
class RemoteController {

    void volumeUp() {
        System.out.println("볼륨 1 증가");
    }

    void volumeUp(int amount) {
        System.out.println("볼륨 " + amount + " 증가");
    }

    void volumeUp(String voiceCommand) {
        System.out.println("음성 명령으로 볼륨 증가: " + voiceCommand);
    }
}
```

```java
class Main {

    public static void main(String[] args) {
        RemoteController remote = new RemoteController();

        remote.volumeUp();
        remote.volumeUp(10);
        remote.volumeUp("볼륨 올려줘");
    }
}
```

### 언제 사용하는가

같은 의미의 동작을 여러 입력 형태로 제공하고 싶을 때 사용한다.

예를 들어 생성자 오버로딩은 객체를 다양한 방식으로 만들 수 있게 해준다.

```java
class User {
    User() {
    }

    User(String name) {
    }

    User(String name, int age) {
    }
}
```

---

## Override

### 설명

오버라이딩은 부모 클래스가 가진 메서드를 자식 클래스에서 다시 정의하는 것이다.

오버라이딩하려면 보통 다음 조건을 만족해야 한다.

- 상속 관계가 있어야 한다.
- 메서드 이름이 같아야 한다.
- 파라미터 목록이 같아야 한다.
- 반환 타입은 같거나 공변 반환 타입이어야 한다.
- 접근 제어자는 부모 메서드보다 좁아질 수 없다.

### 느낌 먼저 이해하기

부모 클래스가 "소리를 낸다"라는 기능을 제공한다고 해보자. 모든 동물이 소리를 낸다는 점은 같지만, 실제 소리는 동물마다 다르다.

이처럼 기본 기능은 같지만 자식 클래스에 맞게 실제 동작을 바꾸는 것이 오버라이딩이다.

### 코드 예시

```java
class Animal {

    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("멍멍");
    }
}
```

```java
class Main {

    public static void main(String[] args) {
        Animal animal = new Dog();
        animal.sound();
    }
}
```

출력:

```txt
멍멍
```

### `@Override`를 붙이는 이유

`@Override`는 이 메서드가 부모 메서드를 재정의한다는 의도를 컴파일러에게 알려준다.

장점은 다음과 같다.

- 메서드 이름 오타를 컴파일 단계에서 잡을 수 있다.
- 파라미터가 달라져서 실수로 오버로딩되는 상황을 막을 수 있다.
- 코드를 읽는 사람이 재정의 의도를 바로 알 수 있다.

---

## 비교표

| 구분 | Overload | Override |
| --- | --- | --- |
| 핵심 기준 | 파라미터 차이 | 상속 관계와 동작 재정의 |
| 메서드 이름 | 같음 | 같음 |
| 파라미터 | 달라야 함 | 같아야 함 |
| 상속 필요 여부 | 필요 없음 | 필요함 |
| 목적 | 같은 기능을 다양한 입력으로 제공 | 부모 기능을 자식에 맞게 변경 |
| 결정 시점 | 컴파일 시점 | 런타임 다형성과 연결됨 |

## 자주 하는 실수

- 반환 타입만 다르게 만들고 오버로딩이라고 생각하는 것.
- 부모 메서드와 파라미터가 달라졌는데 오버라이딩이라고 생각하는 것.
- `@Override`를 생략해서 오타나 시그니처 차이를 늦게 발견하는 것.

## 요약

- Overload는 같은 이름, 다른 파라미터다.
- Override는 같은 이름, 같은 파라미터, 다른 동작이다.
- Overload는 입력 방식을 늘리는 것이고, Override는 상속받은 동작을 바꾸는 것이다.
