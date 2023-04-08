# SQL
* 책: SQL 첫걸음 - 아사이 아츠시

* DB와 DBMS
    * 과거에는 파일 시스템 사용 => 데이터 중복 => 일관성, 무결성 등 문제가 된다.
    * 공통된 데이터를 공유해서 쓰자! => DB와 DB를 관리하는 시스템인 DBMS의 등장
    * DBMS가 왜 필요한가?
        1. 기존에는 시스템 구축 시 기본 기능부터 구현해야 했다. DBMS는 데이터 검색, 추가, 삭제, 갱신 등의 기본 기능을 제공한다.
        2. 대용량 데이터 저장 및 고속 검색, 복수 유저 요청 대응 등 다양한 기능 제공
        3. 신뢰성 보장. 백업기능, 소프트웨어를 통한 확장성(Scalability)과 부하 분산(Load balancing). (클러스터 구성 또는 스케일 아웃이라고도 함)
    * RDBMS는 클라이언트/서버 모델로 구성된다. 클라이언트는 DB서버에 접속해 SQL 명령으로 DB를 조작할 수 있다.

* SQL (Structured Query Language)
    * RDBMS(관계형 DBMS)를 다루기 위한 언어
    * SQL의 종류
        1. DML(Data Manipulation Language) : 데이터베이스에 CRUD 등 데이터를 조작하는데 사용.
        2. DDL(Data Definition Lang.) : DB 객체(테이블)를 만들거나 삭제하는 등의 명령.
        3. DCL(Data Control Lang.) : 트랜잭션 제어, 접근 권한 제어 등의 명령을 포함.

* DB의 종류
    * 계층형(파일)
    * 관계형(RDB)
    * 객체지향 DB : 객체지향 언어의 객체를 가능하면 그대로 저장
    * XML DB : XML의 형태로 태그를 이용해 마크업 문서로 작성.
    * KVS(키-밸류 스토어) : NoSQL의 등장과 함께 나옴, 열 지향 데이터베이스라고도 한다.

* MySQL 클라이언트
    * MySQL 서버에 접속해 SQL 명령을 수행하기 위한 소프트웨어
    * use [database]의 'use'와 show databases 명령의 'show' 등은 SQL명령이 아닌 MySQL 클라이언트 프로그램의 고유명령이다!

* 루프 백 접속 : PC 한 대로 클라이언트와 서버 모두 실행할 때, 클라이언트에서 서버에 접속하기 위해 네트워크를 경유해서 PC 서버로 되돌아오는 형태의 접속.

* 웹 시스템과 동적 웹의 전체적인 그림을 그려보자.
    1. 클라이언트(브라우저)는 웹 서버에 요청(request)한다.
    2. 웹 서버는 클라이언트의 요청에 대한 결과를 만들기 위해, 웹 서버의 CGI(Common Gateway Interface)를 동작시킨다.
    3. CGI는 프로그램이 DB에 클라이언트로서 접속하여 SQL 명령을 전달한다.
    4. DB 서버는 명령에 대한 결과를 돌려주고, 이를 웹서버가 받아 동적 콘텐츠를 생성한다.
    5. 생성한 콘텐츠로 클라이언트(브라우저)에게 응답(response)한다.



## DB
* DB에는 테이블 외에 다양한 데이터를 저장하거나 관리하는 여러 종류의 '데이터베이스 객체'를 만들 수 있다. (ex) 뷰(view))
* DB 객체는 이름을 붙여 관리하고, 같은 이름으로 다른 DB 객체를 만들 수는 없다.
* DESC [Table] : 테이블 '구조'를 참조한다.
    * 테이블의 열 정보(Field, Type, NULL 제약사항 등)을 확인 가능하다.
        * Field : 열 이름
        * Type : 해당 열의 자료형. INTEGER, CHAR, VARCHAR, DATE, TIME
            * ex) int(11) : 11자리의 정수값을 저장할 수 있는 자료형
            * CHAR는 크기 고정. 빈 공간을 공백문자로 채우고, VARCHAR는 필요한 공간만큼만 사용. 저장공간을 가변적으로 사용한다.
            * CHAR를 고정 길이 문자열, VARCHAR를 가변 길이 문자열이라고 한다.
            * DATE는 날짜-연월일 TIME은 시간-시분초
        * NULL : NULL 값을 허용할 것인지를 나타내는 제약사항. YES/NO
        * Key : 해당 열이 '키'로 지정되어 있는지
        * Default : 해당 열에 주어진 '기본값' 즉, 입력을 생략했을 때 주어지는 값

* 물리삭제와 논리삭제
    * DB에서 데이터를 삭제하는 방법은 용도에 따라 크게 '물리삭제'와 '논리삭제'로 나뉜다.
    * 물리삭제 : 테이블에서 실제 데이터(행)을 삭제하는 것. ex) DELETE
    * 논리삭제 : 실제 테이블의 데이터를 삭제하진 않고, 삭제한 것처럼 보이게 하는 것.
        * 주로, 삭제플래그 열을 추가로 둬서 삭제할 행의 삭제 플래그를 update.
        * 평상시에는 SELECT * FROM table WHERE flag <> 1; 과 같은 식으로 삭제한 것처럼 보이게 한다.
        * ex) 쇼핑사이트의 취소 주문 데이터

* 데이터베이스 객체 : 데이터베이스에서 테이블, 뷰, 인덱스 등 데이터베이스 내에 실체를 가지는 모든 것.
    * 데이터베이스 객체를 작성할 때는 이름을 지정해줘야 하는데, **이름은 겹치지 않아야 한다.**
    * **이름은 객체의 종류와는 관계가 없어서, 뷰나 테이블끼리도 이름은 겹치지 않아야 한다.**
* 스키마 : 데이터베이스 객체를 담는 그릇.
    * **따라서 스키마가 다르면 데이터베이스 객체 이름이 같아도 상관없다.**, 어차피 다르다.
    * MySQL의 CREATE DATABASE로 작성한 '데이터베이스', Oracle에서는 데이터베이스와 데이터베이스 사용자가 계층적 스키마가 된다.
    * 테이블은 열을 정의할 수 있고, 스키마 안에는 테이블을 정의할 수 있다.
        * 각각의 그릇 안에서는 중복되지 않도록 이름을 지정한다. 이렇게 이름이 충돌하지 않도록 하는 그릇을 네임스페이스라고 부르기도 한다.

* **제약(Constraint)**
* 테이블에 제약을 설정함으로써 저장될 데이터를 제한할 수 있다.
    * 열 제약과 테이블 제약
        * 열 제약 : 하나의 열에 대해 정의하는 제약.
            * NOT NULL, UNIQUE 등..
        * 테이블 제약 : 하나의 제약이 복수의 열에 제약을 설명.
            * 대표적으로 **PRIMARY KEY**
    * 제약을 정의하는 방법
        1. 테이블 작성시 제약 정의
        ```
        CREATE TABLE sample (
            a INTEGER NOT NULL,
            b INTEGER NOT NULL UNIQUE,
            c VARCHAR(30),
            CONSTRAINT pkey_sample PRIMARY KEY (a, b)
        );
        ```
        * 테이블을 생성하며 a와 b열에 NOT NULL 및 UNIQUE(b열) '열 제약' 정의, (a, b)로 기본키(유일성) 제약 정의
        * **CONSTRAINT 키워드** : 제약에 이름을 붙여 관리한다.

        2. 제약 추가
            * 기존 테이블에 나중에 제약을 추가한다.
            * 열 제약과 테이블 제약을 추가하는 방법이 조금 다르다.
            1. 열 제약 : 열 정의를 변경
                * 추가 : ALTER TABLE sample MODIFY c VARCHAR(30) NOT NULL;
                * 삭제 : ALTER TABLE sample MODIFY c VARCHAR(30);
            2. 테이블 제약
                * 추가 : ALTER TABLE sample ADD CONSTRAINT pkey_sample PRIMARY KEY (a);
                * 삭제 : 'ALTER TABLE sample DROP CONSTRAINT pkey_sample;' 또는 '~ DROP PRIMARY KEY;' (기본키 제약은 테이블에 유일)

* 기본키 제약(Primary Key)
    * primary key는 행을 구분하기 위해 속성에서 '유일'한 값을 가진다. **중복된 값을 가질 수 없다.**
    * NULL 값을 가질 수 없으므로 기본키로 지정할 열을 NOT NULL 제약이 설정되어야 한다.
    * **복수의 열로 기본키를 구성할 수 있다.** 지정된 열 모두 NOT NULL 제약이 설정되어야 한다.
        * 이 경우 키를 구성하는 모든 열을 사용해서 중복하는 값이 있는지 검사한다.

* 인덱스(=색인) 구조, 인덱스 또한 데이터베이스 객체 중 하나!
    * 테이블에는 인덱스를 작성할 수 있다. 인덱스의 역할을 탐색속도의 향상!
    * 인덱스에는 탐색 시 사용되는 키워드와 대응하는 데이터 행의 장소가 저장.
        * 인덱스는 테이블에 의존하는 객체. 테이블을 삭제하면 대부분 인덱스도 같이 삭제된다. 
    * **인덱스는 테이블과 별개로 독립된 데이터베이스 객체로 작성. 정렬된 상태를 유지한다. => 이진탐색**
        * 테이블의 모든 행을 항상 정렬된 상태로 유지하기는 힘듬 => 인덱스를 분리
        * **인덱스는 보통 이진 트리 구조로, 테이블 데이터와는 별개로 작성된다.
    * 유일성
        * **이진 트리는 값의 중복을 허용하지 않는다. => 테이블의 기본키는 중복을 허용하지 않는다.**

* **ACID**, DB 트랜잭션이 안전하게 수행됨을 보장하기 위한 속성
    * Atomicity, 원자성 : 기냐 아니냐. 트랜잭션의 실행이 중간에 취소되면 해당 트랜잭션은 모든 데이터베이스에 걸쳐 수행되지 않은 것이어야 하고, 그 반대에도 동일하다.
        * 정확하게는 트랜잭션이 더 이상 분할할 수 없는 작업의 단위임을 나타내며, 이 말에 의미는 트랜잭션과 관련된 작업들이 부분적으로 실행되다 중단되지 않는 것이다.
        * 어떤 이유로든 트랜잭션이 실행되다 실패하면, DB를 이전 상태로 롤백해서 트랜잭션 수행 전으로 되돌려서 불완전한 트랜잭션의 흔적을 남기지않아야 한다..
    * Consistency, 일관성 : 데이터베이스에서 수행되는 모든 트랜잭션이 데이터베이스를 일관된 상태로 유지시켜야 한다.
        * 데이터가 데이터베이스 스키마에 정의된 모든 무결성 제약 조건을 충족해야 하며, 데이터에 대한 모든 변경사항이 유효하고 의도한 변경사항을 반영해야 한다.
    * Isolation, 고립 : 동시 트랜잭션이 시스템에 유일한 트랜잭션처럼 실행되야 한다.
        * 각 트랜잭션은 다른 트랜잭션과 독립적으로 수행되야 한다. 
        * 이를 위해 시스템은 각 트랜잭션이 서로 격리되도록 하는 메커니즘을 제공해야 한다.
    * Durability, 지속성 : 트랜잭션이 일단 커밋되면 저장된 내용은 그 후로도 영구적으로 유지되어야한다.
        * 트랜잭션이 커밋되면 데이터에 대한 변경사항은 유지되야 한다. 
        * 즉, 어떤 후속 오류가 발생해도 유지되어야 한다.
        * 모든 트랜잭션은 모든 로그를 남기고 커밋되어야 하고, 남긴 로그로 시스템에 장애가 발생해도 발생 전 상태로 돌아갈 수 있다.

## DML
* SQL의 '구'에는 순서가 정해져 있다. ex) 'SELECT 열 FROM 테이블명 WHERE 조건식'은 가능 => 'SELECT .. WHERE .. FROM ..' 등은 불가
* SELECT : 데이터베이스의 데이터를 읽어오는(**참조하는**) DML 명령
    * SELECT는 '질의' 또는 '쿼리'로 불리기도 한다.
    * ex) SELECT * FROM [Table];        ('*'은 '모든 열'을 의미하는 메타문자)
    * 수치형은 오른쪽 정렬, 문자열, 날짜, 시간형은 왼쪽 정렬된다.

    * SELECT 구 : 열을 선택할 때 사용.
        * ex) SELECT no, name FROM sampletable; : sampletable 테이블에서 no, name 열을 가져온다.
        * 열을 지정하는 순서는 임의대로 정할 수 있다. SQL 명령의 결과는 지정한 순서대로 나온다.
            * 동일한 열을 중복으로 가져올 수도 있다..

    * WHERE 구 : 'WHERE [조건식]' 형태로, 행을 선택할 때 사용.
        * ex) SELECT * FROM sample21 WHERE no = 2; : no 열이 2인 행만 가져온다.
    * LIKE (술어): **패턴 매칭, 문자열의 일부분을 비교**
        * 'WHERE 열 LIKE 패턴' 형식으로 사용한다. 
        * 패턴은 매칭 대상을 지정한다. 수치형 상수는 지정할 수 없으며, 문자열로 지정한다.
        * '=' 연산자는 내용이 완전히 동일한지, LIKE는 특정 문자나 문자열이 포함되어 있는지를 본다.

    * ORDER BY 구 : 행을 정렬해서 결과를 가져올 때 사용.
        * 'WHERE'이 존재하면 그 뒤에, 존재하지 않으면 'FROM' 뒤에 작성한다.
        * 'ORDER BY 열' 형식으로 사용하며, 열 뒤에 DESC(descendant, 하강)를 붙이면 내림차순으로 정렬, 
        * 기본값을 오름차순으로 정렬되며, 열 뒤에 ASC(ascendant, 상승)를 붙여도 된다.
        * 수치형, 날짜, 시간형은 숫자의 대소관계로 정렬되지만, **문자열형의 경우 사전순서로 정렬**된다.
            * 따라서 문자열형 도메인 셀에 숫자를 저장한다면, 정렬 시 주의해야 한다.
        * 마찬가지로 'OREDER BY 열 [ASC/DESC], 열 [ASC/DESC]...' 형태로 여러 정렬 기준을 세울 수 있다.
        * MySQL에서 ASC는 기본값이기 때문에, 안 써도 되지만 DB에 따라 기본값이 다를 수 있다.
        * **가독성을 위해 ASC도, DESC도(당연히) 지정하자.**
        * **NULL 값의 정렬 순서**
            * NULL 값은 표준 SQL에도 정의되어 있지 않아, DB 제품따라 기준이 다르다.
            * **MySQL에서는 NULL 값을 가장 작은 값으로 본다.** (ASC에서 가장 먼저 나옴)

    * LIMIT [OFFSET] 구 : 결과 행을 제한하는데 사용.
        * 커뮤니티 게시판에 페이지가 1, 2, 3... 으로 나뉘는 것 처럼, 결과값을 제한해서 반환하는데 사용한다.
        * 'LIMIT 행수 [OFFSET 시작행]' 형식으로 사용. SELECT 명령의 **마지막에 지정한다.**
        * LIMIT 뒤 '행수'만큼의 행을 결과로 가져오고, 'OFFSET'은 'LIMIT'만큼의 행을 가져올 시작위치를 지정한다.
        * 'OFFSET' 뒤 '시작행' + 1 위치부터 가져온다. 즉, '시작행'까지를 OFF로 만들고 그 다음행부터 가져온다.
        * 이는 내부적으로 WHERE 구로 검색한 후 ORDER BY로 정렬된 뒤 최종적으로 처리된다.

    * GROUP BY 구 : 열에서 같은 값을 가진 행끼리 묶어 **그룹화**한다. 집계함수의 활용 범위를 넓힌다.
        * SELECT name, COUNT(name) FROM sample GROUP BY name;
        * GROUP BY 구에는 그룹화할 열을 지정한다. **복수 열을 GROUP BY에 지정할 수 있다.**
        * 집계함수로 넘겨줄 집합을 '그룹'으로 나눈다. **그룹화된 각각의 그룹이 하나의 '집합'으로 집계함수에 넘겨진다.**
        * **GROUP BY에 지정한 열 이외의 열은 집계함수를 사용하지 않응 채 SELECT 구에 쓸 수 없다.**
            * GROUP BY로 지정된 열은 그룹화되어 각 그룹당 하나의 값만을 가지지만, 그 외의 열들은 여러 값들 중 어떤 값을 가져와야 할지 알 수 없다!
        * GROUP BY 사용시에도 정렬을 위해 ORDER BY를 사용할 수 있다. 아래는 자주 사용되는 패턴이다.
            * SELECT name, COUNT(name), SUM(quantity) AS num FROM sample GROUP BY name ORDER BY num DESC;

    * HAVING 구 : 그룹화한 결과에 조건식을 달아 원하는 값만을 가져올 수 있다.
        * WHERE에 조건식에는 COUNT와 같이 그룹화가 필요한 집계함수는 지정할 수 없다. (WHERE가 GROUP BY보다 먼저 수행된다.)
        * 대신 GROUP BY 뒤에 HAVING을 사용하면, 그룹화된 결과에 조건식을 달 수 있다.
        * **'SELECT name, COUNT(name) FROM sample GROUP BY name HAVING COUNT(name)=1;' 는 가능**
        * 'SELECT name, COUNT(name) FROM sample WHERE COUNT(name)=1 GROUP BY name;' 는 **불가능하다.**
        * 즉, HAVING을 사용하면 WHERE 구와 HAVING 구에 지정된 조건으로 검색하는 2단 구조를 만들 수 있다.
        * 반면, GROUP BY보다 나중에 수행되는 ORDER BY와 같은 경우에는 문제없이 집계함수를 사용할 수 있다.


    * 수치 연산
        * SQL은 DB 조작어이기도 하지만, 컴퓨터를 조작하는 언어이기 때문에 계산기능을 포함한다.
        * 즉, + - * / % 등 기본 연산과 SQRT, ROUND, TRUNCATE, SIN, COS, LOG, MOD 등의 함수를 이용해 연산이 가능하다.
        * ROUND, TRUNCATE 등은 두번째 인수에 소수점 몇번째 자리에서 반올림/버림 할 것인지 지정할 수 있다.
            * 0은 소수점 첫번째, 1은 두번째, -2는 정수부 10의 단위를 의미한다.
        * SELECT 구에서 연산
            * SELECT 구는 열명을 지정한다. 그런데 열명 대신 식을 기술할수도 있다.
            * SELECT *, price*quantity FROM sample; : 이는 sample 테이블의 모든 행과 열 + price와 quantity를 곱해 만든 새로운 열을 반환한다.
        * WHERE 구에서 연산
            * WHERE 구의 조건식에도 연산을 적용할 수 있다.
            * SELECT  *, price*quantity AS amount FROM sample WHERE price*quantity >= 2000;
                * price * quantity가 2000 이상인 행만을 가져온다.
            * **WHERE 구에서는 SELECT 구에서 지정한 별명(alias)를 사용하지 못 한다!!**
                * DB 서버에서 내부적으로 WHERE 구 -> SELECT 구 순서로 처리된다.
                * 많은 DB가 행선택(WHERE) 이후 열선택(SELECT)의 순서로 내부처리를 한다.
                * 별명은 SELECT 구문을 처리할 때 붙여지므로, WHERE 구애서는 별칭을 아직 알지 못 한다!
        * ORDER BY 구에서 연산
            * ORDER BY 구에서 연산을 통해 결과값들을 정렬할 수 있다.
            * 예를 들어 가격 X 수량의 연산 결과 값으로 오름차순 정렬할 수 있다.
            * SELECT *, price*quantity AS amount FROM sample ORDER BY price*quantity;
            * **ORDER BY 구에서는 SELECT에서 지정한 별칭을 사용할 수 있다.**
                * DB서버에서 **내부적으로 WHERE -> SELECT -> ORDER BY 순으로 처리된다.**
        * 열의 별명(alias)
            * 위와같이 식을 통해 새로운 열을 만들 때, 열 이름이 길고 어려운 경우에 별명을 붙여 열 이름을 재지정할 수 있다.
            * SELECT *, price * quantity AS amount FROM sample; : 'AS 별명' 형식으로 새로운 열 뒤에 별명을 붙인다.
            * AS는 생략 가능하나 하지않는 것이 좋다.
            * 별명은 영어, 숫자, 한글로 지정할 수 있는데, 한글로 지정하는 경우 여러 이유로 오작동하는 경우가 많다.
            * 그래서 한글로 지정하는 경우에는 SQL 표준은 더블쿼트(""), MySQL에서는 백쿼트(``)로 감싸 지정한다.
                * ex) SELECT price*quantity AS `금액` FROM sample34;
                * 이는 DB 객체 이름에 ASCII문자 이외의 것을 사용할 경우에 해당한다.
                * 마찬가지로 백쿼트(MySQL)로 감싸면, 예약어도 별명으로 사용이 가능하다.
                    * ANSI_QUOTES SQL 모드가 활성화 된 상태에선, 더블 쿼트를 사용할 수 있다.
                    * 이는 강제로 ANSI SQL 표준을 따르게 한다. 다만, **일반적으로 MySQL은 백쿼트의 사용을 권장한다.**
                * 여러 프로그래밍 언어에서와 마찬가지로 객체 이름은 숫자로 시작할 수 없다.
        * 더블쿼트(""), 싱글쿼트('')
            * 더블 쿼트로 둘러싸면 명령구문을 분석할 때 데이터베이스 객체의 이름이라고 간주한다.
                * MySQL에서는 백쿼트(``)로 사용한다.
            * 싱글 쿼트로 둘렀는 것은 문자열 상수를 의미한다.

    * 집계 연산
        * 집계함수를 이용해서 데이터 집합을 인자로 지정해, 특정 방법으로 계산한 '하나의 값'을 구할 수 있다.
        * **집계 함수는 NULL값을 무시한다!!** NULL 값은 아예 무시하고 계산한다. (집계함수는 SELECT 절에 많이 쓴다)
        * COUNT() : 테이블의 행 개수를 구한다. COUNT(*)은 전체 테이블을 의미하고, COUNT(no), COUNT(name) 등 열을 지정할 수 있다.
        * SUM() : 합계를 구한다. 마찬가지로 NULL은 무시한다. **수치형만 사용 가능하다.**
        * AVG() : 평균을 구한다. 마찬가지로 NULL은 무시한다. **수치형만 사용 가능하다.**
            * if NULL을 0으로 치고 평균을 내고싶다. => case문 사용 가능
            ```
            SELECT COUNT(
                CASE
                WHEN quantity IS NULL THEN 0
                ELSE quantity
                END
            ) AS `avg(null=0)`
            FROM table;
            ```
        * MAX(), MIN() : 집합에서 최대/최소를 구한다. 수치형, 문자열형, 날짜시간형에도 사용 가능하다. NULL은 무시한다.
    
    * DISTINCT/ALL : SELECT 구에서 DISTINCT는 집합 안에 중복된 값을 제거하고 ALL은 전부 반환한다.
        * 중복 여부는 SELECT 구에 지정된 모든 열을 비교해서 판단한다.
        * 집계 함수에 인수에 수식자로 지정해서, 중복을 제거한 뒤 집계할 수 있다.
            * ex) SELECT COUNT(DISTINCT name) FROM sample;

* INSERT
    * 테이블에 새로운 행을 삽입(추가)한다.
    * INSERT INTO 테이블명 (열1, 열2,...) VALUES (값1, 값2,...);
    * 열을 지정해서 행을 추가할 때, 지정하지 않은 열은, 테이블 생성 시 default로 지정한 값이 들어간다.
    * **VALUES에 값에는 NULL이나 DEFAULT(명시적 지정)도 올 수 있다.**
    * DESC로 테이블을 확인했을 때, NULL이 NO로 되어있으면 해당 열에는 NULL 값 저장이 불가능하다.
    * 이렇게 **테이블에 저장하는 데이터를 설정으로 제한하는 것을 통틀어 '제약'이라고 한다.**
        * 위의 경우를 **NOT NULL 제약**이라고 한다.

* DELETE
    * 테이블에 행을 삭제한다.
    * WHERE로 범위를 지정하지 않으면, ex) DELETE FROM sample 테이블의 모든 데이터가 삭제된다.
    * WHERE를 이용해 삭제할 행을 선택한다. DELETE FROM sample WHERE no=3;
    * MySQL은 DELETE 명령에서 OREDER BY와 LIMIT 구를 사용해 삭제할 행을 지정할 수 있다.

* UPDATE
    * 테이블의 셀에 저장되어 있는 값을 갱신한다.
    * UPDATE 테이블명 SET 열=값 [, 열=값] [WHERE 조건식], 갱신할 열과 값을 다중으로 처리 선택할 수 있다.
    * WHERE로 지정하지 않으면, 테이블의 모든 행이 갱신된다.
    * **UPDATE sample SET no=no+1;** 다음과 같은 문장도 가능하다.
        * 물론, NULL값으로 초기화 시킬수도 있다. 'NULL 초기화'
    * MySQL은 복수 열을 동시에 갱신할 때, SET 절에 따라나오는 순서대로 처리한다.

* **서브쿼리**
    * SQL 명령문 안에 지정하는 하부 SELECT 명령. 괄호로 묶어 지정하고 주로 WHERE 절에서 많이 사용한다.
    * 서브쿼리는 SELECT, SET, FROM 등 많은 곳에 사용이 가능하다. 다만, SELECT구에서 서브쿼리를 지정할 때엔 스칼라 서브쿼리가 필요하다.
    * ex) DELETE FROM sample WHERE a = (SELECT MIN(a) FROM sample); **이 명령은 MySQL에서는 불가능**
        * MySQL은 'DELETE FROM sample WHERE a = (SELECT min_a FROM (SELECT MIN(a) AS min_a FROM sample) AS temp);'로 작성
        * **MySQL은 트랜잭션의 원자성을데이터를 추가하거나 갱신할 경우, 동일한 테이블을 서브쿼리에서 사용할 수 없도록 한다.**
        * **위의 방법으로 에러를 발생하지 않고 인라인 뷰로 임시 테이블을 만들어 처리한다**
        * 'AS temp'는 테이블에 별명을 붙인다. **MySQL의 모든 파생 table은 별명을 가져야한다**
    
    * 서브쿼리의 반환 패턴
        1. **스칼라 값**(하나의 값을 반환하는 패턴)
            * 서브쿼리로 사용하기 쉬워서 중요시된다.
            * 스칼라 값을 반환하도록 SELECT 명령을 쓰기 위해선, SELECT 구에서 단일 열을 선택해야 한다.
            * 여러 열을 선택할 경우 3번 또는 4번과 같은 열이 복수인 패턴이 되어버리기 때문
        2. 복수의 행이지만 열은 하나인 패턴
        3. 하나의 행이지만 열이 복수인 패턴
        4. 복수의 행, 복수의 열이 반환되는 패턴
    
    * SELECT 구에서 서브쿼리
        * 열을 지정하므로, **SELECT 구의 서브쿼리는 스칼라 값을 반환해야한다.**
        * SELECT (SELECT COUNT(*) FROM sample51)AS sq1, (SELECT COUNT(*) FROM sample52) AS sq2;

    * SET 구에서 서브쿼리
        * **SET 구에서도 서브쿼리를 사용할 떼, 스칼라 값을 반환해야 한다.**
        * UPDATE sample SET a = (SELECT max_a FROM (SELECT MAX(a) AS max_A FROM) AS temp);

    * FROM 구에서 서브쿼리
        * FROM 구에는 테이블 이외에도 서브쿼리를 사용할 수 있따.
        * **FROM 구에서는 서브쿼리가 스칼라 값을 반환하지 않아도 된다.**
        * SELECT * FROM (SELECT * FROM sample) AS sq;
            * **nested(내포, 중첩) 구조** : SELECT 명령 안에 SELECT 명령이 들어있는 것 같이 보인다. 

    * INSERT 명령과 서브쿼리
        * 두 가지 방법
            1. VALUES 구의 일부로 서브쿼리 사용
                * 스칼라 서브쿼리여야 한다. 해당 열의 값을 서브쿼리의 반환값으로 지정한다.
                * INSERT INTO sample VALUES ((SELECT COUNT(*) FROM sample51), (SELECT COUNT(*) FROM sample52));
            2. VALUES 구 대신 SELECT 명령을 사용
                * SELECT 명령은 꼭 스칼라 서브쿼리일 필요는 없다. **SELECT 명령의 결과를 INSERT INTO 테이블에 전부 추가한다.**
                * **서브쿼리가 반환하는 열 수와 자료형이 INSERT 할 테이블과 일치하기만 하면 된다.**
                * 주로 데이터의 복사나 이동을 위해 사용된다.
                * INSERT INTO sample51 SELECT * FROM sample52; (이 때, sample51과 sample52의 열 구성이 같아야 한다.)

* 상관 서브쿼리
    * 하위쿼리의 자체 쿼리에 대한 매개변수 또는 조건으로 상위쿼리의 열을 하나 이상 사용한다면, 이를 연관되어 있다고 한다.
    * 즉, 상관 서브쿼리는 내부쿼리가 외부쿼리에 따라 결과가 달라지는 서브쿼리를 의미한다.
    * **상관 서브쿼리의 내부쿼리는 외부쿼리의 각 행에 대해 매번 실행된다.**

    * EXISTS (술어) : 서브쿼리가 반환하는 결과값이 있는지를 조사한다.
        * 무엇이든 반환된 행이 있으면 참, 없으면 거짓을 반환한다.
        * ex) 검색할 때 데이터가 존재하는지 아닌지를 판별하기 위해 사용
        ```
        UPDATE sample1 
        SET a = '있음' 
        WHERE EXISTS (
            SELECT * 
            FROM sample2 
            WHERE sample2.no = sample1.no 
        );
        ```
        * sample2 테이블에 no과 일치하는 sample1 테이블 행의 a 값을 '있음'으로 갱신한다.
        * 무엇인가 반환되는 행이 존재하지 않는 상태를 찾기 위해서는 'NOT EXISTS'를 사용한다.
    
    * IN (술어) : 집합(오른쪽)안의 값(왼쪽, 하나의 열 지정)이 존재하는지 조사한다.
        * 'SELECT * FROM sample WHERE no IN (3, 5);' == 'SELECT * FROM sample WHERE no=3 or no=5;'
        * **집합 부분을 서브쿼리로 바꿔도 된다. 물론, 복수의 열을 반환해서는 안 된다.**
            * SELECT * FROM sample1 WHERE no IN (SELECT no2 FROM sample2);
        * 'NOT IN'으로 포함되어 있지 않은 경우를 조사할 수 있다.
        * **집합에 NULL이 포함된 경우, 'IN'과 'NOT IN'은 왼쪽 값이 포함되있지 않으면 NULL을 반환한다.**
            * 포함된 경우에는 원래대로 참/거짓을 반환한다.
        * **비교하는 열 값이 NULL인 경우 그 결과는 NULL이 된다.**

* 클라이언트 변수
    * 변수에 미리 값을 저장해놓고 추후에 사용이 가능하다.
    * MySQL
    ```
    set @a = (SELECT MIN(a) FROM sample);
    DELETE FROM sample WHERE a = @a;
    ```
    * 다만, DELETE 또는 테이블 갱신을 수행하고 나서, sample테이블의 a열의 최소값이 바뀐다고 해도, @a 변수의 저장된 값은 바뀌지 않는다.

* CASE 문
    * CASE문은 주로 간단하게 데이터를 변환하는데 사용한다.
    * CASE문은 검색 CASE와 단순 CASE로 나뉜다.
    * **CASE는 SELECT뿐 아니라 어디에사나 사용이 가능하다.**
    * **NULL 값을 지정하기 위해선 IS NULL 연산을 이용한 '조건식'을 써야하기 때문에 검색 CASE를 사용해야 한다.
    * 검색 CASE
        * CASE WHEN 조건식1 THEN 식1 [WHEN 조건식2 THEN 식2...] [ELSE 식3] END
        * WHEN-THEN는 여러 개 작성이 가능하다. 
        * 모든 조건식에 걸리지 않은 경우 ELSE 절에 식으로 반영되고, **ELSE를 절을 생략할 경우 ELSE NULL로 간주된다.**
        * 따라서 ELSE는 생략하지 않는 것이 좋다.
        * 사용 예
        ```
        // sample 테이블에서 a열과, NULL 값을 0으로 바꾼 a(null=0)열을 가져온다.
        SELECT a, CASE WHEN a IS NULL THEN 0 ELSE a END AS `a(null=0)` FROM sample;
        ```
    * 단순 CASE
        * CASE 식1 WHEN 식2 THEN 식3 [WHEN 식4 THEN 식5...] [ELSE 식6] END
        * CASE의 식1과 같은 WHEN 절을 찾는다. (switch-case와 동일)
        * 검색 CASE는 WHEN 절에 '조건식', 단순 CASE는 '식'을 지정한다.
        * 사용 예
        ```
        SELEC a,
        CASE a
            WHEN 1 THEN '남자'
            WHEN 2 THEN '여자'
            ELSE '미지정'
        END AS "성별" FROM sample;
        ```
        
* COALESCE()
    * COALESCE() 함수를 사용하면 NULL값을 더 쉽게 변환할 수 있다.
    * 여러 개의 인수를 지정할 수 있다. 주어진 인수 가운데 NULL이 아닌 값에 대해서는 가장 먼저 지정된 인수의 값을 반환한다.
    ```
    // 위의 검색 CASE 쿼리와 같은 결과를 보여준다.
    SELECT a, COALESCE(a, 0) FROM sample
    ```



## 연산
* 조건식에서 문자열형, 날짜형, 시간형 등은 '' 싱클 쿼트로 둘러싸 표기해야한다. 
    * 그 외에 날짜형의 연월일은 '-' 하이픈으로, 시간형의 시분초는 ':' 콜론으로 구분하여 표기한다.
    * 수치형은 그대로 표기한다.
* 비교 연산자
    * 'IS NULL' : NULL 값을 검색할 때 사용한다. NULL 값은 '=' 연산자로 검색할 수 없다.
        * NULL 값이 '아닌' 값을 검색하고 싶을 때는 'IS NOT NULL'을 사용한다.
        * ex) SELECT * FROM sample WHERE name IS NULL;
    * '<>' : 값이 서로 다른 경우 '참', 같으면 '거짓'. 즉, '='의 역이다.
* NULL 값의 연산
    * **SQL에서는 NULL로 연산한 결과는 NULL이 된다!**
* 논리 연산자
    * AND
    * OR
        * **AND가 OR에 비해 우선순위가 높다.**
        * 즉, 'a=1 OR a=2 AND b=1 OR b=2' 는 'a=1 OR (a=2 AND b=1) OR b=2'와 같아진다.
        * 따라서 AND와 OR을 같이 쓸 때에는, 생각한 우선순위에 맞춰 적절하게 괄호로 묶어준다.
    * NOT
        * 'NOT 조건식' 형태로 사용. 조건식에 반대되는 값을 반환.

* **패턴 매칭**
    * 메타문자 (와일드카드)
    * 패턴 매칭 시, 임의의 문자 또는 문자열에 매치하는 부분을 지정하기 위해 쓰이는 특수 문자.
        * '%' : 임의의 '문자열'을 의미. 빈 문자열과도 매치된다!
            * 'SQL'이 포함된 문자열을 검색할 때,
                1. 'SQL%' : 전방 일치, SQL로 시작하는 문자열, 즉, 지정한 문자(SQL) 뒤로 임의의 문자열 존재.
                2. '%SQL%' : 중간 일치, 지정한 문자 앞뒤로 임의의 문자열 존재.
                3. '%SQL' : 후방 일치, 앞쪽에 임의의 문자열 존재.
        * '_' : 임의의 '문자' 하나를 의미
            * '*'도  또한 자주 쓰이는 와일드카드이지만, LIKE에서는 사용할 수 없다.
        * 메타문자를 검색하고 싶을 경우.
            * 이스케이프 : '\'를 앞에 붙인다. ex) text LIKE '%\%%'
        * ' 싱글 쿼트를 검색하고 싶은 경우.
            * '를 두개 연속으로 써서 '' 같은 형식으로 사용. ex) It's => 'It''s'

* 문자열 연산
    * CONCAT() : 문자열을 합치는 연산 함수이다. 인자로 여러 문자열을 동시에 받을 수 있다. 
        * CONCAT()은 NULL 값이 인자로 올 경우, 전체 결과를 NULL로 반환한다. 
    * SUBSTRING() : 문자열을 슬라이싱한 결과를 반환한다. 'SUBSTRING(str, start, length)'
        * str의 하위 문자열을 추출한다. start부터 시작해 length만큼의 문자수만큼 반환한다.
        * 대부분의 프로그래밍 언어와는 다르게 **인덱싱의 시작위치는 1부터 시작이다.**
    * TRIM() : 문자열 앞 뒤, 여분의 스페이스를 제거한다.
        * 고정길이 문자열형에 대해 많이 사용한다.
        * 인수를 지정해서 스페이스 이외 문자를 제거할 수 있다.

    * CHAR_LENGTH() or CHARACTER_LENGTH : 문자열의 길이를 반환한다.
    * OCTET_LENGTH() : 문자열의 길이를 바이트 단위로 계산한다. 사용하는 character set에 따라 값이 달라질 수 있다.

* 날짜 연산
    * 날짜/시간 데이터를 저장하는 방법은 DB마다 다르다. 
        * MySQL은 DATE, TIME, DATETIME, TIMESTAMP형을 제공한다.
    * **DATE_FORMAT(날짜, 형식)** : 지정된 형식 문자열로 날짜 값을 문자열로 형식화
        * TO_DATE() : 날짜 데이터의 서식을 임의로 지정, 변환할 수 있는 함수
    * STR_TO_DATE(문자열, 형식) : 문자열을 날짜로 변환.
    * CURRENT_TIMESTAMP : 현재 TIMESTAMP를 YYYY-MM-DD HH:MM:SS 형식으로 반환한다.
        * 함수이지만 인수도 괄호도 필요없다. 
    * CURRENT_TIME : 현재 시간을 HH:MM:SS 형식으로 반환한다.
    * CURRENT_DATE : 현재 날짜를 YYYY-MM-DD 형식으로 반환한다.
    * INTERVAL 1 DAY : interval은 기간형 데이터로 날짜형 데이터와 덧셈 및 뺄셈을 할 수 있다.
    * DATE_DIFF(end, start) : 종료날짜와 시작날짜 간의 차이를(start-end) 일 단위로 반환한다.


## DDL
* Data Definition Language, DDL은 데이터를 정의하는 명령으로, 스키마 내의 객체를 관리할 때 사용한다.
* 테이블의 작성, 삭제, 변경
    * DDL은 모둑 같은 문법을 사용한다. CREATE로 작성, DROP으로 삭제, ALTER로 변경
    * CREATE : 객체를 작성한다. CREATE TABLE, CREATE VIEW 등..  
        * SYNTAX
        ```
        CREATE TABLE table_name (
            no INTERGER [DEFAULT '리터럴'] [NULL / NOT NULL],
            열 정의2,
            열 정의3,
        );
        ```
        * 열 정의 : 열명, 자료형, [DEFAULT '리터럴'] [NULL/NOT NULL] [UNIQUE] ...
            * 그 뒤로 여러 제약 조건을 걸 수 있다. DEFAULT는 생략하면 자동으로 NULL이 들어간다.
            * **NOT NULL 제약조건을 명시하지 않을 경우, 기본적으로 NULL 허용으로 생성된다.**
            * **기본키(primary key)로 설정할 열은 NOT NULL 제약이 선행되어야 한다.**
            * 자료형으로 CHAR나 VARCHAR를 지정할 때에는 꼭 최대길이를 함께 지정한다. ex) VARCHAR(20)

    * DROP : 테이블을 삭제한다. '테이블 삭제 시, 따로 확인하지 않고 수행되니 주의!'
        * DELETE는 데이터의 행 단위 삭제, 만일 테이블을 삭제하지 않고 모든 레코드만을 삭제하고 싶다. 
        * => 'TRUNCATE TABLE 테이블명' : 행 단위로 내부처리가 일어나는 DELETE (WHERE를 지정하지 않은) 보다 빠르다. DDL

    * ALTER : 테이블을 작성한 뒤, 추후에 테이블의 열 구성을 변경할 수 있다.
        * ADD : 테이블의 열 및 제약(constraint)을 추가한다.
            * 기존 데이터행이 존재하면 추가한 열의 값이 모두 기본값(미지정 시, NULL)이 된다.
            * **NOT NULL 제약을 붙여 열을 추가하려면, 먼저 NOT NULL로 제약을 건 뒤 NULL 이외의 기본값을 지정**
            * ALTER TABLE 테이블명 ADD 열 정의
                * ALTER TABLE sample ADD new_col VARCHAR(20) DEFAULT '' NOT NULL;
            * **새로운 열을 추가할 경우, 기존 시스템에서 변경된 테이블에 INSERT 하는 명령을 확인해야 함.**
            
        * MODIFY : 테이블의 열 속성을 수정한다.
            * 기존 데이터 행이 존재하는 경우, 속성 변경에 따라 데이터도 변환된다. (변환 과정에서 오류시 미실행)
            * ALTER TABLE sample MODIFY col VARCHAR(20);

        * CHANGE : 테이블의 열 이름과 동시에 열 정의를 수정한다.
            * 열 이름을 변경할 때 사용한다. 열 이름을 제외한 열의 속성을 변경시에는 대부분 MODIFY를 사용한다.
            * ATLER TABLE sample CHANGE col new_col INTERGER NOT NULL;

        * DROP : 테이블의 열 및 제약을 삭제한다.
            * ALTER TABLE sample DROP col;