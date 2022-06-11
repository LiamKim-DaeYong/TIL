# JVM(Java Virtual Machine)
- **자바 바이트 코드를 실행할 수 있는 주체**
- 자바가 운영체제 종류, 플랫폼에 관계없이 독립적으로 실행 가능하도록 해준다.
- GC(Garbage Collection)를 통해 메모리 관리를 자동으로 해준다.


# 자바의 실행 과정
1. 자바 컴파일러를 통해 자바 소스 코드(.java)를 자바 바이트 코드(.class)로 변환
2. 클래스 로더를 통해 자바 바이트 코드를 JVM 런타임 데이터 영역에 로드
3. 실행 엔진을 통해 실행


# JVM 구성요소
![helloworld-1230-1](https://user-images.githubusercontent.com/55070039/173191207-5c66f8d9-49ed-4575-ad74-473e23dc7b78.png)

- Java Compiler: Java source(.java)를 Byte code로 변환하여 .class 파일을 만든다. 
- Class Loader: 컴파일 된 Byte code(.class) 파일을 JVM 메모리에 Load하고 배치 시킨다.
- Execution Engine: 배치된 Byte code를 명령어 단위로 실행한다.
- Garbage Collector: 어플리케이션이 생성한 객체(인스턴스)의 생존 여부를 판단하여 더 이상 참조되지 않거나 null인 객체의 메모리를 해제시킨다.
- Runtime Data Area: JVM이 실행되면서 할당받은 메모리 공간이다.

# Runtime Data Area

![다운로드](https://user-images.githubusercontent.com/55070039/173191410-f71823f0-187e-4923-8233-0398d5108fac.png)

- Method Area: 클래스, 인터페이스에 대한 런타임 상수풀, 메서드, 필드, 생성자, 타입 정보, static 변수, static 메서드, 바이트 코드등을 보관한다.
전체 thread에서 공유할 수 있고, 시작시에 생성되서 종료될 때 까지 존재한다. 따라서 전역변수를 너무 많이 쓰게되면 메모리 초과 위험이 있다.


- Runtime constant pool: 각 class, interface는 각각의 런타임 상수풀을 가진다. 상수들은 메서드와 필드, 데이터들의 래퍼런스 값을 가지고 있다.
중복되는 정보가 필요하게 되면 런타임 상수풀에서 꺼내서 사용한다. (Symbol table)


- Head Area: 레퍼런스 타입을 갖는 객체(인스턴스), 배열 등이 동적으로 생성되서 저장되는 곳이다.
new 연산자를 통해 heap 영역에 객체를 생성하여 메모리를 부여받고 stack 영역에 참조변수에 실제 객체의 레퍼런스 값을 리턴받는다. 이 레퍼런스 값을 통해서만 객체를 다룰 수 있다.


- Stack Area: 각 스레드 시작시에 하나씩 할당되고 종료될 때 해제 된다. 메소드 호출시에 프레임을 추가하고(push) 종료되면 제거한다.(pop)
지역변수, 매개변수, 메서드 정보들의 임시 데이터가 저장된다.
primitive 타입 변수는 스택 영역에 직접 값을 가진다. reference 타입의 변수는 힙 영역이나 메서드 영역의 객체 주소를 가진다. 

- PC Register: 현재 실행중인 JVM 주소를 저장한다. (프로그램 실행은 CPU에서 인스트럭션(Instruction)을 수행)


- Native Method Stack Area: 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 stack 이다.
Java Native Interface를 통해 호출되는 C/C++등의 코드를 수행하기 위한 스택이다.
