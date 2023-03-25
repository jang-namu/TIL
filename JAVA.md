# JAVA

# 다차원 배열 선언 순서와 메모리
* 백준 문제를 풀며, 배열이 차지하는 공간과 관련해서 궁금증이 생겼다.
* 배열의 순서에 따라, (예를 들어, int arr[10][2]와 int arr[2][10]) 메모리 사용량이 다르다고 한다. 이유를 알아보자.
* in JAVA에서..
    * JAVA에서 2D 배열은 1D 배열의 배열로 이며, 모든 객체는 요소를 저장하는 용도 외에도 헤더가 필요하다.
    * 아래의 예는 32비트 JVM을 가정한다.
    * 즉, int arr[10][2]는 10개의 1D 배열을 담는 배열까지 총 배열 객체 11개와 2개의 요소로 구성된 10개의 배열이 필요하다.
        * 합계 11개의 배열 객체, 30개(배열을 담는 배열에서 10개 + 요소 20개)의 요소가 필요하다.
    * 반면, int arr[2][10]은 2개의 1D 배열을 담는 배열까지 총 배열 객체 3개와 10개의 요소로 구성된 2개의 배열이 필요하다.
        * 합계 3개의 배열 객체, 22개의 요소 필요.
    * 헤더 크기가 3개의 32비트 워드(32비트 JVM의 경우)이고 참조가 1개의 32비트 워드라고 가정하면
    * int arr[10][2] : 11*3 + 30*1 = 63워드 = 252바이트 소요
    * int arr[2][10] : 3*3 + 22*1 = 31워드 = 124바이트 소요
* 결론 : 가장 큰 차원을 가장 오른쪽에 쓰면 더 적은 메모리를 사용할 수 있다.
    * [다차원 배열과 메모리](https://stackoverflow.com/questions/15339296/does-order-in-a-declaration-of-multidimensional-array-have-an-influence-on-used/15339442#15339442)
    * [공간 지역성](https://eli.thegreenplace.net/2015/memory-layout-of-multi-dimensional-arrays)

# String, StringBuffer, StringBuilder
* 면접을 보다가 내가 잘 모르고 있는 부분이라 정리한다. StringBuffer, StringBuilder는 입, 출력 단계에서 밖에 잘 안 써봤는데..
* String, StringBuffer, StringBuilder 모두 문자열을 다루는 클래스다. Java는 왜 문자열을 다루는 클래스를 여러 개로 나눠놨을까?
    1. String
        * String은 불변성이 특징이다. String으로 생성한 객체(문자열)는 값이 바뀌지 않는다. 아래 코드를 살펴보자
        ```
        String str = new String("Hello"); // 1번
        str = str + " World"    // 2번
        system.out.println(str);    // Hello World
        ```
        * 위 코드는 str 객체의 값이 바뀌는 것 처럼 보인다.
        * 하지만, 실상은 값이 바뀌는 것이 아닌, 2번에서 값을 변경할 때 새로운 객체가 만들어진다.
        * 그리고 str은 새로 만들어진 객체를 참조한다. 1번에서 만든 메모리 영역은 Garbage로 남아있다가, 후에 GC에 의해 처리된다.
        * String은 immutable(불변성)하다. 즉, 문자열을 수정하는 시점에 새로운 인스턴스가 만들어진다.
    2. StringBuffer, StringBuilder
        * StringBuffer와 StringBuilder는 mutable(가변)하다.
        * 이 두 클래스는 append(), delete() 등의 메소드로 동일 객체내에서 문자열 수정이 가능하다.
        * 두 클래스의 차이는 뭘까? 
            * 가장 큰 차이점은 동기화 유무로, StringBuffer는 동기화 키워드를 지원하여 멀티 쓰레드 환경에서 안전(thread-safe)하다는 점이다.
            * 끼어들자면 String도 불변성을 가지기 때문에 thread-safe하다.
            * 반면, StringBuilder 동기화를 지원하지 않는 대신, 동기화를 고려하지 않기 때문에 단일 쓰레드 환경에서 StringBuffer에 비해 성능이 뛰어나다.
