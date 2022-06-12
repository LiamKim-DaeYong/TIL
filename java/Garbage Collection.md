# Garbage Collection
Garbage Collection은 자바의 메모리 관리 방법 중의 하나로 JVM의 Heap 영역에서
동적으로 할당했던 메모리 영역 중 필요 없게 된 메모리(Garbage) 영역을 주기적으로 삭제하는 프로세스를 말한다.

# Minor GC와 Major GC
JVM의 Heap 영역은 처음 설계될 때 다음의 2가지를 전제로 설계되었다.
- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성이며, 메모리에 오랫동안 남아있는 경우는 드물다는 것이고
객체의 생존 기간에 따라 물리적으로 Heap 영역을 `Young`, `Old`로 나누어 설계되었다.
(초기에는 Perm 영역이 존재하였지만 Java8부터 제거되었다.)

![다운로드 (1)](https://user-images.githubusercontent.com/55070039/173237473-5dc0a0b5-c42e-48a6-babc-3ade6c9b3867.png)

## Young 영역 (Young Generation)
- 새롭게 생성된 객체가 할당(Allocation)되는 영역
- 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Young 영역에 대한 가비지 컬렉션(Garbage Collection)을 Minor GC 라고 부른다.

## Old 영역 (Old Generation)
- Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
- Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 적게 발생된다.
- Old 영역에 대한 가비지 컬렉션(Garbage Collection)을 Major GC 또는 Full GC 라고 부른다.

Old 영역이 Young 영역보다 크게 할당되는 이유는 Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않으며 큰 객체들은 Young 영역이 아니라
바로 Old 영역에 할당되기 때문이다.

예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 것이다. 이러한 경우를 대비하여 Old 영역에는
512 bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.

![다운로드 (2)](https://user-images.githubusercontent.com/55070039/173237657-7592073f-552d-4465-a28b-5c961773eb65.png)

# Garbage Collection 동작 방식
Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에, 세부적인 동작 방식은 다르다.
하지만 기본적으로 가비지 컬렉션이 실행된다고 하면 다음의 2가지 공통적인 단계를 따르게 된다.

1. Stop The World
2. Mark and Weep

## Stop The World
`Stop The World`는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다.
GC가 실행될 때는 GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다.

쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 `stop-the-world`의 시간을 줄이는 작업을 한다.
또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.

## Mark and Sweep
- Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

`Stop The World`를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다.
그리고 사용되고 있는 메모리를 식별하는데, 이러한 과정을 Mark라고 한다. 이후에 Mark가 되지 않은 객체들을 메모리에서 제거하는데, 이러한 과정을 Sweep이라 한다.

# Minor GC 동작 방식
Minor GC를 정확히 이해하기 위해서는 Young 영역의 구조에 대해 이해해야 한다. Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역, 총 3가지로 나뉘어진다.
- Eden 영역: 새로 생성된 객체가 할당(Allocation) 영역
- Survivor 영역: 최소 1번의 이상 GC에서 살아남은 객체가 존재하는 영역

객체가 새롭게 생성되면 Young 영역 중에서도 Eden 영역에 할당(Allocation)이 된다. 그리고 Eden 영역이 꽉 차면 Minor GC가 발생하게 되는데, 사용되지 않은 메모리는 해제되고
Eden 영역에 존재하는 객체는 (사용중인) Survivor 영역으로 옮겨지게 된다. Survivor 영역은 총 2개이지만 반드시 1개의 영역에만 데이터가 존재해야 한다.

1. 새로 생성된 객체가 Eden 영역에 할당된다.
2. 객체가 계속 생성되어 Eden 영역이 꽉차게 되고 Minor GC가 실행된다.
3. Eden 영역에서 사용되지 않는 객체의 메모리가 해제된다.
4. Eden 영역에서 살아남은 객체는 1개의 Survivor 영역으로 이동된다.

객체의 생존 횟수를 카운트하기 위해 Minor GC에서 살아남은 횟수를 의미하는 age를 Object Header에 기록한다. 그리고 Minor GC때 Object Header에 기록된 age를 보고 Promotion 여부를 결정한다.
또한 Survivor 영역 중 1개는 반드시 사용이 되어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아님을 파악할 수 있다.

![다운로드 (3)](https://user-images.githubusercontent.com/55070039/173238054-962e21ba-77c7-400d-9e4f-b4ffbb791281.png)

# Major GC 동작 방식
Young 영역에서 오래 살아남은 객체는 Old 영역으로 Promotion 하고 Old 영역의 메모리가 부족해지만 Major GC가 발생된다.

Young 영역은 일반적으로 Old 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다.
하지만 Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수도 있다. 그렇기 때문에 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다.
