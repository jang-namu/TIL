# SpringBoot

---

<aside>
👨‍💻 SpringBoot를 학습하며 중요 내용을 기록하는 페이지입니다.

작성자: jang-namu
참고자료: 스프링 부트 핵심 가이드 
최초 작성일: 2023.07.08

</aside>

## 6장. 데이터베이스 연동

---

<aside>
☝ 책에서는 MariaDB와 서브파티 GUI 도구인 HeidiSQL을 사용합니다.
HeidiSQL은 Mac 환경에서는 지원되지 않기 때문에,
여기서는 MySQL Workbench를 사용합니다.

</aside>

### 환경 설정

---

1. MariaDB는 homebrew로 다운받았습니다.

```bash
brew install mariadb@10.6
```

1. MariaDB를 실행시킵니다.

```bash
brew services start mariadb@10.6
```

1. 설정을 확인하겠습니다. 설정 파일은 /opt/homebrew/etc/my.cnf 입니다.

![Untitled](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled.png)

- port는 기본적으로 3306을 사용합니다.
- 기본 설정은 localhost(127.0.0.1) 접속만을 허용해둔 상태입니다.
- bind-address를 주석 처리하여, 다른 IP에서의 접속을 허용할 수 있습니다.
1. MariaDB에 접속합니다.

```bash
mysql -u root -p
```

- 접속 권한이 거부(access denied)될 수 있습니다. 이럴 경우 sudo로 접속합니다.
1. root 계정의 비밀번호를 변경합니다. 

```sql
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('비밀번호');
```

## 6.2 ORM

---

<aside>
☝ **ORM, Object-Relational Mapping**

객체지향언어의 객체와 RDB의 테이블을 자동으로 매핑하는 방법. 

둘 사이의 불일치와 제약사항을 해결한다.

</aside>

ORM 프레임워크를 이용하면 쿼리문이 아닌, **코드로 데이터를 조작**할 수 있다.

### 장점

- ORM을 사용하면서 데이터베이스 쿼리를 객체지향적으로 조작할 수 있다.
    - 기존 DB중심의 설계로 객체지향을 제대로 사용하지 못 하는 단점을 보완.
    - SQL을 직접 작성할 일이 줄어, 개발 비용 감소.
- 재사용 및 유지보수가 편리하다
    - 객체단위로 생각. 객체들은 각 클래스로 나뉘어 있어 유지보수 및 재사용이 수월하다.
- 데이터베이스에 대한 종속성이 줄어든다.
    - ORM을 통해 생성된 SQL문은 사용중인 DBMS만 알려주면 자동으로 바뀐다.
    - 즉, DBMS를 교체하는 상황에서도 비교적 적은 리스크만 부담한다.

### 단점

- ORM만으로 구현하기에는 한계가 있다.
    - 복잡한 서비스의 경우 결국 직접 쿼리를 작성해야 한다.
    - 복잡한 SQL을 ORM만으로 구성하게 되면 성능 저하 이슈가 생길수있다.
- 애플리케이션 객체 관점과 DB 관계 관점의 불일치가 발생한다.
    - Granularity: 세분성, ORM의 자동 설계에 따라 DB 테이블 수와 Entity 클래스 수가 다른 경우 발생 (클래스 수> 테이블 수)
    - Intheritance: RDBMS에는 상속이라는 개념이 존재하지 않으므로 불일치  발생.
    - Identity: RDBMS는 primary key로 ‘동일성‘ 정의하는 반면, Java는 두 객체의 값이 같아도 다르다고 판단할 수 있다. (식별과 동일성 문제)
    - Associations: 객체지향에서는 객체 참조를 통해 연관성 표현(방향성 존재), RDB에서는 Foreign key를 삽입해서 연관성 표현(양방향=방향성 없음).
    - Nevigation: 값에 접급하는 방식의 차이. Java는 객체 참조와 같이 객체를 여러번 연결해서 접근하는 그래프 형태의 접근법, RDB에서는 쿼리를 최소화하고 JOIN을 통해 여러 테이블을 로드하고 값을 추출하는 접근 방식을 채택했다.

## 6.3 JPA

---

<aside>
☝ **JPA, Java Persistance API**
Java 진영의 ORM 기술 표준,

Persistance(영속성)이란?, 
→ 데이터를 생성한 프로세스가 종료되더라도, 데이터가 사라지지 않고 유지되는 특성

</aside>

JPA 자체는 실제 구현체가 아니며, Java에서 ORM이 어떻게 동작해야 하는지 설명하는 인터페이스이다. 

![Untitled](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled%201.png)

JPA는 내부적으로 JDBC(Java DataBase Connectivity)를 사용한다. 

개발자가 직접 JDBC를 구현하면 DBMS에 의존적이게 된다. 

JPA는 JDBC를 캡슐화하고 높은 수준의 추상화를 제공하여 코드를 통한 데이터 조작을 가능케한다. 

![Untitled](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled%202.png)

여기서는 가장 널리 사용되고 있는 Hibernate를 사용한다.

### Spring Data JPA

---

JPA를 편리하게 사용할 수 있도록 지원하는 Spring 하위 프로젝트.

- CRUD 처리에 필요한 인터페이스 제공한다.
- Hibernate의 EntityManager를 직접 다루지 않고 Repository를 정의해 사용함으로써, 스프링이 적합한 쿼리를 생성하는 방식으로 데이터베이스를 조작한다.

![Untitled](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled%203.png)

## 6.5 영속성 컨텍스트

---

### Persistance Context

---

- Entity와 레코드의 괴리를 해소 및 객체를 보관하는 기능을 수행한다.
- Entity 객체가 Persistance Context에 들어오면 JPA는 객체의 매핑 정보를 DB에 반영한다.
- Persistance Context에 들어와 JPA의 관리 대상이 된 객체를 영속 객체(Persistance Object)라고 부른다.

![Untitled](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled%204.png)

- 영속성 컨텍스트는 세션 단위의 생명주기를 갖는다. DB에 접근하기 위한 세션이 생성되면 영속성 컨텍스트가 만들어지고, 세션이 종료되면 없어진다.
- EntityManager는 일련의 과정에서 영속성 컨텍스트에 접근하기 위한 수단으로 사용된다.

### EntityManager

---

Entity를 관리하는 객체, 데이터베이스에 접근해서 CRUD 작업을 수행하는 주체.

![Spring Data JPA - SimpleJPARepositary](SpringBoot%2046bc57f95e2d4a47bfecc46b915c6c95/Untitled%205.png)

Spring Data JPA - SimpleJPARepositary

앞서 Spring Data JPA에서는 Hibernate의 EntityManager를 직접 다루지않고 Repositary를 정의하여 사용한다고 했다. 

위 figure에서 보이는 것과 같이 Repositary는 내부적으로 EntityManager를 사용한다.

EntityManager는 EntityManagerFactory가 만든다. 

이는 원래 Hibernate에서는 persistance.xml 설정 파일을 통해 구성하고 사용해야 하지만, 스프링 부트의 자동 설정 기능을 통해 application.properties에 작성한 최소 설정만으로도 동작한다.

엔티티 매니저 팩토리는 애플리케이션에서 단 하나만 생성되며, 모든 엔티티가 공유해서 사용한다. 엔티티 매니저 팩토리로 생성된 엔티티 매니저는 엔티티를 영속성 컨테스트에 추가해서 영속 객체로 만드는 작업을 수행하고, 영속성 컨텍스트와 데이터베이스를 비교하면서 실제 데이터베이스를 대상으로 작업을 수행한다.

### 엔티티의 생명주기

---

영속성 컨텍스트에서 엔티티 객체는 4가지 상태로 구분된다.

1. 비영속(New): 영속성 컨텍스트에 추가되지 않은 엔티티 객체 상테
2. 영속(Managed): 영속성 컨텍스트에 의해 객체가 관리되는 상태
3. 준영속(Detached): 영속성 컨텍스트에 의해 관리되던 엔티티가 컨텍스트와 분리된 상태
4. 삭제(Removed): DB에서 레코드를 삭제하기 위해 컨텍스트에 삭제 요청을 한 상태

참고.

[영속성 컨텍스트와 엔티티의 생명주기](https://siyoon210.tistory.com/138)