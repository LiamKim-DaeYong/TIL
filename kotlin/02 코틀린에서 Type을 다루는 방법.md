# 코틀린에서 Type을 다루는 방법

## 기본 타입

| Java Type | Kotlin Type | Kotlin Type 용량(bit) |
|:---------:|:-----------:|:-------------------:|
|  double   |   Double    |         64          |
 |   float   |    Float    |         32          |
|   long    |    Long     |         64          |
|    int    |     Int     |         32          |
|   short   |    Short    |         16          |
|   byte    |    Byte     |          8          |

```kotlin
val number1 = 3 // Int
val number2 = 3L // Long

val number3 = 3.0f // Float
val number4 = 3.0 // Double
```

### Java와 Kotlin의 기본타입 차이점
- Java : 기본타입 간의 변환은 **암시적**으로 이루어질 수 있다
- Kotlin : 기본타입 간의 변환은 **명시적**으로 이루어져야 한다

```java
int number1 = 4;
long number2 = number1;
```
Java의 경우 int 타입의 값이 long 타입으로 암시적으로 변경(long이 int보다 더 큰 타입이므로)

```kotlin
val number1: Int = 4
val number2: Long = number1.toLong()
```
Kotlin의 경우 타입 변환 시 `to변환타입()`를 사용하여야 한다. (예: `toDouble()`, `toByte()`, `toString()`...)

### nullable 변수 타입변환
```kotlin
val number1: Int? = 4
val number2: Long = number1?.toLong() ?: 0L
```
nullable 변수의 타입변환 시에는 위와 같이 `null safe operator`과 `Elvis operator`를 활용한다 

## 타입 캐스팅
```java
// Java
public static void convertObj(Object obj) {
    if (obj instanceof Person) {
        Person person = (Person) obj;
        System.out.println(person.getAge());
    }
}
```

```kotlin
// Kotlin
fun convertObj(obj: Any) {
    if (obj is Person) {
        val person = obj as Person
        println(person.age)
    }
}
```
두 코드의 차이점을 보면 Java의 `instanceof`가 Kotlin에서는 `is`로`(Person) obj`의 경우는 `obj as Person`으로 표현하는 것을 알 수 있다.

### 스마트 캐스트
위의 예제에서 obj를 Person 타입으로 변경하기 위해 `as Person` 구문을 사용하였다. 
Kotlin은 스마트 캐스트라는 기능을 지원하여 해당 구문을 생략할 수도 있다. 
```kotlin
fun convertObj(obj: Any) {
    if (obj is Person) {
        println(obj.age)
    }
}
```
쉽게 설명하여 `is`를 사용하여 타입을 체크하면 이후 컴파일러가 자동으로 형변환(캐스팅)을 해준다. 이를 스마트 캐스트라 한다.

## Kotlin의 3가지 특이한 타입

### Any
- Java의 Object 역할 (모든 객체의 최상위 타입)
- 모든 Primitive Type의 최상위 타입도 Any
- Any 자체로는 null을 포함할 수 없어 null을 포함하고 싶다면 Any?로 표현
- Any에는 `equals`, `hashCode`, `toString` 메소드가 존재 

### Unit
- Java의 void와 동일한 역할
- void와 다르게 Unit은 그 자체로 타입 인자로 사용 가능
- 함수형 프로그래밍에서 Unit은 단 하나의 인스턴스만 갖는 타입을 의미.
즉, 코틀린의 Unit은 실제 존재하는 타입이라는 것을 표현

### Noting
- 함수가 정상적으로 끝나지 않았다는 사실을 표현하는 역할
- 무조건 예외를 반환하는 함수, 무한 루프 함수 등

## 문자열 다루기 

### String Interpolation
```java
//java
Person person = new Person("김xx", 31);
String str = String.format("이름은 %s이고 나이는 %d 입니다.", person.getName(), person.getAge());
```

```kotlin
//kotlin
val person = Person("김xx", 31)
val str = "이름은 ${person.name}이고 나이는 ${person.age} 입니다."
```

### 여러 줄의 문자열 표현
kotlin은 """(3개)를 사용하여 여러줄의 문자열을 표현할 수 있다.
```kotlin
val str = 
"""
    코틀린은
    여러 줄의 문자열을
    표현할 수 있다.
""".trimIndent()
```

### String indexing
```java
//java
String str = "ABCDE";
char ch = str.charAt(1);
```

```kotlin
//kotlin
val str = "ABCDE"
val ch = str[1]
```
