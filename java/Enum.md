# Enum 이란
Enum이란 프로그램이 실행되는 동안 고정되어 있는 값(상수)을 정의하기 위해 사용하는 클래스이다. 

# 상수를 정의하는 다양한 방법
```java
public class example {
    private final static int MONDAY = 1;    
    private final static int TUESDAY = 2;
    private final static int WEDNESDAY = 3;    
    private final static int THURSDAY = 4;    
    private final static int FRIDAY = 5;    
    private final static int SATURDAY = 6;    
    private final static int SUNDAY = 7;    
}
```

```java
interface DAY {
    int MONDAY = 1;
    int TUESDAY = 2;
    int WEDNESDAY = 3;
    int THURSDAY = 4;
    int FRIDAY = 5;
    int SATURDAY = 6;
    int SUNDAY = 7;
}
```

# Enum을 사용한 상수 정의
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

위와 같이 enum을 사용하면 class나 interface를 사용하여 상수를 정의할 때보다 여러 장점을 가진다.

- 코드가 단순해지며 가독성이 높음
- 인스턴스 생성과 상속을 방지
- 키워드(enum)를 사용하기 때문에 열거임을 분명하게 나타낼 수 있음

# Enum과 생성자

## enum 클래스의 원소에 추가 속성 부여
enum은 생성자를 사용하여 추가 속성을 부여할 수 있다. 생성자의 파라미터를 통해 추가 속성을 설정받고, 
getter를 통해 해당 속성의 값을 확인할 수 있다.

```java
public enum DeleteYn {
    Y("삭제"),
    N("미삭제");
    
    private String desc;

    DeleteYn(String desc) {
        this.desc = desc;
    }
    
    public String getDesc() {
        return desc;
    }
}
```
