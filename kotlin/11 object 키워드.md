# object 키워드

## static 함수와 변수

자바의 static 함수와 변수를 표현한 코드를 코틀린 코드로 변경해보자
```java
public class Person {

    private static final int MIN_AGE = 1;

    public static Person newBaby(String name) {
        return new Person(name, MIN_AGE);
    }
    
    private String name;

    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```kotlin
class Person private constructor(
    var name: String,
    var age: Int
) {
    companion object {
        private const val MIN_AGE = 1

        fun newBaby(name: String): Person {
            return Person(name, MIN_AGE)
        }
    }
}
```

코틀린에서는 `static` 키워드가 사라졌다. 대신 이를 표현하기 위해 `companion object`를 사용한다.
코드만 봤을 때는 자바의 `static`을 대체하는 것으로 보여질 수 있지만 **`companion object`는 객체라는 점에서 둘은 분명한 차이가 있다.**

그리고 코틀린에서 `const`라는 키워드가 추가된 것을 볼 수 있다. 이는 상수를 선언하기 위한 키워드로 `val MIN_AGE`와 `const val MIN_AGE`의 차이는
`val`은 **런타임 시점**에 값이 할당되고 `const val`은 **컴파일 시점**에 값이 할당된다.

`const val`은 자바의 경우 `static final`과 같다고 볼 수 있다. `const`는 기본 타입과 String에 붙일 수 있다.

### companion object
`companion object`는 위에서 언급하였든 하나의 객체로 간주된다. 때문에 이름을 붙일 수도 있고, interface를 구현할 수도 있다.

```kotlin
class Person private constructor(
    var name: String,
    var age: Int
) {
    // 이름 사용
    companion object Factory : Log {
        private const val MIN_AGE = 1

        fun newBaby(name: String): Person {
            return Person(name, MIN_AGE)
        }
        
        // 인터페이스 구현
        override fun log() {
            println("Log override")
        }
    }
}
```

## @JvmStatic
자바에서 코틀린에 있는 static field나 static 함수를 활용하는 코드를 구현해보자. 
위에서 구현한 코틀린 클래스를 자바에서 활용하는 예이다.

```java
public static void main(String[]args) {
    Person.Companion.newBaby("baby"); // companion object 이름이 생략된 경우
    Person.Factory.newBaby("baby"); // companion object 이름이 있는 경우
}
```

그리고 아래는 @JvmSatic을 사용할 경우이다.

```kotlin
// kotlin
class Person private constructor(
    var name: String,
    var age: Int
) {
    companion object Factory {
        private const val MIN_AGE = 1
        
        @JvmStatic
        fun newBaby(name: String): Person {
            return Person(name, MIN_AGE)
        }
    }
}

//java
public static void main(String[]args) {
    Person.newBaby("baby");
}
```

## 싱글톤

자바코드로 싱글톤을 구현해보자
```java
public class Singleton {
    private final Singleton instance = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

이를 코틀린 코드로 구현하면 아래와 같다.

```kotlin
object Singleton {
}
```
코틀린은 `객체 선언(object)` 기능을 통해 싱글톤을 언어 자체에서 기본 지원한다. 
코틀린에서 싱글톤 객체 사용 시에는 객체명으로 사용하면 되고, 자바에서 사용 시에는 자동으로 생성되는 `INSTANCE`필드를 호출하여 사용할 수 있다.

## 익명 클래스
익명 클래스는 특정 인터페이스나 클래스를 상속받은 구현체를 일회성으로 사용할 때 활용하는 클래스이다. 이를 자바 코드로 표현해 보자

```java
public static void main(String[]args) {
    moveSomething(new Movable() {
        @Override
        public void move() {
            System.out.println("move");
        }
        
        @Override
        public void fly() {
        System.out.println("fly");
        }
    });
}
```

코틀린은 `object` 키워드를 사용하여 익명 클래스를 표현할 수 있다.

```kotlin
fun main() {
    moveSomething(object : Movable {
        override fun move() {
            println("move")
        }
        
        override fun fly() {
            println("fly")
        }
    })
}
```

## 정리

 - 자바의 `static` 변수와 함수를 만드려면 코틀린에서는 `companion object`를 사용해야 한다.
 - `companion object`는 객체로 간주되기 때문에 이름을 가질 수 있고, 다른 타입을 상속 받을 수 있다.
 - 코틀린에서 싱글톤 클래스를 만들 때는 `object` 키워드를 사용한다.
 - 코틀린에서 익병 클래스를 만들 때는 `object : 타입` 형식으로 사용할 수 있다.
