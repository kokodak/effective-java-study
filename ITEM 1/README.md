# 아이템 1, 생성자 대신 정적 팩터리 메서드를 고려하라

## 클래스의 인스턴스를 얻는 두 가지 방법

- public 생성자
- **정적 팩터리 메서드**

### 정적 팩터리 메서드 예시

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

## 정적 팩터리 메서드의 장점

### 첫 번째, 이름을 가질 수 있다.

#### 역할을 쉽게 묘사할 수 있다.

```java
// 생성자
BigInteger(int, int, Random)

// 정적 팩터리 메서드
BigInteger.probablePrime()
```

위의 두 코드는 모두 같은 일을 수행합니다. 바로 **값이 소수인 BigInteger를 반환** 하는 일이죠.   
생성자는 생성자 자체만으로 어떤 역할을 하고 있는지 알 수 없지만, 정적 팩터리 메서드는 그렇지 않습니다. 이름만 잘 짓는다면 어떤 일을 하는지 쉽게 묘사할 수 있죠.

#### 파라미터 타입으로 인한 제약이 없다.

생성자는 이름을 변경할 수 없습니다. 이 때문에 생성자는 메서드 시그니처로부터 자유롭지 못합니다. 예를 들면 이러한 경우겠죠.

```java
public class Animal {

    private String gender;
    private String name;
    private int age;

    public Animal(String gender, int age) {
        ...
    }
    
    // Animal(String, int) 시그니처 중복으로 인한 컴파일 에러
    public Animal(String name, int age) {
        ...
    }
}
```

그렇지만, 정적 팩터리 메서드는 생성자와는 다르게 다양한 이름을 가질 수 있습니다. 따라서, 같은 파라미터 타입이더라도 시그니처는 겹치지 않습니다.

```java
public class Animal {

    private String gender;
    private String name;
    private int age;

    public Animal(String gender, String name, int age) {
        ...
    }

    public static Animal getAnimalWithoutGender(String name, int age) {
        ...
    }

    public static Animal getAnimalWithoutName(String gender, int age) {
        ...
    }
}
```

### 두 번째, 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

