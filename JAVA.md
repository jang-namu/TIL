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

# MyBatis, JDBC, DAO, DTO, VO
* DAO, Data Access Object는 데이터베이스에 접근하여 데이터를 조회, 삽입, 수정, 삭제 등의 작업을 수행하는 객체를 말한다.
* 즉, 이 때 SQL문을 사용하여 데이터베이스와 통신하게 된다. 따라서 DAO 클래스는 DB와 직접적으로 관련된 로직을 처리하게 된다.

* DTO, Data Transfer Object는 계층간 데이터를 교환하기 위해 사용하는 객체이다. 
* 이 객체는 로직이 없이 순수한 getter와 setter만을 갖는 클래스이다.
    * 사용 예로는, 유저가 입력한 데이터를 DB에 넣는 경우가 있다.
    * 유저가 브라우저에서 데이터를 입력하여 form에 있는 데이터를 DTO에 실어 전송한다.
    * DTO를 받은 서버는 DAO를 이용하여 DB로 데이터를 집어넣는다.

* VO, Value Object는 값을 저장하는 객체이고 setter가 존재하지 않고 read-only의 특징을 갖는다.
* 즉, 사용 중 값에 수정이 불가능하고 단지 읽기만 가능한 객체이다.
* DTO와 유사하지만 DTO는 setter가 있어 수정이 가능하고 VO는 불가능하다.

* JDBC, Java DataBase Connectivity는 Java를 이용하여 DB에 접속하고, SQL문을 실행하고 결과를 처리하기 위한 API(Application Programing Interface)이다.
* JDBC만을 이용해 SQL문을 사용하려면, 매번 JDBC 드라이버를 로드하고, DB와의 커넥션을 잡고, SQL문 실행 -> 결과 처리 -> 연결 종료의 단계를 거쳐야해서 불편하다.
* 여기서 나온게 SQL문을 XML파일로 분리해 저장하고, Java 코드로 해당 SQL문 id에 매핑되는 메서드를 선언한 인터페이스를 만들어 사용하는 MyBatis이다.

* MyBatis는 Java 코드로 Mapper 인터페이스를 선언하고 그 안에 XML 파일로 작성한 SQL문과 매핑되는 메서드를 선언한다.
* 그리고, DAO 클래스를 구현해서 SqlSessionFactory를 필드로 갖는다. 
* SqlSessinonFactory는 MyBatis의 핵심 인터페이스 중 하나이다. 이는 SqlSession 객체를 생성하는 역할을 한다.
* 이는 SqlSessionFactoryBuilder에 build() 메소드로 Configuration 객체(XML 또는 자바코드)를 빌드하여 생성된다.
* SqlSession은 데이터베이스와의 세션을 담당한다. 즉, 데이터베이스에 대한 커넥션을 가져와 SQL문을 실행하고 반환한다.
* session.getMapper() 메서드는 인터페이스를 인자로 받아 이를 구현한 클래스의 객체를 생성하고 이를 반환한다.
* 이제 mapper 객체를 이용해서 Mapper 인터페이스에 정의된 메서드를 호출하면, Mapper 인터페이스의 메서드 이름과 XML 파일의 id가 매핑되어 해당 SQL문이 실행된다.
* 이로써 우리는 단순히 DAO 클래스를 인스턴스화하고 DAO 클래스의 메서드를 호출하는 것으로 SQL문을 실행할 수 있다.

# JPA, ORM, Persistence, Hibernate
* JPA, Java Persistence API는 Java에서 ORM 기술 표준으로 사용되는 인터페이스의 모음이다.
* 즉, JPA란 Java Application에서 RDBMS를 사용하는 방식을 정의한 인터페이스이다.
* 실제 구현체는 아니며 구현된 클래스와 매핑해주기 위해 사용되는 프레임워크이다.
* JPA의 대표적인 구현체로는 Hibernate가 있다.

* ORM, Oriented-Relational Mapping(객체 관계 매핑)은 객체(class)와 관계형 DB의 테이블을 매핑한다는 뜻이다.
* 기술적으로는 객체를 DB 테이블에 '자동'으로 영속화해주는 것이다.

* Persistence, 영속성이란 데이터를 생성한 프로세스가 종료되어도 데이터는 사라지지 않는 특성을 얘기한다.
* 대부분의 프로그램은 지속적으로 데이터를 보유하고 있을 필요가 있어 영속성을 가지려고 한다.
* 데이터를 파일, RDB, NoSQL 저장소 등에 저장해서 Persistence를 유지할 수 있다.

* Hibernate, Hibernate는 JPA의 구현체 중 가장 많이 사용되는 구현체이다.
* Hibernate를 사용하면 직접 SQL을 호출하지 않아도, Java에서 메소드를 호출하는 것만으로 DB에 CRUD 할 수 있다.

# Hibernate와 MyBatis 
* Hibernate는 ORM, 즉 객체와 관계 데이터베이스의 테이블을 매핑한다. 반면 MyBatis는 XML이나 Java 코드로 분리된 SQL문과 DAO 객체의 메소드를 매핑한다.
* 이점에서 살펴보자면 MyBatis는 ORM의 기술을 사용하긴 하지만, JPA도 ORM도 아니라고 할 수 있다.

