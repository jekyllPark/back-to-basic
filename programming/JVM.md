# JVM

- JVM(Java Virtual Machine)
    - 자바 프로그램은 운영체제에서 바로 실행할 수 없다. 자바 프로그램은 완전한 기계어가 아닌, 중간 단계의 바이트 코드이기 때문에 이것을 해석하고 실행할 수 있는 중간 매개체가 필요하다. 그것이 바로 JVM이다.
    - JVM은 OS 위에서 실행되는 가상 머신이며, JAVA와 OS 간의 중개자 역할을 한다.
    - JVM이 있기에 어떤 OS 환경에서도 자바 프로그램을 실행할 수 있는 것이다.
    - 자바의 메모리 영역은 JVM이 OS에게 할당 받은 메모리 영역이라고 할 수 있다.

# 역할

- 자바 애플리케이션을 클래스 로더를 통해 읽어들여 자바 API와 함께 실행해준다.
- JAVA와 OS 사이에 중개자 역할을 수행하여 JAVA가 OS에 구애받지 않고 재사용할 수 있게 해준다.
- 메모리 관리, 가바지 콜렉션을 수행한다.

# 특징

- 스택 기반의 가상 머신이다.
    - ARM 아키텍처 같은 하드웨어는 레지스터 기반으로 동작
- JVM은 한 번의 컴파일로 실행 가능한 기계어가 만들어지는 것이 아닌 기계어로 번역되고 실행되기 때문에 C와 C++의 컴파일 보다는 속도가 느리다.
- 자바는 소스가 들어오면 컴파일러를 통해 바이트 코드가 되고, 이후에 JVM에서 기계어로 변환이 된다.

# 실행 과정

- 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당 받는다.
    - JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
- 자바 컴파일러가 자바 소스코드를 읽어들여 자바 바이트코드로 변환시킨다.
- ClassLoader를 통해 클래스 파일을 JVM으로 로딩한다.
- 로딩된 클래스 파일들은 Execution Engine을 통해 해석이 된다.
- 해석된 바이트 코드는 Runtime Data Area에 배치되어 실질적 수행이 이루어진다.
- 이러한 실행 과정 속 JVM은 필요에 따라 Thread Synchronization과 GC 같은 관리작업을 수행한다.

# 구성

## Class Loader(클래스 로더)

- JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈.
- 런타임 시에 동적으로 클래스를 로드한다.
- jar 파일 내 저장된 클래스들은 JVM 위에 탑재하고 사용하지 않는 클래스들을 메모리에서 삭제한다.
- 자바는 동적 코드, 컴파일 타임이 아니라 런타임에 참조한다.
- 즉, 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크하는 것이다.

## Exceution Engine(실행 엔진)

- 클래스를 실행시키는 역할을 한다.
- 클래스 로더가 JVM 내의 런타임 데이터 영역에 바이트 코드를 배치시키고, 이것은 실행 엔진에 의해 실행된다.
- 자바 바이트코드는 인간이 보기 편한 형태로 기술된 것이다.
    - 그래서 실행 엔진은 이와 같읕 바이트 코드를 JVM 내부에서 기계가 실행할 수 있는 형태로 변경한다.

## Interpreter

- 실행 엔진은 바이트 코드를 명령어 단위로 읽어서 실행한다.
    - 이 방식은 인터프리터 언어의 단점을 그대로 갖고있기에 느리다는 단점이 있다.

## JIT

- 인터프리터 방식의 단점을 보완하기 위해 도입된 컴파일러이다.
- 인터프리터 방식으로 실행하다 적절한 시점에 바이트 코드 전체를 컴파일하여 네이티브 코드로 변경하고, 이후에는 해당 코드를 더 이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식이다.
- 네이티브 코드는 캐시에 보관하기 한 번 컴파일 된 코드는 빠르게 수행할 수 있다.
- 하지만 JIT 컴파일러가 컴파일하는 과정은 바이트 코드를 인터프리팅 하는 것 보다 훨씬 오래 걸리므로 한 번만 실행되는 코드라면 컴파일하지 않고 인터프리팅 하는 것이 유리하다.
- 따라서 JIT 컴파일러를 사용하는 JVM들은 내부적으로 해당 메소드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행한다.

## Runtime Data Area

- 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간

## PC Register

- 스레드가 시작할 때 생성되며 스레드마다 하나씩 존재한다. 만약에 스레드가 자바 메소드를 수행하고 있으면 JVM 명령의 주소를 PC Register에 저장하게 된다. 하지만 다른 언어의 메소드를 수행하고 있다면, undefined 상태가 된다, 왜냐하면 이 두 경우를 따로 처리하기 때문인데, 이 부분이 Native Method Stack Area이다.

## Native Method Stack Area

- 이 공간은 자바 프로그램의 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램의 호출을 저장하는 영역이다.
- JVM은 네이티브 방식을 지원하기 때문에 스레드에서 네이티브 방식의 메소드가 실행되면 Native Method Stack Area에 쌓이게 된다.
- 일반적으로 메소드를 실행하는 경우 JVM 스택에 쌓이다가 해당 메소드 내부에 네이티브 방식을 사용하는 메소드가 있다면 해당 메소드는 네이티브 스택에 쌓인다. 그리고 네이티브 메소드 수행이 끝나면 다시 자바 스택으로 돌아오게 되는데, 네이티브 메소드가 호출한 스택 프레임으로 돌아가는 것이 아닌 새로운 스택 프레임을 하나 생성해서 여기서 다시 작업을 수행한다.
- 그래서 네이티브 코드로 되어 있는 코드로 되어있는 함수의 호출을 자바 프로그램 내에서도 직접 수행할 수 있고 그 결과를 받아올 수 있는 것이다.

## Method Area

- 메소드 영역에서는 코드에서 사용되는 클래스들을 클래스 로더로 읽어 클래스 별로 런타임 상수풀, 필드 데이터, 메소드 데이터, 메소드 코드, 생성자 코드 등으로 분류해서 저장한다.
- 메소드 영역은 JVM이 시작할 때 생성되고, 모든 스레드가 공유하는 영역이다.

## Heap Area

- 힙 영역은 객체와 배열이 생성되는 곳이다.
- 힙 영역에 생성된 객체와 배열은 JVM 스택 영역의 변수나 다른 객체의 필드에서 참조한다.
- 참조하는 변수나 필드가 없으면 의미없는 객체가 되어 이 것을 GC를 통해 힙 영역에서 자동으로 제거한다.
- GC가 있기에 개발자는 객체를 제거하기 위한 별도의 코드를 작성할 필요가 없으며, 자바는 사용자에게 코드로 객체를 직접 제거시키는 방법을 제공하지 않는다.

## Stack Area

- 스택 영역은 각 스레드마다 하나씩 존재하며 스레드가 시작될 때 할당 된다.
- 자바 프로그램에서 추가적인 스레드를 생성하지 않았다면 main 스레드만 존재하여 JVM 스택도 main 스레드의 것 하나 뿐이다.
- JVM 스택은 메소드를 호출할 때마다 프레임을 푸시하고 메소드가 종료되면 해당 프레임을 팝하는 동작을 수행한다.

# 추가로 보면 좋을 포스팅

- 자바8 이후 메소드 영역 변경에 관해
    - https://kkang-joo.tistory.com/18
- 컴파일에 관한 글
    - https://seastar105.tistory.com/136