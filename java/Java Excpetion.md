# Exception
프로그래밍에서 예외(Exception)란 입력 값에 대한 처리가 불가능하거나, 프로그램 실행 중에 참조된 값이 잘못된 경우 등
정상적인 프로그램의 흐름이 어긋나는 것을 말한다.

# Error
에러는 시스템에 무엇인가 비정상적인 상황이 발생한 경우에 사용된다.
주로 자바 가상머신에서 발생시키는 것이며 예외와 반대로 이를 애플리케이션 코드에서 잡으려고 하면 안된다.
에러의 예로는 `OutOfMemoryError`, `ThreadDeath`, `StackOverflowError`등이 있다.

# 예외 구분 - Checked, Unchecked
Exception은 Checked Exception과 Unchecked Exception으로 구분할 수 있는데,
간단하게 RuntimeException을 상속하지 않는 클래스는 Checked Exception, 반대로 상속한 클래스는 Unchecked Exception으로 분류할 수 있다.

![2019-03-02-java-checked-unchecked-exceptions-1](https://user-images.githubusercontent.com/55070039/173815649-303a32ce-e485-46f8-b47c-bf22ace96e3b.png)

여기서 RuntimeException은 Exception 클래스의 서브 클래스이기 때문에 Exception의 일종이기도 하지만 자바에서는 RuntimeException과 이를 상속한 클래스를 조금 특별하게 취급한다.
명식적으로 예외 처리를 하지 않아도 되기 때문이다.

# 예외 처리
예외를 처리하는 방법에는 **예외 복구**, **예외 처리 회피**, **예외 전환** 방법이 있다.

## 예외 복구
- 예외 상황을 파악하고 문제를 해결하여 정상으로 돌려놓는 방법
- 예외를 잡아서 일정 시간, 조건만큼 대기 후 재시도를 반복한다.
- 최대 재시도 횟수를 넘기게 되는 경우 예외를 발생시킨다.

## 예외 처리 회피
- 예외 처리를 직접 담당하지 않고 호출한 쪽으로 던져 회피하는 방법

## 예외 전환
- 예외 회피와 비슷하게 메서드 밖으로 예외를 던지지만, 그냥 던지지 않고 적절한 예외로 전환하여 넘기는 방법
- 조금 더 명확한 의미로 전달하기 위해 적합한 의미를 가진 예외로 변경한다.
- 예외 처리를 단순하게 만들기 위해 wrapping 할 수도 있다.

