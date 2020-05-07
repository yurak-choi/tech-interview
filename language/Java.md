# JAVA
- [JVM](https://github.com/yurak-choi/tech-interview/blob/master/language/Java.md#jvm)
- [GC](https://github.com/yurak-choi/tech-interview/blob/master/language/Java.md#gc)

## JVM
**Java Virtual Machine**의 줄임말로 class 파일(Byte Code)을 OS가 이해할수 있도록 해석하여 실행시켜주는 역할을 한다.
- 실행시 JVM의 해석을 거치기 때문에 속도가 느리다는 단점이 있다. 그러나 JIT컴파일러와 최적화 기술이 적용되어 속도의 격차가 많이 줄었다.
- JVM 위에서 OS 상관없이 실행된다. `Write once. run any where.`

### 실행 과정
1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로하는 메모리를 할당받는다. JVM은 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)가 자바 소스코드(.java)를 자바 바이트코드(.class)로 변환시킨다.
3. 클래스 로더를 통해 class 파일들을 JVM 메모리 영역으로 로딩한다.
4. 로딩된 class파일들은 실행 엔진을 통해 해석된다.
5. 해석된 바이트 코드는 메모리 영역에 배치되어 실질적인 수행이 이루어진다.

### JVM 구조
- **Class Loader(클래스 로더)**  
JVM은 런타임시에 클래스를 참조할 때 해당 클래스를 로드하고 메모리 영역에 배치시킨다. 이 `동적로드`를 담당하는 부분이 바로 클래스 로더이다.

- **Execution Engine(실행 엔진)**  
클래스 로더에 의해 메모리에 배치된 바이트코드를 해석하여 실행시킨다.
  - **Interpreter(인터프리터)**  
바이트코드를 명령어 단위로 읽어서 실행한다. 한 줄 씩 수행하기 때문에 느리다.
  - **JIT Compiler(Just-In-Time 컴파일러)**  
인터프리터의 단점을 보완하기 위해 도입되었다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 네이티브 코드로 컴파일하고 이후부터는 인터프리팅 하지 않고 직접 실행한다.  
하지만 인터프리터에 비해 시간이 오래 걸리므로 자주 실행되지 않는 코드라면 인터프리팅하는 것이 유리하다. 따라서 JVM 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하여 특정 범위를 넘을 때에 컴파일을 수행한다.
  - **Garbage Collector**  
GC(Garbage Collection)을 수행하는 모듈 [GC에 대하여](https://github.com/yurak-choi/tech-interview/blob/master/language/Java.md#gc)

- **Runtime Data Area**  
JVM이 OS에게 할당받은 메모리 공간
  - **PC Register**  
스레드가 시작될 때 생성되며 스레드가 어떤 명령을 실행해야 할지 기록하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖는다.
  - **JVM Stack**  
지역변수, 매개변수, 메서드 정보 등의 임시 데이터가 저장되는 영역으로 스레드가 시작될 때 생성되며 메서드 호출시 각각의 스택 프레임이 생성되고 메서드가 종료되면 해당 프레임을 삭제한다.
  - **Native Method Stack**  
자바가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역이다. JAVA Native Interface를 통해 바이트코드로 변환되어 저장된다.
  - **Method Area(=Class Area =Static Area)**  
JVM이 시작될 때 생성되고 클래스를 처음 메모리에 올릴 때 초기화되는 대상을 저장하기 위한 영역이다. 클래스와 인터페이스의 Runtime Constant Pool, 필드 및 메서드의 정보, 바이트코드가 저장된다.
  - **Heap**  
런타임에 동적으로 할당되는 데이터가 저장되는 영역으로 객체나 배열이 저장된다. 메서드영역과 마찬가지로 힙에 할당된 데이터들은 가비지컬렉터의 대상이 된다.

## GC
**Garbage Collection**의 줄임말로 더이상 사용되지 않는 인스턴스를 찾아 메모리에서 삭제하는 작업을 말한다.

### Stop The World
GC을 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것으로 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다.

### Minor GC와 Major GC
Heap은 Young, Old, Permanent 세 영역으로 나뉜다. Young 영역에서 발생하는 GC를 Minor GC, 나머지 영역에서 발생한 GC를 Major GC(Full GC)라고 한다.
- Young 영역 : 새롭게 생성한 객체의 대부분이 여기에 위치한다. 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다.
- Old 영역 : 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다. 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다.

### 참고 블로그
GC의 범위가 넓어 간단히 정리하고 자세한 내용은 블로그 참조로 대체한다.
- Naver D2의 [Java Garbage Collection](https://d2.naver.com/helloworld/1329)
- Naver D2의 [Java Reference와 GC](https://d2.naver.com/helloworld/329631)
