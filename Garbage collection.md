# 가비지 컬렉션(Garbage Collection)

## 목차
1. [가비지 컬렉션 정의](##가비지-컬렉션-정의)
2. [필요한 이유](#필요한-이유)
    - [문자열 검색 및 매칭](#문자열-검색-및-매칭)
    - [데이터 유효성 검증](#데이터-유효성-검증)
    - [문자열 처리 및 대체](#문자열-처리-및-대체)
3. [단점](#단점)
4. [기본적인 동작 메커니즘](기본적인-동작-메커니즘)
   - [Reachability](#reachability)
   - [Mark and Sweep](#mark-and-sweep)
5. [가비지 컬렉션 동작 코드 작성](#가비지-컬렉션-동작-코드-작성)
6. [가바지 컬렉션 메모리 leak 발생 코드 작성](#가바지-컬렉션-메모리-leak-발생-코드-작성)

## 가비지 컬렉션 정의
가비지 컬렉션(Garbage Collection)은 자바의 메모리 관리 방법 중의 하나로 JVM(자바 가상 머신)의 Heap 영역에서 동적으로 할당했던 메모리 중 필요 없게 된 메모리 객체(garbage)를 모아 주기적으로 제거하는 프로세스를 말한다.
![Garbage Collection](/img/p1.png "Garbage Collection")   
C / C++ 언어에서는 이러한 가비지 컬렉션이 없어 프로그래머가 수동으로 메모리 할당과 해제를 일일이 해줘야 했었다.
반면 Java에서는 가비지 컬렉터가 메모리 관리를 대행해주기 때문에 Java 프로세스가 한정된 메모리를 효율적으로 사용할수 있게 하고, 개발자 입장에서 메모리 관리, 메모리 누수(Memory Leak) 문제에서 대해 관리하지 않아도 되어 오롯이 개발에만 집중할 수 있다는 장점이 있다.

## 필요한 이유
- 메모리 누수 방지 : 프로그램이 실행되는 동안 메모리를 동적으로 할당하고 사용하지만 개발자가 명시적으로 메모리를 해제하지 않거나 잘못된 방법으로 해제하는 경우 메모리 누수가 발생할 수 있다. 가비지 컬렉션은 이러한 누수된 메모리를 탐지하여 해제함으로써 메모리 사용량을 최적화하고 누수를 방지한다.

- 편의성과 생산성 : 명시적인 메모리 관리는 복잡하고 오류가 발생할 수 있는 작업이지만 가비지 컬렉션은 개발자의 메모리 관리에 대한 부담을 덜 수 있고, 코드 작성과 유지보수에 대한 생산성을 향상시킬 수 있다.

## 단점
이런 만능 같은 가비지 컬렉션에도 단점이 존재한다.

자동으로 처리해준다 해도 메모리가 언제 해제되는지 정확하게 알 수 없어 제어하기 힘들며, 가비지 컬렉션(GC)이 동작하는 동안에는 다른 동작을 멈추기 때문에 오버헤드가 발생되는 문제점이 있다.

이를 전문적인 용어로 Stop-The-World 라 한다.

    STW (Stop The World)
    GC를 수행하기 위해 JVM이 프로그램 실행을 멈추는 현상을 의미.
    GC가 작동하는 동안 GC 관련 Thread를 제외한 모든 Thread는 멈추게 되어 서비스 이용에 차질이 생길 수 있다.
    따라서 이 시간을 최소화 시키는 것이 쟁점이다.
![STW](/img/p2.png)   

## 기본적인 동작 메커니즘
- Reachability(도달능력) : GC는 사용되지 않는 객체를 식별하기 위해 도달성 개념을 사용한다. 즉, 객체가 다른 활성 객체로부터 도달 가능한지 여부에 따라 객체의 생존 여부를 결정한다.

- Mark and Sweep(표시 및 해제) : 일반적인 GC 알고리즘 중 하나로, 객체의 도달 가능 여부를 판별하기 위해 Mark 단계와 사용되지 않는 객체를 해제하는 Sweep 단계로 구성된다.

## Reachability
가비지 컬렉션은 특정 객체가 garbage인지 아닌지 판단하기 위해서 도달성, 도달능력(Reachability) 이라는 개념을 적용한다.   
객체에 레퍼런스가 있다면 Reachable로 구분되고, 객체에 유효한 레퍼런스가 없다면 Unreachable로 구분해버리고 수거해버린다.
![Reachability](/img/p3.png)   
JVM 메모리에서는 객체들은 실질적으로 Heap영역에서 생성되고 Method Area이나 Stack Area 에서는 Heap Area에 생성된 객체의 주소만 참조하는 형식으로 구성된다.   
하지만 이렇게 생성된 Heap Area의 객체들이 메서드가 끝나는 등의 특정 이벤트들로 인하여 Heap Area 객체의 메모리 주소를 가지고 있는 참조 변수가 삭제되는 현상이 발생하면, 위의 그림에서의 빨간색 객체와 같이 Heap영역에서 어디서든 참조하고 있지 않은 객체(Unreachable)들이 발생하게 된다.   
이러한 객체들을 주기적으로 가비지 컬렉터가 제거해주는 것이다.

## Mark and Sweep
Mark-Sweep 이란 다양한 GC에서 사용되는 객체를 솎아내는 내부 알고리즘이다.   
가비지 컬렉션이 동작하는 아주 기초적인 청소 과정이라고 생각하면 된다.
![Mark and Sweep](/img/p4.png)   
원리로는 가비지 컬렉션이 될 대상 객체를 식별(Mark)하고 제거(Sweep)하며 객체가 제거되어 파편화된 메모리 영역을 앞에서부터 채워나가는 작업(Compaction)을 수행하게 된다.   
- Mark 과정 : 먼저 Root Space로부터 그래프 순회를 통해 연결된 객체들을 찾아내어 각각 어떤 객체를 참조하고 있는지 찾아서 마킹한다.
- Sweep 과정 : 참조하고 있지 않은 객체 즉 Unreachable 객체들을 Heap에서 제거한다.
- Compact 과정 : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축한다. (가비지 컬렉터 종류에 따라 하지 않는 경우도 있음)

## 가비지 컬렉션 동작 코드 작성

```java
public class MyClass {
        private String name;
    
        public MyClass(String name) {
            this.name = name;
        }
    
        @Override
        protected void finalize() throws Throwable {
            super.finalize();
            System.out.println(name + " is being finalized.");
        }
    
        public static void main(String[] args) {
            MyClass obj1 = new MyClass("Object 1");
            MyClass obj2 = new MyClass("Object 2");
    
            obj1 = null; // obj1에 대한 참조 제거
            System.gc(); // 가비지 컬렉터 명시적 호출
    
            // 이 지점에서 가비지 컬렉터가 동작하여 "Object 1 is being finalized."을 출력
    
            obj2 = null; // obj2에 대한 참조 제거
            // 가비지 컬렉터가 동작하여 "Object 2 is being finalized."을 출력
        }
    }
```
실제로 자바의 가비지 컬렉터는 애플리케이션의 메모리 사용 패턴에 따라 자동으로 동작합니다. 명시적으로 가비지 컬렉터를 호출해야 하는 경우는 극히 드물며, 대부분의 경우 자바 가비지 컬렉터가 자동으로 메모리를 관리합니다.

## 가바지 컬렉션 메모리 leak 발생 코드 작성
```java
import java.util.ArrayList;
import java.util.List;

public class MemoryLeakExample {
    private static List<String> data = new ArrayList<>();

    public static void main(String[] args) {
        while (true) {
            String input = getUserInput();
            processInput(input);
            data.add(input);
        }
    }

    private static String getUserInput() {
        // 사용자로부터 입력 받는 로직
        return "userInput";
    }

    private static void processInput(String input) {
        // 입력 처리 로직
    }
}

```
### 이 예제에서 메모리 누수 발생하는 이유
data 리스트에 계속해서 데이터가 추가되지만, 더 이상 필요하지 않은 데이터는 제거되지 않기 때문   
즉, data 리스트는 점점 더 커지고 메모리를 계속 점유하게 된다. 이러한 상황에서 가비지 컬렉터는 data 리스트에 대한 참조가 계속 유지되어 있는 한 해당 객체들을 회수하지 못하게 된다.