# backend_intervew
# 면접 질문 리스트

[TOC]

## DB

### ORM(Object Relational Mapping)

* 객체와 관계와의 설정
* 객체와 관계형 DB를 Mapping해준다.
* 객체와 테이블을 Mapping하기 때문에 SQL을 직접 날리는 것이 아니라 마치 자바에서 라이브러리 사용하듯이 사용하면 된다.
* 객체와 관계형 데이터에비스와의 설정을 자동으로 해준다.
* 관계형 데이터베이스의 데이터를 객체형 데이터처럼 사용할 수 있다.
* JPA, Hibernate, MyBatis
* 장점
  * 객체 지향적인 코드로 인해 더 직관적이고 비즈니스 로직에 집중할 수 있게 도와준다.
  * 선언, 할당, 종료 같은 부수적인 코드가 줄어든다.
  * 재사용 및 유지 보수의 편리성이 증가한다.
  * DBMS에 대한 종속성이 줄어든다.
  * 절차적, 순차적 접근이 아닌 객체 지향적 접근으로 인해 생산성이 증가한다.
* 단점
  * 모든 기능을 ORM으로만 작성하기에는 쿼리가 복잡해지면 쓰기 어렵다.
  * 많은 수의 레코드를 잦은 빈도로 벌크 수행
  * ORM으로만 완벽하게 서비스를 구현하기가 어렵다.
  * 프로시저가 많은 시스템에선 ORM의 객체 지향적인 장점을 활용하기 어렵다.

### MySQL 엔진

* InnoDB
  * 기본값 스토리지 엔진
  * 트랜잭션 safe, 커밋과 롤백, 데이터 복구 기능을 제공한다.
  * row-level locking을 제공한다.
  * 데이터를 clusterd index에 저장하여 pk기반 query의 I/O 비용을 줄인다.
  * fk제약을 제공하여 데이터 무결성을 보장한다.
* MyISAM
  * 트랜잭션을 지원하지 않는다.
  * 테이블 단위의 locking을 제공한다.
  * 특정 세션이 테이블을 변경하는 동안 테이블 단위로 lock이 잡힌다.
* Archive
  * 로그 수집에 적합한 엔진
  * 데이터가 메모리 상에서 압축되고 압축된 상태로 디스크에 저장된다.
  * row-level locking이 가능하다.
  * 한번 insert된 데이터는 update/delete가 불가능하다.
  * 인덱스를 지원하지 않는다.
  * 거의 가공하지 않을 윈시 로그 데이터를 관리하는데 효율적이고, 테이블 파티셔닝도 지원한다.
  * 트랜잭션은 지원하지 않는다.

### 인덱스

* RDBMS에서 검색 속도를 높이기 위해 사용하는 기술
* 색인
* 해당 테이블의 컬럼을 색인화(따로 파일로 저장)하여 검색시 테이블 전체를 full scan 하는 것이 아니라 색인화 되어있는 index 파일을 검색하여 검색 속도를 빠르게 한다.
* B+ 트리로 저장된다.
* index로 설정한 컬럼 값이 변경되거나 추가되면, 인덱스 역시 변경된다. 따라서 적절하게 인덱스를 설정해야 한다.
* 데이터의 삽입, 삭제가 빈번한 경우 index의 성능이 떨어진다. 매번 B+트리를 수정해야 하기 때문이다.
* index가 데이터베이스 공간을 차지하기 때문에 추가적인 공간이 필요하다.(10%)
* index는 B+ 트리에서 key값으로 column의 값을 저장하고 있기 때문에 SELECT 조회 시 index로 설정한 데이터만 조회한다면 테이블을 조회하지 않고 인덱스만 조회한다. (covering index)
* index를 생성하는데 시간이 많이 소요될 수 있다.
* SELECT의 WHERE / JOIN / GROUP BY 시에만 인덱스가 사용되며 SELECT의 검색 속도를 빠르게 하는것이 목적이다.
* WHERE의 타겟이 되는 컬럼을 인덱스로 만드는 것이 좋다.
* 데이터 중복도가 높은 컬럼은 인덱스로 만들어도 효과가 없다.
* 외래키가 사용되는 컬럼은 인덱스를 생성해 주는 것이 좋다.
* 사용하지 않는 인덱스는 제거하는 것이 좋다.

### 정규화

* 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화 하는 프로세스이다.
* 관계를 재구성하여 작고 잘 조직된 관계를 생성하는 것에 있다.
* 성능상의 이유로 비정규화 될 수 있다.

### 트랜잭션
* 데이터베이스 관리 시스템에서 상호 작용의 단위이다.
* 트랜잭션은 ACID를 보장해야 한다.
* 원자성(Atomicity) : 한 트랜잭션 내에서 실행한 작업들은 하나의 작업으로 간주한다. 모두 성공하거나, 모두 실패되어야 한다.
* 일관성(Consistency) : 모든 트랜잭션은 일관성있는 데이터베이스 상태를 유지한다. DB에서 정한 무결성 원칙을 항상 만족한다.
* 고립성(Isolation) : 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야 한다.
* 영구성(Durability) : 트랜잭션을 성공적으로 마치면 그 결과가 항상 저장되어야 한다.

### DB Isolation Level
* 격리성 관련 문제점

##### 1.Dirty Read
* 커밋되지 않은 수정 중인 데이터를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생하는 현상이다.
* 어떤 트랜잭션에서 아직 실행이 끝나지 않은 다른 트랜잭션에 의해 변경 사항을 보게 되는 경우이다.

##### 2.Non-Repeatable Read
* 한 트랜잭션에서 같은 SQL을 두 번 수행할 때, 두 SQL의 결과가 다르게 나타나는 비 일관적인 현상이다.
* 한 트랜잭션이 수행중일 때 다른 트랜잭션이 값을 수정, 삭제함으로써 나타난다.

##### 3.Phantom Read
* 한 트랜잭션에서 같은 SQL을 두 번 수행할 때, 첫번째 SQL에서 없던 데이터가 두번째 SQL에서 나타나는 현상이다.
* 한 트랜잭션이 수행중일 때 다른 트랜잭션이 값을 삽입함으로써 나타난다.

#### 트랜잭션 격리 수준
* 트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준이다.
* 데이터베이스는 ACID 같이 트랜잭션이 원자적이면서도 독립적인 수행을 하도록 한다.
* 4단계의 격리 수준을 나누었다.
* 단계를 내려갈수록 격리 수준이 높아져서 언급된 이슈는 적게 발생하지만, 동시 처리 성능은 떨어진다.
* 응답성을 높이기 위해 단계를 올라간다면, 잘못된 값이 처리 될 여지가 있다.
* 트랜잭션이 발생하면 Lock이 걸리는데, SELECT시에는 공유 Lock, CREATE/INSERT/DELETE 시에는 배타적 Lock이 걸린다.

##### 1.Read Uncommitted(레벨 0)
* `SELECT가 수행되는 동안 해당 데이터에 공유 Lock이 걸리지 않는다.`
* 트랜잭션에 처리중인 혹은 아직 커밋되지 않은 데이터를 `다른 트랜잭션이 읽는 것을 허용`한다.
* 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 아직 완료되지 않은(Uncommitted 혹은 Dirty) 트랜잭션이지만 변경된 데이터인 B를 읽을 수 있다.
* 데이터베이스의 일관성을 유지할 수 없다.
* Dirty Read, Non-Repeatable Read, Phantom Read 발생

##### 2.Read Committed(레벨 1)
* `SELECT가 수행되는 동안 해당 데이터에 공유 Lock이 걸린다.`
* 트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 된다.
* 커밋이 이루어진 트랜잭션만 조회할 수 있다.
* 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 `다른 사용자는 해당 데이터에 접근할 수 없다.`
* SQL Server가 Default로 사용하는 단계이다.
* Non-Repeatable Read, Phantom Read 발생

##### 3.Repeatable Read(레벨 2)
* 트랜잭션이 완료될 때까지 `SELECT가 사용하는 모든 데이터에 공유 Lock이 걸린다.`
* 트랜잭션이 범위 내에서 조회한 데이터의 내용이 항상 동일함을 보장한다.
* 다른 사용자는 그 영역에 해당되는 데이터에 대한 `수정이 불가능` 하다.
* Phantom Read 발생

##### 4.Serializable(레벨 3)
* 트랜잭션이 완료될 때까지 `SELECT가 사용하는 모든 데이터에 공유 Lock이 걸린다.`
* 완벽한 읽기 일관성 모드를 제공한다.
* 가장 엄격한 격리 수준이다.
* 다른 사용자는 그 영역에 해당되는 데이터에 대한 `수정 및 입력이 불가능`하다.
* 위 3가지 문제점을 모두 커버 가능하다.
* 동시 처리 성능은 급격히 떨어질 수 있다.

### Partitioning(파티셔닝)

* 수직 파티셔닝
* 큰 테이블이나 인덱스를 관리하기 쉬운 단위로 분리하는 방법을 의미한다.
* 물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.
* 큰 테이블을 제거하여 관리를 쉽게 해준다.
* 특정 DML과 쿼리의 성능을 향상시킨다.
* 주로 대용량 데이터 Write 환경에서 효율적이다.
* 많은 데이터 삽입이 있는 OLTP(Online Transaction Processing) 시스템에서 삽입 작업들을 분리된 파티션들로 분산시켜 경합을 줄인다.
* 테이블간 join에 대한 비용이 증가한다.
* 테이블과 인덱스를 별도 파티션 할 수는 없다. 테이블과 인덱스를 같이 파티셔닝해야 한다.

### Sharding(샤딩)

* 수평 파티셔닝
* 대용량의 데이터를 처리하기 위해 테이블을 수평 분할하여 데이터를 분산 저장하고 처리하는 것을 의미한다.
* 같은 타입의 데이터를 다수의 데이터베이스에 쪼개서 저장하는 것
* 샤딩 알고리즘이 매우 쉽게 일반화가 가능하기에 애플리케이션 레벨이나 데이터베이스 레벨이서 구현이 가능하다.

### Replication(리플리케이션)

* 두 개 이상의 DBMS 시스템을 Master/Slave로 나눠서 동일한 데이터를 저장하는 방식
* Master에는 데이터의 수정 사항을 반영만 하고 Replication을 하여 Salve에 실제 데이터를 복사한다.
* 삽입/수정/삭제는 Master가 담당하고 조회는 Slave가 담당해서 성능 향상 효과를 얻을 수 있다.
* 로그 기반 복제
  * Statement Based : SQL을 복사하여 진행
  * Row Based : SQL에 따라 변경된 Row만 기록하는 방식
  * Mixed : 기본적으로 statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.

### MongoDB

* 도큐먼트 지향 데이터베이스 시스템이다.
* NoSQL
* 똑같은 조건으로 설계했을 시 RDBMS보다 빠르다.
* 트랜잭션을 지원하지 않는다.
* 도큐먼트의 집합을 컬렉션이라고 한다. = RDBMS에서 테이블
* Join을 지원하지 않는다.
* 데이터 갱신 및 입력이 바로 디스크에 쓰이지 않는다.(비동기식) 따라서 데이터가 유실될 가능성이 있다.
* 자체적으로 스케일 아웃을 지원한다.

### Redis

* 인 메모리 데이터베이스
* NoSQL
* 키-값 구조
* C 코드로 개발되었다.
* 데이터를 디스크에 저장하기 때문에 데이터 유실 위험이 memcached에 비해 적다.
* 클러스터링을 지원하지 않는다.
* 샤딩을 통해 확장한다.
* 데이터를 저장하는 방식
  * snapshotting
    * save
      * 순간적으로 redis를 정지시키고, 그 때의 스냅샷을 디스크에 저장한다.
    * blocking
      * 별도의 프로세스를 띄운후, 그 당시의 메모리 스냅샷을 디스크에 저장하며 redis는 정상적으로 작동한다.
  * AOF(Append On File)
    * redis의 모든 write/update 연산 자체를 모두 log 파일에 기록해 두었다가 서버가 재 시작될 때 기록된 로그를 통해 데이터를 복구한다.
    * 기본적으로 non-blocking call이다.
* Single Thread로 작동한다.
* Single Thread로 작동하기 때문에 여러 명령을 script로 한 번에 요청할 경우 transaction level serializable과 같은 효과를 얻을 수 있다.

### Memcached

* DB / API 통신을 줄이기 위해 데이터를 캐싱 처리하는데에 사용하면 좋은 캐시이다.
* 트래픽이 몰려도 응답 속도는 안정적인 편이다.
* Redis에 비해 데이터 타입과 API가 다양하지 않다.
* Key-Value만 지원한다.
* 캐시 솔루션이다. 레디스는 여기에 저장소의 개념이 추가된 것이다.

### MariaDB

* MySQL이 오라클로 넘어간 뒤 불확실한 라이선스 문제를 해결하기 위해 나온 오픈 소스 DBMS이다.
* MySQL의 대체제, 사용법이 같다.

### Datasocurce

* 커넥션 풀의 커넥션을 관리하기 위한 인터페이스이다. (javax.sql.DataSource)
* Datasource를 구현한 대표적인 예가 hikariCP이다. [HikariDataSource](https://github.com/brettwooldridge/HikariCP/blob/dev/src/main/java/com/zaxxer/hikari/HikariDataSource.java#L40)
* 데이터소스 객체를 통해서 필요한 커넥션을 획득, 반납한다.
* JNDI(Java Naming and Directory Interface) Server를 통해 이용된다.

### HikariCP

* 스프링 부트 2.0의 기본 jdbc cp이다.

## Java

### Java 8 변경 사항

* 인터페이스에 디폴트 메소드 추가
* 스트림 api
* 람다
* 함수형 프로그래밍
* Optional

### Interface(인터페이스)

* 클래스들이 구현해햐 하는 동작을 지정하는데 사용되는 추상형
* 규약, 규제
* 추상 클래스의 극단적인 경우
* 클래스와 달리 다중 상속(다중 구현)이 가능하다.
* 객체의 내부 구조를 알 필요 없이 인터페이스의 메소드만 알고 있으면 된다는 장점
* 다형성을 이용한 인터페이스의 구현 객체를 손쉽게 교체할 수 있다.
* 자바 8에서 인터페이스가 가질 수 있는 것
  * 상수 필드(public static final) : 기존, 생략 가능
  * 추상 메소드(public abstract) : 기존, 생략 가능
  * 디폴드 메소드(public default) : 새로 추가됨
    * 이전에 개발한 구현 클래스를 그대로 사용하지 않고 변경하지 않으면서, 새롭게 개발하는 클래스는 디폴트 메소드를 활용해 새로운 기능을 만들 수 있다. 

### 다형성

* 하나의 메소드나 클래스가 상황에 따라 다양한 방법으로 동작하는 것을 의미한다.
* 오버로딩(Overloading)은 다형성의 가장 대표적인 예
* 다형성의 조건
  * 공통의 부모
  * 공통의 메소드 재정의
  * 부모 타입의 변수로 호출

### Generic(제네릭)

* 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에 컴파일 시의 타입 체크를 해 주는 기능이다.
* 클래스 내부에서 사용할 데이터 타입을 나중에 인스턴스를 생성할 때 확정하는 것을 말한다.
* 타입 안정성을 제공한다.
* 타입 체크와 형 변환을 생략할 수 있으므로 코드가 간결해진다.

### mutable & immutable

* mutable
  * 변경 가능 객체
  * 상태를 바꿀 수 있는 객체
  * 최초 생성 이후에도 자유롭게 값 변겨 가능
* immutable
  * 변경 불가 객체
  * 상태를 바꿀 수 없는 객체
  * 최초 생성 이후로는 값 변경 불가

### OOP(Object-Oriented Programming, 객체 지향 프로그래밍)

* 프로그램을 수 많은 객체라는 기본 단위로 나누고 이 객체들의 상호 작용으로 프로그램을 서술하는 방식
* 객체들을 데이터 묶음 뿐만 아니라 하나의 역할을 수행하는 메소드와 데이터의 묶음
* 큰 문제를 작게 쪼개는 것이 아니라, 먼저 작은 문제들을 해결할 수 있는 객체들로 만든 뒤, 이 객체들을 조합해서 큰 문제를 해결하난 상향식(Buttom up) 해결법 도입
* 캡슐화(Encapsulation)
  * 프로그램의 세부 구현을 외부로 드러나지 않도록 특정 모듈(클래스) 내부로 감추는 것이다.
  * 접근 제어 지시자를 통해 외부에서 접근을 제어한다.
* 상속(Inheritance)
  * 자식 클래스가 부모 클래스의 특성과 기능을 그대로 물려받는 것을 말한다.
  * 재정의를 통해 자식 클래스에서 상속받은 기능만들 재 정의해 사용이 가능하다.
* 다형성(Polymorphism)
  * 하나의 변수, 함수 등이 상황에 따라 다른 의미로 해석될 수 있다.
  * 오버로딩

### Reflection

* 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법
* 자바는 동적으로 객체를 생성하는 기술이 없다. 그래서 동적으로 인스턴스를 생성하는 Reflection으로 그 역할을 대신하게 된다.
* Reflection은 구체적인 클래스 타입을 알지 못해도, 그 클래스의 메소드, 타입, 변수들을 접근할 수 있도록 해주는 자바 API이다.

### Annotation

* 자바 소스 코드에 추가하여 사용할 수 있는 메타데이터의 일종이다.
* @기호를 붙여서 사용한다.
* JDK 1.5버전 이상에서 사용 가능하다.
* CompileTime, Runtime시에 해석될 수 있다.
* 클래스 파일에 포함되어 컴파일러에 의해 생성된 후 JVM에 포함되어 작동한다.
* @Target으로 어노테이션이 적용할 위치를 선택한다.
* @Retention으로 컴파일러가 어노테이션을 다루는 방법을 설정한다.

### Enum

* 열거 타입, 변수를 선언할 때 변수 타입으로 사용할 수 있다.
* IDE의 지원을 받을 수 있다.(자동 완성, 오타, 텍스트 리팩토링)
* 허용 가능한 값들을 제한할 수 있다.
* 리팩토링시 변경 범위가 최소화된다. 내용의 추가가 필요하더라도, Enum 코드외에 수정할 필요가 없다.
* 완전한 기능을 갖춘 클래스이다.

### 접근 제어 지시자

| 지시자    | 클래스 내부 | 동일 패키지 | 상속받은 클래스 | 이외의 영역 |
| --------- | ----------- | ----------- | --------------- | ----------- |
| private   | :o:         | :x:         | ❌               | ❌           |
| default   | :o:         | :o:         | :x:             | ❌           |
| protected | :o:         | :o:         | :o:             | ❌           |
| public    | :o:         | :o:         | :o:             | ⭕️           |

* private : 클래스 내부(메소드)에서만 접근을 허용한다.
* default : 클래스 내부와 동일 패키지에서만 접근을 허용한다.
* protected : 클래스 내부와 동일 패키지 상속받은 클래스에서만 접근이 가능하다.
* public : 어디서든 접근이 가능하다.

### String, StringBuilder, StringBuffer

* String은 immutable, private final char[] 형태
* immutable인 이유 : 퍼포먼스, 동시성, GC
* StringBuilder, StringBuffer은 mutable
* StringBuffer는 Thread safe, StringBuilder는 Thread safe하지 않다. 따라서 Multi Thread 환경에서는 StringBuffer를 사용해야 한다.

### Collection

* 데이터를 저장하는 기본적인 자료구조들을 한 곳에 모아 관리하고 편하게 사용하기 위해서 제공한다.
* 모든 Collection의 상위 인터페이스로써 Collection이 갖고 있는 핵심 메소드를 선언한다.
* List
  * 순서가 있는 데이터의 집합이다.
  * 데이터의 중복을 허용한다.
  * LinkedList : 양방향 포인터 구조로 데이터의 삽입, 삭제가 빈번할 경우 빠른 성능을 보장한다. 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 사용된다.
  * ArrayList : 상당히 빠르고 크기를 마음대로 조절할 수 있는 배열이다. 단방향 포인터 구조로 자료에 대한 순차적인 접근에 강점이 있다.
* Set
  * 순서가 없는 데이터의 집합이다.
  * 데이터의 중복을 허용하지 않는다.
  * HashSet : 가장 빠른 임의 접근 속도를 가진다. 순서를 전혀 예측할 수 없다.
  * TreeSet : 정렬된 순서대로 보관하며 정렬 방법을 지정할 수 있다.
  * LinkedHashSet : 추가된 순서, 또는 가장 최근에 접근한 순서대로 접근이 가능하다.
* Map
  * 키-값 쌍으로 이루어진 데이터의 집합이다.
  * 순서는 유지되지 않고, 키는 중복을 허용하지 않는다. 값은 중복을 허용한다.
  * HashMap : Map 인터페이스를 구현하기 위해 HashTable을 사용한 클래스, 중복을 허용하지 않고 순서를 보장하지 않는다. 키와 값으로 null이 허용된다.
  * TreeMap : 이진검색트리의 형태로 키와 값이 쌍으로 이루어진 데이터를 저장한다. 정렬된 순서로 키, 값 쌍을 저장하므로 빠른 검색이 가능하다. 저장시 정렬을 하기 때문에 저장시간이 다소 오래걸린다.
  * HashTable : HashMap보다 느리지만 동기화가 지원된다. 키와 값으로 null이 허용되지 않는다.
  * LinkedHashMap : 기본적으로 HashMap을 상속받아 HashMap과 매우 흡사하다. Map에 있는 엔트리들이 연결 리스트가 유지되므로 입력한 순서대로 반복이 가능하다.

## JVM(Java Virtual Machine)

- 자바 가상 머신
- 자바 애플리케션을 클래스 로더를 통해 읽어 들어 자바 API와 함께 실행하는 것이다.
- 자바 바이트 코드를 실행할 수 있는 주체이다.
- 인터프리터나 JIT컴파일 방식으로 다른 컴퓨터 위에서 자바 바이트 코드를 실행할 수 있도록 구현한다.
- 자바 바이트 코드는 플랫폼에 독립적이다.
- 이론적으로 모든 자바 프로그램은 CPU나 운영체제의 종류와 무관하게 동작할 것을 보장한다.
- 스택 기반, 대다수의 명령어가 스택 선두에서 피연산자를 택하고 다시 스택에 넣는다.
- 포인터를 지원하지만, C처럼 주소 값을 임의로 조작이 가능한 포인터 연산을 지원하지 않는다.
- GC를 사용해 자원을 관리한다.
- 메모리 관리가 가능하다.

### Class Loader

* 각 소스코드 파일을 오브젝트 파일로 변환시켜주는 역할을 한다.
* Loader(로더)
  * 목적 프로그램(기계어 파일)을 실행 가능한 파일로 변환하기 위해 컴퓨터의 주 기억 장소를 할당(Allocation)하거나, 여러개의 목적 프로그램을 연계 편집하여 CPU가 처리할 수 있는 프로그램으로 변환시킨다.
  * 프로그램을 실행하기 위하여 프로그램을 보조 기억 장치로부터 컴퓨터의 주 기억 장치에 올려 놓는 작업을 수행
  * 목적 프로그램들 끼지 연결(Linking)시키거나, 주 기억 장치를 재배치(Relocation)하는 증 포괄적인 작업을 수행
  * 할당(Allocation) -> 연결(Linking) -> 재배치(Relocation) -> 적재(Loading) 순으로 진행
* Linker : 모든 오브젝트 파일들을 하나의 오브젝트 파일로 합친다.
* JVM내로 Class(.class)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.
* Runtime 시점에 Dynamic으로 Class를 Load한다.
* jar 파일 내 저장된 Class들을 JVM 위에 올리고, 사용하지 않는 Class들은 메모리에서 삭제한다.
* class의 instance를 생성하면 Class Loader를 통해 메모리에 Load하게 된다.
* Java는 Dynamic Code
* Compiletime이 아니라 Runtime시에 참조한다. 즉 Class를 처음 참조할 때, 해당 Class를 Load하고 Link한다.

### Runtime Data Areas

* JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당 받은 메모리 공간을 의미한다.
* PC Register
  * CPU는 명령어를 수행하는 동안 필요한 정보를 Register라고 하는 CPU내의 기억장치를 사용한다.
  * Stack - Base로 동작한다.
  * 각 쓰레드별로 하나씩 존재하며, 현재 수행중인 JVM 명령어의 주소를 가지게 된다.
  * 만약 Native Method를 수행한다면 PC Register는 undefined 상태가 된다.
  * PC Register에 저장되는 명령어의 주소는 Native Pointer 일 수도 있고, Method ByteCode일 수도 있다.
  * Native Method를 수행할 때에서는 JVM을 거치지 않고 API를 통해 바로 수행하게 된다.
* JVM Stack
  * JVM Stack은 Thread의 수행 정보를 Frame를 통해서 저장하게 된다.
  * Thread가 시작될 때 생성되며, 각 쓰레드별로 생성이 되기 때문에 다른 쓰레드에는 접근할 수 없다.
  * 메소드가 호출되면 메소드와 메소드 정보는 스택에 쌓이게 되며 메소드 호출이 종료될 때 스택 point에서 제거된다.
  * 메소드 정보는 해당 메소드의 매개변수, 지역변수, 임시변수, 주소(메소드 호출한 주소) 등을 저장하고, 메소드 종료시 메모리 공간이 사라진다.
* Native Method Stack
  * Java외의 언어로 작성된 Native Code들을 위한 스택, 즉 JNI(Java Native Interface)를 통해 호출되는 C/C++ 등의 코드를 수행하기 위한 Stack
* Method Area(Class Area, Static Area)
  * 모든 쓰레드가 공유하는 메모리 영역이다.
  * Method Area는 Class, Interface, Method, Field, Static 변수 등의 Byte Code 등을 보관한다.

* Heap
  * 인스턴스 또는 객체를 저장하는 공간이다.
  * 프로그램상에서 런타임시 다이나믹으로 할당하여 사용하는 공간이다.
  * .class를 이용해 인스턴스를 생성하면 Heap에 저장된다.
  * new 연산자로 생성된 인스턴스를 저장한다.
  * 초기 heap size는 64m이고 최대 heap size는 256m이다.
  * GC의 대상이 된다.
  * 성능 이슈를 일으키는 공간이다.
    * Permanent Generation
      * 생성된 인스턴스의 주소값이 저장된 공간이다.
    * New/Young Area
      * eden : 인스턴스들이 최초로 생성되는 공간
      * survivor 0/1 : eden에서 참조되는 인스턴스들이 저장되는 공간
    * Old
      * new Area에서 일정 시간 참조되고 살아남은 객체들이 저장되는 공간
      * eden 영역에서 인스턴스가 가득차게 되면 첫번째 GC가 발생한다.
      * eden 영역에 있는 값들을 Survivor 1영역에 복사하고, 이 영역을 제외한 나머지 영역의 객체를 삭제한다.
* Runtime Constant Pool
  * 각 클래스와 인터페이스의 상수, 메소드와 필드에 대한 모든 레퍼런스를 담고 있는 테이블이다.
  * 메소드나 필드의 실제 메모리상 주소를 찾을땐 해당 풀을 참조한다.

### Execution Engine

* Load된 Class의 바이트코드를 실행하는 Runtime Module이다.
* Class Loader를 통해 JVM내의 Runtime Data Areas에 배치된 ByteCode는 Execution Engine에 의해 실행되며, 실행 엔진은 자바 바이트코드를 명령어 단위로 읽어서 실행한다.
* 최초의 JVM은 인터프리터 방식이기 때문에 속도가 느린 단점이 있었지만, JIT 컴파일러 방식을 통해 이 점을 보완했다.
* JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정한 기준을 넘어서면 JIT 컴파일러 방식으로 실행한다.

### 실행 과정

1. 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다. JVM은 이 메모리를 용도에 따라 여러 영역(Rumtime Data Areas)으로 나누어 관리한다.
2. Java Compiler(Javac)가 Java SourceCode(.java)를 읽어들여 Java ByteCode(.class)로 변환한다.
3. Class Loader를 통해 class 파일들을 JVM으로 Loading한다.
4. Loading된 class 파일들은 Execution Engine을 통해 해석된다.
5. 해석된 ByteCode는 Runtime Data Areas에 배치되며 실질적인 수행이 이루어지게 된다. 이러한 과정속에서 필요에 따라 Thread Synchronization과 같은 GC 작업을 수행한다.

### JIT(Just In Time) Compiler

* Interpreter 방식의 단접을 보완하기 위해 도입된 컴파일러

* 인터프리터 방식으로 실행하다가 일정한 기준을 넘어서면 바이트코드 전체를 컴파일하여 네이티브 코드로 변환한다.

* 이후에는 더이상 인터프리터 방식으로 컴파일하지 않고, 네이티브 코드로 직접 실행한다.

* NativeCode는 Cache에 보관하기 때문에 한번 Compile된 Code는 빠르게 수행된다.

* JIT Compiler가 Compile하는 과정은 ByteCode를 Interpreting하는 방식보다 훨씬 오래 걸리므로 한 번만 실행되는 Code라면 Interpreter방식이 적절하다.

## Garbage Collection

### Stop-the-world

* GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다.
* GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다.
* GC튜닝은 이 stop-the-world 시간을 줄이는 것이다.

### 개론

* System.gc()메소드를 절대 사용해서는 안된다. 시스템의 성능에 매우 큰 영향을 끼친다.
* 더 이상 필요없는 객체를 찾아 치우는 작업을 한다.
* 두 가지 가설 weak generaional hypothesis
  1. 대부분의 객체는 금방 접근 불가능 상태가 된다.
  2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다.
* 이 가설의 장점을 최대한 살리기 위해서 HotSpot VM에는 Young, Old 두 개의 물리적 공간을 나누었다.

### Young Generation(Young)

* 새롭게 생성한 객체의 대부분이 위치한다.
* 대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 Young영역에 생성되었다가 사라진다.
* 이 영역에서 객체가 사라질 때 Minor GC가 발생한다.
* 3 영역으로 나뉜다. (Eden + Survivor 2개)
* 새로 생성한 대부분의 객체는 Eden 영역에 위치한다.
* Eden 영역에서 GC가 한 번 발생한 후 살아남은 객체는 Survivor 영역중 하나로 이동된다.
* Eden 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 Survivor 영역으로 객체가 계속 쌓인다.
* 하나의 Survivor 영역이 가득 차게 되면 그 중에서 살아남은 객체를 다른 Survivor 영역으로 이동한다. 그리고 가득 찬 Survivor 영역은 아무 데이터도 없는 상태로 된다.
* 이 과정을 반복하다가 계속해서 살아남아 있는 개체는 Old 영역으로 이동하게 된다. 
* Survivor 영역 중 하나는 반드시 비어 있는 상태로 나아 있어야 한다.
* 만약 두 영역에 모두 데이터가 존재하거나, 두 영역 모두 사용량이 0이라면 정상적인 상황이 아니다.

### Old Generation(Old)

* 접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 이곳으로 복사된다.
* Young 영역보다는 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다.
* 이 영역에서 객체가 사라질 때 Major GC(Full GC)가 발생한다.
* Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우 카드 테이블을 이용한다.
* 카드 테이블
  * Old 영역에는 512Byte의 덩어리(chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.
  * Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때 마다 정보가 표시된다.
  * Young 영역의 GC를 실행할 때에는 Old 영역의 모든 객체의 참조를 확인하지 않고, 이 카드 테이블만 뒤져서 GC 대상인지 식별한다
  * write barrier를 사용하여 관리한다. Minor GC를 빠르게 할 수 있도록 하는 장치이다. 약간의 오버헤드는 발생하지만 전반적인 GC 시간은 줄어들게 된다.

### Old GC

* Serial GC
  * 절대 사용하면 안 되는 방식이다.
  * CPU 코어가 1개만 있을 때 사용하기 위해 만든 방식이다.
  * 애플리케이션의 성능이 많이 떨어진다.
  * 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식이다.
  * GC를 처리하는 스레드가 하나다.
  * Old 영역의 GC는 Mark-Sweep-Compact 알고리즘을 사용한다.
    1. Old 영역에 살아 있는 객체를 식별(Mark)한다.
    2. Heap의 앞 부분부터 확인하여 살아 있는 것만 남긴다.(Sweep)
    3. 각 객체들이 연속되게 쌓이도록 Heap의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다.(Compaction)
* Parallel GC
  * GC를 처리하는 쓰레드가 여러 개이다.
  * Serial GC보다 빠르게 객체를 처리할 수 있다.
  * 메모리가 충분하고 코어의 개수가 많을 때 유리하다.
* Parallel Old GC(Parallel Compacting GC)
  * Old 영역의 GC는 Mark-Summary-Compaction 단계를 거친다.
  * Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다.
  * Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르다.
* Concurrent Mark & Sweep GC(CMS)
  * Stop-the-world 시간이 매우 짧다.
  * 모든 애플리케이션의 응답 속도가 매우 중요할 때 CMS GC를 사용한다.
  * Low Latency GC라고도 한다.
  * 다른 GC보다 메모리와 CPU를 더 많이 사용한다.
  * Compaction 단계가 기본적으로 제공되지 않는다.
  * 초기 Initial Mark 단계에서는 클래스 로더에서 가장 가까운 객체 중 살아있는 객체만 찾는 것으로 끝난다. 멈추는 시간은 매우 짧다.
  * Current Mark 단계에서는 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인한다. 다른 스레드가 실행 중인 상태에서 동시에 진행된다.
  * Remark 단계에서는 Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다.
  * Concurrent Sweep 단계에서는 쓰레기를 정리하는 작업을 실행한다. 다른 스레드가 실행되고 있는 상황에서 진행한다.
* G1(Garbage First) GC
  * 가장 성능이 빠르다.
  * 아직 안정화 단계이다.

### Permanent Genration(Perm)

* Method Area
* 객체나 억류(intern)된 문자열 정보를 저장한다.
* Old 영역에서 살아남은 객체가 영원히 남아 있는 곳은 절대 아니다.
* 이 영역에서 발생하는 GC는 Major GC의 횟수에 포함된다.

## Spring

### Spring

* 스스로 발전하는 프레임워크
* 스프링 개발 철학 중 하나는 "항상 프레임워크 기반의 접근 방법을 사용하라"
* 스프링 기능의 대부분은 핵심 기능을 확장해서 발전시킨 결과물이다.
* 단순함과 유연성을 중요 가치로 생각한다.
* 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크
* 필요한 부분만 가져다 사용할 수 있도록 모듈화 되어 있다.
* 각 모듈은 독립적으로 분리되어 있고, 재사용이 가능하다.
* IoC/DI/AOP를 지원한다.

### Spring Boot

* Spring Project의 하나
* 초기 수작업의 셋팅을 자동으로 해준다.
* 프로젝트마다 기본적으로 설정하게 되는 부분들을 이미 내부적으로 가지고 있다.
* Servlet Contatiner를 기본 내장하고 있다.(Tomcat, Jetty)
* Pom.xml에서 의존 라이브러리의 버전을 자동으로 관리해준다.
* 자주 사용하는 프로젝트 조합을 미리 만들어 놓고 스프링을 더욱 쉽고 간단하게 사용하기 위해 만들어졌다.

### Spring Bean Life Cycle

1. Bean 인스턴스화 및 DI
   1. XML파일 / Java Config / Annotation에서 bean 정의를 스캔
   2. bean 인스턴스 생성
   3. bean property에 의존성 주입
2. 스프링인지 여부 검사
   1. bean이 BeanNameAware 인터페이스 구현 시 setBeanName() 호출
   2. bean이 BeanClassLoaderAware 인터페이스 구현 시 setBeanClassLoader() 호출
   3. bean이 ApplicationContextAware 인터페이스 구현 시 setApplicationContext() 호출
3. Bean 생성 생명주기 Callback
   1. @PostConstruct Annotation 적용 메소드 호출
   2. bean이 initializingBean 인터페이스 구현시 afrerPropertiesSet() 호출
   3. bean이 init-method 정의하면 지정한 메소드 호출
4. Bean 소멸 생명주기 Callback
   1. @PreDestory Annotation 적용 메소드 호출
   2. bean이 DispoableBean 인터페이스 구현시 destroy() 호출
   3. bean이 destroy-method 정의하면 지정한 메소드 호출

### IoC Container

* 컨테이너는 보통 인스턴스의 생명 주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 한다.
* 작성한 코드의 처리과정을 위임받은 독립받은 존재이다.
* 적절한 설정만 되어 있다면 누구의 도움 없이도 프로그래머가 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.
* 스프링 컨테이너는 IoC를 이숑해 애플리케이션을 구성하는 빈/컴포넌트들을 관리한다.
* 스프링 컨테이너 = IoC컨테이너 = DI컨테이너
* 스프링 컨테이너는 두 종류가 있다.
  * BeanFactory
    * DI의 기본사항을 제공하는 가장 단순한 컨테이너
    * 팩토리 패턴을 구현한 것
    * Bean을 생성하고 분배하는 책임을 지는 클래스
    * Bean의 정의는 즉시 로딩하지만, 빈 인스턴스 생성은 Lazy Loading한다.
    * 처음으로 getBean()이 호출된 시점에서야 해당 빈을 생성한다.(Lazy Loading)
  * ApplicationContext
    * BeanFactory 인터페이스를 상속 받은 하위 인터페이스이다.
    * 하지만 내부적으로 별도의 BeanFactory를 유지하고 있다.
    * 즉시 인스턴스를 만든다.
    * BeanFactory와 유사한 기능을 제공하지만 좀 더 많은 기능을 제공한다.
    * 국제화가 지원되는 텍스트 메시지를 관리해준다.
    * 이미지 같은 파일 자원을 로드 할 수 있는 포괄적인 방법을 제공한다.
    * Listener로 등록된 Bean에게 이벤트 발생을 알려준다.
    * Context초기화 시점에서 모든 싱글톤 Bean을 미리 로드한 후 애플리케이션을 기동한다.

### IoC(Inversion of Control, 제어의 역전)

* 프로그램의 제어 흐름 구조가 바뀌는 것
* 사용자가 객체를 생성하고 소멸시키는 것이 컨테이너가 대신 하게 된다.(제어의 역전)
* 이 제어권이 스프링 컨테이너로 넘어가는 것이 Spring IoC이다.
* 제어권이 컨테이너로 넘어감으로써 DI, AOP가 가능해졌다.
* 인스턴스의 생성부터 소멸까지의 객체(Bean) 생명주기를 컨테이서가 관리하게 된다.
* 스프링에서 객체가 만들어지는 순서
  1. 객체 생성
  2. 의존성 객체 주입(스스로 만드는 것이 아니라 제어권을 가진 스프링에게 위임하여 스프링이 만드러 놓은 객체를 주입한다.)
  3. 의존성 객체 메소드 호출

### DI(Dependency Injection, 의존성 주입)

* 인스턴스를 자신이 아닌 IoC 컨테이너에서 생성 후 주입한다.
* 내부적으로 new 키워드를 사용하지 않고 setter나 생성자를 이용한다.
* 기능이 변경 될 때 마다 코드를 변경하는 것은 비용이 많이 들게 되므로 가급적 코드의 변화가 적어지도록 프로그램을 작성하기 위해 탄생
* 모듈 간 결합도를 낮춰서 유연한 변경을 가능하도록 한다.
* 불필요한 의존 관계를 없애거나 줄일 수 있다.
* 각 객체를 bean 컨테이너로 관리한다.
* IoC를 구현하는 한 가지 방법이 DI이다.

### AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)

* 애플리케이션 전체에 걸쳐 사용되는 기능들 재사용하도록 지원하는 것이다.
* 가로(횡단) 영역의 공통된 부분을 잘라냈다고 하여 크로스 컷팅(Cross-Cutting)이라고도 불린다.
* 로깅, 트랜잭션, 보안 등
* 로직 주입
* 프록시 패턴과 유사
* 용어
  * Aspect - 어플리케이션에서 분산된 실제 비지니스 코드가 아닌 코드 또는 기능을 말한다. 스프링 AOP에서는 Advice 와 Pointcut을 합친 것을 말한다.
  * JoinPoint - 조인 포인트는 프로그램 실행하는 동안 메소드나 예외 처리와 같은 실행 지점이다. 스프링 AOP에서는 항상 메소드 실행을 의미한다.
  * Advice - 특정 Joinpoint, 즉 특정 지점에서 실행될 행위이다. 스프링 AOP에서는 특정 메소드에 추가할 코드를 의미한다.
  * Pointcut - Advice를 실행할 Joinpoint를 나타내는 표현식이다.
  * Weaving - Pointcut으로 지정한 메소드에 Advice를 적용하는 과정이다. 스프링 AOP에서는 Aspect와 타겟 오브젝트를 연결해주는 과정을 의미한다.
* AOP 구현 방법으로는 Spring AOP와 AspectJ가 있다.
  * 최신 버전의 Spring AOP는 CGLib Proxy를 사용한다. (JDK Proxy vs CGLib Proxy 그림 참조)
  * 둘의 차이점은 Weaving이다.
  * Spring AOP는 CGLib를 사용하기 때문에 클래스를 상속해서 Proxy 패턴을 만든다.
    * 상속을 하기 때문에 셀프 호출에는 Spring AOP를 적용할 수 없다.
  * AspectJ는 Proxy 패턴을 만들지 않고 바이트 코드를 직접 수정한다.
    * 바이트 코드를 직접 수정하기 때문에 셀프 호출을 해도 AOP가 적용된다.
    
<img width="873" alt="JDK Proxy vs CGLib Proxy" src="https://www.baeldung.com/wp-content/uploads/2017/10/springaop-process-1024x504.png">
  

### Spring MVC

1. 클라이언트의 요청에 대한 최초 진입 지점은 DispatcherServlet이 담당하게 된다. 일종의 front controller이다. 이 servlet이 다음 작업을 처리하게 된다.
2. DispatcherServlet은 Spring Bean Definition에 설정되어 있는 Handler Mapping 정보를 참조하여 해당 요청을 처리하기 위한 Controller를 찾는다.
3.  DispatcherServlet은 선택된 Controller를 호출하여 클라이언트가 요청한 작업을 처리한다.
4. Controller는 Business Layer와 통신하여 원하는 작업을 처리한 다음 요청에 대한 성공 유무에 따라 ModelAndView 인스턴스를 반환한다. ModelAndView 클래스에는 UI Layer에서 사용할 Model 데이터와 UI Layer로 사용할 View에 대한 정보가 포함되어 있다.
5. DispatcherServlet은 ModelAndView의 View의 이름이 논리적인 View 정보이면 ViewResolver를 참조하여 이 논리적인 View 정보를 실질적으로 처리해야 할 View를 생성하게 된다.
6. DispatcherServlert은 ViewResolver를 통하여 전달된 View에게 ModelAndView를 전달하여 마지막으로 클라이언트에게 원하는 UI를 제공할 수 있도록 한다. 마지막으로 클라이언트에게 UI를 제공할 책임은 View 클래스가 담당하게 된다.

### Spring Data JPA

* JPA(Java Persistence API) : 자바 영속성
* 도메인 주도 개발이 가능하다.
* 애플리케이션 코드가 SQL 데이터베이스 관련 코드에 잠식 당하는 것을 방지하고 도메인 기반의 프로그래밍으로 비즈니스 로직을 구현하는데 집중할 수 있다.
* 개발 생산성이 좋으며, 데이터베이스에 독립적인 프로그래밍이 가능하다.
* 타입 안정적인 쿼리 작성, Persistent Context가 제공하는 캐시 기능으로 성능 최적화까지 가능하다.
* 영속성 관리와 ORM을 위한 표준 기술이다.
* ORM 표준 기술로 Hibernate, OpenJPA, EclipseLink, TopLink Essentails과 같은 구현체가 있고 이에 표준 인터페이스가 JPA이다.

### MyBatis

* 개발자가 지정한 SQL, 저장프로시저, 그리고 몇가지 고급 매핑을 지원하는 Persistent 프레임워크다.
* JDBC로 처리한는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
* 데이터베이스 결과에 원시 타입과 Map 인터페이스 그리고 POJO를 설정해서 매핑하기 위해 XML과 어노테이션을 사용할 수 있다.
* SqlSessionFactory을 사용한다. 실제 SQL를 호출해주는 역할을 한다.
* Java 소스에서 SQL을 분리해준다.

### 생성자 의존성 주입

* Spring 4.3+부터 생성자가 1개일 경우 @Autowired없이 생성자 의존성 주입이 가능하다.
* 단일 책임의 원칙
  * 생성자의 인자가 많을 경우 코드량도 많아지고, 의존 관계도 많아져 단일 책임의 원칙에 위배된다.
  * 의존관계, 복잡성을 쉽게 알수 있어 리팩토링의 단초를 제공하게 된다.
* 테스트 용이성
  * 특정 DI 컨테이너에 의존하지 않고, POJO여야 한다.
  * DI 컨테이너를 사용하지 않고도 인스턴스화 할 수 있고, 단위 테스트도 가능하며 다른 DI 프레임워크로 전환할 수 있게 된다.
* immutability
  * 필드가 final이 가능해 객체가 변경 불가능 상태가 된다.
* 순환 의존성
* 의존성 명시

### JSP(Java Server Pages)

* 실행 시 서블릿.HttpServlet 클래스를 상속받은 자바 소스코드로 변환한 다음 컴파일되어 실행된다.
* JSP 파일을 서블릿 클래스로 변환하고 실행시켜 주는 역할을 하는 것이 서블릿 컨테이너이다.

## Hystrix

#### Spring Hystrix
* Netflix OSS의 하나
* 분산 환경(MSA)에서 장애 내성과 지연 내성을 위한 라이브러리다.
* 최소한의 부하로 운영이 가능하다.
* Circuit Breaker, DashBoard 기능이 있다.
* 내부적으로 RxJava를 사용하고 있다.
* Circuit Breaker 구현체라고도 한다.

#### Circuit Breaker
* 서비스간 의존성이 발생하는 접근 포인트를 분리시켜서 장애 전파를 막는다.
* Fallback를 제공하여 시스템 장애로부터 복구를 유연하게 한다.
* 스레드 풀 방식과 세마포어 방식이 있다.
* 동기 방식과 비동기 방식으로 구성할 수 있다.

##### Thread Pool
* 서비스 호출이 별도의 스레드에서 수행된다.
* Tomcat 스레드 풀과 서비스 호출 스레드 풀이 격리된다.
* 약간의 오버헤드가 발생한다.
* 기본값이다.

##### Semaphore
* 서비스 호출을 위한 스레드를 생성하지 않는다.
* Tomcat 스레드를 그대로 사용한다.

#### Circuit Breaker 발동 조건(기본 값)
* 20번의 메소드 실행 중, 10번 이상 실패 시 서킷 브레이커가 열린다.
* 19번의 메소드가 실행됬다면, 기본 충족수 미달로 서킷브레이커가 발동하지 않는다.

#### Circuit Breaker 해제 조건(기본 값)
* 5초 이내에 단 하나의 메소드를 다시 실행 후 성공 시 서킷 브레이커가 닫힌다. 실패할 경우 열림이 유지 된다.

#### Circuit Breaker 생명 주기
1. HystrixCommand, HystrixObservableCommand 객체 생성
2. Command 실행
3. 캐시 상태 확인
4. 회로 상태 확인
5. 사용 가능한 Thread Pool / Semaphore가 있는지 확인
6. HystrixCommand.run() / HystrixObservableCommand.construc() 실행
7. Calculate Circuit Health 확인
8. Fallback 실행
9. 응답 반환

#### threadPoolProperties 설정 값
* @HystrixCommand Annotation - threadPoolProperties
* coreSize : Thread Pool 갯수
* maximumSize : 최대 Thread Pool 갯수
* allowMaximunSizeToDriverageFromCoreSize : 최대 Queue 크기 속성 활성화
* maxQueueSize : 최대 Queue Size
* queueSizeRejectionThreshold : 거절된 메소드 Queue Size, 최대 큐 크기에 도달하지 않더라도 거부가 발생하는 queue 크기
* thread.timeoutInMilliseconds : 쓰레드 타임아웃 시간
* circuitBreaker.requestVolumeThreshold : 감시 시간 내 요청 갯수 제한

#### Thread Pool을 사용하여 의존성 격리를 구성한 이유
* Thread를 나누어 다른 Thread에 접근하기 어렵도록 종속성을 원천 차단
* Application은 수 없이 많은 back-end service를 끝없이 호출한다.
* 각 서비스는 Client library를 가지고 있다.
* Client library는 항상 바뀐다.
* Client library는 새로운 네트워크를 호출할 수도 있고, retry, parsing, caching 등의 logic을 가진다.

#### Thread Pool 사용 상 장, 단점
* 새 Client library를 추가할 때의 위험성을 낮출 수 있다. 장애는 격리된 Thread에서 발생한다.
* 애플리케이션이 Client library로 부터 보호된다.
* queueing, scheduling, Context Switching 등의 오버헤드가 발생된다.

#### Thread Pool 비용
* Hystrix는 자식 스레드에서 construct(), run()을 실행할 때, 부모 스레드에서 총 종단 시간을 측정하여 오버헤드를 측정한다.
* Netflix에서는 10억 건 이상의 Hystrix Command를 실행하며 각 API 인스턴스마다 5-20개의 Thread를 가지고 있는 Thread Pool을 40+개를 설정한다.(대부분의 Thread Pool 내의 Thread 개수는 10개)

#### Circut Breaker는 어떻게 메소드 성공, 실패를 기록하는가?
* HystrixCommandMetric는 Hystrix Command 마다 하나씩 생성된다.
* HystrixCommandMetric는 HystrixMetrics를 상속한다.
* HystrixCommandMetric 안에 스레드 safe 하기 위해 private static final ConcurrentHashMap<String, HystrixCommandMetrics> metrics 속성이 있다. 
* metrics에 다시 HystrixCommandMetric이 저장된다.
* getInstance 메소드를 통해 HystrixCommandMetric를 가져오거나 저장한다.
* 내부에 static 클래스로 HealthCounts가 있다.
    * private final long totalCount;
    * private final long errorCount;
    * private final int errorPercentage;
    * 에러 퍼센트는 새로 객체를 생성할 때 계산한다.
    * 세 개의 속성을 가지고, 해당 카운트를 변경하기 위해 plus 메소드를 호출한다.
    * plus 메소드에서는 long[] 타입의 eventTypeCounts를 파라미터로 받는다.
    * long successCount = eventTypeCounts[HystrixEventType.SUCCESS.ordinal()];
    * long failureCount = eventTypeCounts[HystrixEventType.FAILURE.ordinal()];
    * long timeoutCount = eventTypeCounts[HystrixEventType.TIMEOUT.ordinal()];
    * long threadPoolRejectedCount = eventTypeCounts[HystrixEventType.THREAD_POOL_REJECTED.ordinal()];
    * long semaphoreRejectedCount = eventTypeCounts[HystrixEventType.SEMAPHORE_REJECTED.ordinal()];
    * 여기에 이전의 long updatedTotalCount = totalCount;, long updatedErrorCount = errorCount; 더한다.
    * updatedTotalCount += (successCount + failureCount + timeoutCount + threadPoolRejectedCount + semaphoreRejectedCount);
    * 총 카운트 : 성공 카운트 + 실패 카운트 + 시간 초과 카운트 + 스레드 거절 카운트 + 세마포어 거절 카운트 + 이전의 총 카운트
    * updatedErrorCount += (failureCount + timeoutCount + threadPoolRejectedCount + semaphoreRejectedCount);
    * 에러 카운트 : 실패 카운트 + 시간 초과 카운트 + 스레드 거절 카운트 + 세마포어 거절 카운트 + 이전의 에러 카운트
    * return new HealthCounts(updatedTotalCount, updatedErrorCount);
    * 새로 객체를 생성한다.
    * 이 puls 메소드를 호출하는 곳은 어디인가??
* healthCounts의 plus 메소드는 HealthCountsStream에서 사용된다.
* HystrixCircuitBreaker 인터페이스에 makeSuccess메소드는 HystrixCommandMetrics의 resetStream을 실행한다.
* resetStream메소드의 마지막에 healthCountsStream = HealthCountsStream.getInstance(key, properties); 이 있다.
* HealthCountsStream의 getInstance를 할 때 마다 새로 객체가 생성된다.
* AbstractCommand에는 protected final HystrixCircuitBreaker circuitBreaker; 이 있다.
* AbstractCommand의 executeCommandAndObserved Observable에서 circuitBreaker의 markSuccess 메소드를 실행하는 Action1 객체를 생성한다.(RxJava 문법 인듯)
* 이 모든것은 HystrixCommand.run()메소드 실행시 동작

#### Hystrix의 3가지 패턴
1. Circuit Breaker 패턴
2. Bulkhead 패턴
3. Fall back 패턴

## Web

### WAS(Web Application Server)

* 인터넷 상에서 HTTP를 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해 주는 미들웨어
* 동적 서버 컨턴츠를 수행하는 것으로 주로 DB서버와 같이 수행된다.
* 대부분 자바 기반이다.
* 일반적으로 Web Server의 기능을 포함하고 있어 Web Server가 없어도 서비스가 가능하다.
* 여러개의 트랜잭션을 관리한다.
* Servlet 페이지를 HTML 형태로 변환한다.
* JSP의 경우 java class 파일로 컴파일 후 print 형식으로 페이지를 사용자에게 전달한다.
* 사용자의 요청을 스레드 방시식으로 처리한다. 따라서 멀티 쓰레드 기반이다.

### WAS 생명주기

1. Web Server/클라이언트로부터 요청이 들어오면 제일 먼저 컨테이너가 이를 알맞게 처리한다.
2. 컨테이너는 web.xml를 참조하여 해당 서블릿에 대한 스레드를 생성하고 httpServletRequest / httpServletResponse 객체를 생성하여 전달한다.
3. 다음으로 컨테이너는 서블릿을 호출한다.
4. 호출된 서블릿의 작업을 담당하게 된 스레드는 요청에 따라 doPost(), doGet()을 호출한다.
5. 호출된 doPost(), doGet() 메소드는 생성된 동적 페이지를 Response 객체에 실어서 컨테이너에 전달한다.
6. 컨테이너는 전달받은 Response 객체를 HttpResponse 형태로 전환하여 웹 서버에 전달하고 생성되었던 스레드를 종료하고, httpServletRequest / httpServletResponse 객체를 소멸시킨다.

### Tomcat

* Apache 소프트웨어 재단에서 개발한 Servlet Container(Web Container)만 있는 Web Application Server이다.
* Web Server와 연동하여 실행할 수 있는 자바 환경을 제공한다.
* Servlet이나 JSP를 실행하기 위한 Servlet Container을 제공한다.
* 정적 컨텐츠를 로딩하는데 웹 서버보다 수행 속도가 느리다.
* Tomcat 8.5기준
  * MaxThreadPool : 200
  * minSpareThreads : 25
  * MaxQueueSize : Integer.MAX_VALUE

### Servlet

- Java를 사용하여 웹 페이지를 동적으로 생성하는 서버측 프로그램을 말한다.
- 웹 서버의 성능을 향상하기 위해 사용되는 자바 클래스의 일종이다.
- JSP와 비슷한 점이 있지만, JSP가 HTML에 Java코드를 포함하고 있는 반면, Servlet은 Java코드에 HTML을 포함하고 있다.
- 외부 요청마다 Thread로 응답한다.
- Java로 구현되기 때문에 다양한 플랫폼에서 동작한다.

### Web Server

* 클라이언트의 요청을 받아 html이나 오브젝트를 http 프로토콜을 이용해 전송하는 것이다.
* 정적 컨텐츠를 관리한다.(html, css, js, 이미지등)
* Apache, Nginx

### 브라우저에 URL 입력시 동작

1. 주소창에 URL을 입력하고 Enter를 입력한다.
2. 웹 브라우저가 URL을 해석한다.
3. URL이 문법에 맞으면 Punycode 인코딩을 URL의 host 부분에 적용한다.
4. HSTS(HTTP Strict Transport Security) 목록을 로드해서 확인한다.
5. DNS를 조회한다.
6. ARP(Address Resolution Protocol)로 대상(서버)의 IP와 MAC주소를 알아낸다.
7. 서버와 TCP 통신을 통해 Socket를 연다.
8. HTTPS인 경우 TLS HandShake가 추가된다.
9. HTTP 프로토콜로 요청한다.
10. 서버가 응답한다.
11. 웹 브라우저가 결과를 렌더링한다.

### OAuth

* 인터넷 사용자들이 비밀번호를 제공하지 않고 다른 웹 사이트의 자신들의 정보에 대해 웹 사이트나 애플리케이션의 접근 권한을 부여할 수 잇는 공통적인 수단으로 사용된다.
* 접근 위임을 위한 개방형 표준이다.
* OAuth를 이용하면 이 인증을 공유하는 애플리케이션끼리는 별도의 인증이 필요없다.
* 여러 애플리케이션을 통합하여 사용하는 것이 가능하다.
* 용어
  * 사용자(Client) : 서비스 공급자와 소비자를 사용하는 계정을 가지고 있는 개인
  * 소비자(App) : Open API를 이용하여 개발된 OAuth를 사용하여 서비스 제공자에게 접근하는 웹 사이트 또는 애플리케이션
  * 서비스 제공자(Facebook) : OAuth를 통해 접근을 지원하는 웹 애플리케이션(Open API를 제공하는 서비스)
  * 소비자 비밀번호 : 서비스 제공자에서 소비자가 자신임을 인증하기 위한 키
  * 요청 토큰(Request Token) : 소비자가 사용자에게 접근 권한을 인증받기 위해 필요한 정보가 담겨있으며 후헤 접근 토큰으로 변환된다.
  * 접근 토큰(Access Token) : 인증 후에 사용자가 서비스 제공자가 아닌 소비자를 통해서 보호된 자원에 접근하기 위한 키를 포함한 값이다.
* 인증 방식
  1. 소비자(App)가 서비스 제공자(Facebook)에게 요청 토큰(Request Token)을 요청한다.
  2. 서비스 제공자(Facebook)가 소비자(App)에게 요청 토큰(Request Token)을 발급해준다.
  3. 소비자(App)가 사용자(Clinet)를 서비스 제공자(Facebook)로 이동시킨다. 여기서 사용자 인증이 수행된다.
  4. 서비스 제공자(Facebook)가 사용자(Clinet)를 소비자(App)로 이동시킨다.
  5. 소비자(App)가 접근 토큰(Access Token)을 요청한다.
  6. 서비스 제공자(Facebook)가 접근 토큰(Access Token)을 발급한다.
  7. 발급된 접근 토큰(Access Token)을 이용하여 소비자(App)에서 사용자 정보(App이 원하는 진짜 정보)에 접근한다.

### CORS(Cross Origin Resource Sharing)

* Cross-Site Http Request를 가능하게 하는 표준 규약이다.
* 다른 도메인으로부터 리소스가 필요할 경우 cross-site http request가 필요하다.
* 이미지, 링크, 스크립트 태그는 작동 한다.
* 하지만 스크립트 태그로 둘러싸여 있는 스크립트에서 생성된 cross-site http request는 same origin policy를 적용 받기 때문에 cross-site http request가 불가능하다.
* 기존에는 XMLHtppRequest는 보안상의 이유로 자신과 동일한 도메인만 HTTP 요청을 보내도록 제한했다. 즉 cross-origin http 요청을 제한했다.
* Simple/Preflight, Credential/Non-Credential의 조합으로 4가지가 존재한다.
* Simple Request
  * 서버에 1번 요청하고, 서버도 1번 회신하는 것으로 처리가 종료된다.
  * GET, HEAD, POST 중의 한 가지 방식을 사용해야 한다.
  * POST 방식일 경우 Content-type이 셋 중 하나여야 한다.
    * application/x-www-form-urlencoded
    * multipart/form-data
    * text/plain
  * 커스텀 헤더를 전송하지 말아야 한다.
* Preflight Request
  * Simple Request 조건에 해당하지 않으면 브라우저는 Preflight Request 방식으로 요청한다.
  * GET, HEAD, POST외의 다른 방식으로도 요청을 보낼 수 있다.
  * Application/xml처럼 다른 Content-type으로 요청을 보낼 수 있다.
  * 커스텀 해더도 사용할 수 있다.
  * 예비 요청과 본 요청으로 나뉘어 전송된다.
  * 예비 요청과 본 요청에 대한 서버의 응답을 프로그래머가 프로그램 내에서 구분하여 처리하는 것은 아니다.
* Request with Credential
  * HTTP Cookie와 HTTP Authentication 정보를 인식할 수 있게 해주는 요청
  * 요청 시 xhr.withCredentials = true를 지정해서 Credential 요청을 보낼 수 있다.
  * 서버는 Response Header에 반드시 Access-Control-Allow-Credentials: true를 포함해야 하고, Access-Control-Allow-Origin 헤더값에는 * 가 오면 안되며, 구체적인 도메인이 와야한다.
* Request without Credential
  * CORS 요청은 기본적으로 Non-Credential 요청이다.
  * xhr.withCredentials = true를 지정하지 않으면 Non-Credential 요청이다.

### CSRF(Cross-Site Request Forgery, XSRF, 사이트 간 요청 위조)

* 웹 사이트 취약점 공격의 하나이다.
* 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위를 특정 웹 사이트에 요청하게 하는 공격을 말한다.
* 특정 웹 사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 것이다.
* 사용자가 웹 사이트에 로그인한 상태에서 CSRF 코드가 삽입된 페이지를 열면, 공격 대상이 되는 웹 사이트는 위조된 공격 명령이 믿을 수 있는 사용자로부터 발송된 것으로 판단하게 되어 공격에 노출된다.
* **특정 웹 사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 점이다.**
* 공격 과정
  1. 이용자는 웹 사이트에 로그인해 정상적인 쿠키를 발급받는다.
  2. 공격자는 악성 링크를 이메일이나 게시판들의 경로를 통해 이용자에게 전달한다.
  3. 이용자가 공격용 페이지를 열면, 브라우저는 이미지 파일을 받아오기 위해 공격용 URL을 연다.
  4. 이용자의 승인이나 인지 없이 출발지와 도착지가 등록도미으로써 공격이 완료된다.

### XSS(Cross-Site Scripting, 사이트 간 스크립팅, 크로스 사이트 스크립팅)

* 웹 애플리케이션에서 많이 나타나는 취약점의 하나이다.
* 웹 사이트 관리자가 아닌이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다.
* 주로 여러 사용자가 보게되는 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어진다.
* 웹 애플리케션이 사용자로부터 입력 받은 값을 제대로 검사하지 않고 사용할 경우 나타난다.
* 이 취약점으로 해커가 사용자의 정보(쿠키, 세션)을 탈취하거나 자동으로 비정상적인 기능을 수행하게 할 수 있다.
* 주로 다른 웹 사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 한다.
* **사용자가 특정 웹 사이트를 신용하는 점을 노린 것이다.**

### Cookie(쿠키)

* 클라이언트 로컬에 저장되는 키와 값이 들어있는 작은 데이터 파일이다.
* 이름, 값, 만료날짜(쿠키 저장기간), 경로 정보가 들어있다.
* 일정 시간동안 데이터를 저장할 수 있다.(로그인 상태 유지에 활용)
* 클라이언트의 상태 정보를 로컬에 저장했다가 참조한다.
* 쿠키는 사용자가 따로 요청하지 않아도 브라우저가 요청시에 헤더를 넣어서 자동으로 서버에 전송한다.
* 프로세스
  * 브라우저에서 웹 페이지 접속
  * 클라이언트가 요청한 웹 페이지를 받으면서 쿠키를 클라이언트(로컬 스토리지)에 저장
  * 클라이언트가 재 요청시 요청과 함께 쿠키값도 전송
  * 지속적으로 로그인 정보를 가지고 있는 것 처럼 사용

### Session(세션)

* 일정 시간동안 같은 브라우저로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 유지하는 기술
* 웹 브라우저를 통해 웹 서버에 접속한 이루호 브라우저를 종료할 때 까지 유지되는 상태
* 클라이언트가 요청을 보내면 해당 서버의 엔진이 클라이언트에게 유일한 ID를 부여하는데 이것이 세션 ID이다.
* 세션을 구분하기 위해 ID가 필요하고 그 ID만 쿠키를 이용해 저장해놓는다.
* 프로세스
  * 클라이언트가 서버에 접속시 세션 ID를 발급한다.
  * 서버에서는 클라이언트로 발급해준 세션 ID를 쿠키를 사용해 저장한다.(JSESSIONID)
  * 클라이언트는 다시 접속할 때 이 쿠키를 이용해서 세션 ID값을 서버에 전달한다.

### Token(토큰)

* Statelss방식이다. 상태유지를 하지 않는다.
* 사용자의 인증 정보를 서버나 세션에 담아두지 않는다.
* 세션이 존재하지 않으니 로그인 되어있는지 안 되어있는지 신경도 쓰지 않으면서 서버를 손쉽게 확장할 수 있다.
* 모바일 애플리케션에 적합하다.
* 인증 정보를 다른 애플리케이션에 전달할 수 있다.(OAuth)
* 서버 기반 인증의 문제점 - 세션, 확정성, CORS의 문제점을 보완할 수 있다.

### HTTP(Hyper Text Transfer Protocol)

* WWW 상에서 정보를 주고 받을 수 있는 프로토콜
* TCP, UDP, IP 프로토콜을 사용한다.
* 80번 포트를 사용한다.
* 연결 상태를 유지하지 않는 프로토콜이다.
  * 서버에 접속하여 클라이언트의 요청에 대한 응답을 전송 후 연결 종료
  * 전산 자원이 적게 들지만, 클라이언트 구분이 힘들다.
  * 요청이 많아 질 경우 또 다른 문제 발생
  * Cookie, Session, Token등을 사용해 단점 해소
* 클라이언트와 서버 사이에 이루어지는 요청(Request) / 응답(Response) 프로토콜이다.

### HTTP Message

* 서버와 클라이언트 간에 데이터가 교환되는 방식이다.
* Request : 클라이언트에 의해 전달되어 서버의 동작을 일으킨다. 요청 메시지
* Response : Request에 대한 서버의 회신이다. 응답 메시지
* ASCII로 인코딩 된 텍스트 정보로 구성되고 여러 줄에 걸쳐 만들어진다.
* 시작줄 : Http Method, Request Target(요청 대상, URL, 도메인), Http 버전 정보, Http Status Code등을 포함한다.
* 헤더(Header)
* 본문(body, payload) : 전송하고, 전송 받은 데이터를 포함한다

### HTTP2

* Http 1 에서는 앞의 요청의 응답을 받아야 다음 요청이 처리될 수 있다.
* Http 1 에서는 Request를 날릴 때 마다 새로 Connection을 생성했다.
* Http 1.1 에서는 지속 커넥션이라는 개념과 Http 파이프라이닝이라는 개념이 포함됬다. 
  * 커넥션을 재사용 할 수 있고, 서버에 여러 개의 Request를 날릴 수 있게 됬다.
  * Request를 날린 순새대로 Response를 받을 수 있다는 문제점이 있다. 
  * 블록킹이 발생하면 뒤의 요청이 블록킹된다.
* Http 1.1 에서는 1 Request당 1개의 Resource를 받아올 수 있다.(한 커넥션에서 한 번에 하나의 Response를 받을 수 있다.)
* Http 1.1에서는 Plain Text로 구성되어 있다.
* Http 2에서는 Multiplexing 개념이 도입되었다.
* 하나의 커넥션으로 동시에 여러개의 메시지를 주고 받을수 있다.
* 응답은 순서에 상관없이 stream으로 주고 받는다.
* 하나의 Request당 동시에 여러 Resources를 받아올 수 있다.
* 데이터를 전송할 때 Binary로 인코딩하여 전송한다.
* Frame, Stream이라는 개념이 추가됬다.
* Frame : Http2 통신에서 데이터를 주고받을 수 잇는 가장 작은 단위이다.
  * 헤더 프레임, 데이터 프레임으로 구성되어 있다.
* Stream : 클라이언트와 서버 사이에 맺어진 연결을 통해 양방향으로 데이터를 주고 받는 한개 이상의 메시지를 의미
* 프레임이 여러개가 모여 메시지, 메시지가 여러개 모여 스트림이 되는 구조이다.

### HTTPS

* HTTP의 보안이 강화된 버전이다.
* S는 Over Secure Socket Layer의 약자이다.
* 소켓 통신(TCP)에서 일반 텍스트 대신에 SSL이나 TLS 프로토콜을 통해 세션 데이터를 암호화한다.
* 데이터의 적절한 보호를 보장한다.
* 클라이언트 요청시 SSL에 필요한 통신이 추가되기 때문에 통신이 느려진다.
* 암호화 복화화 계산을 하기 때문에 서버나 클라이언트의 자원을 추가적으로 소비한다.
* 증명서를 구입해야 한다.

### URI(Unifrom Resource Identifier, 통합 자원 식별자)

* 인터넷에 있는 자원을 나타내는 유일한 주소이다.
* 자원을 식별할 수 있는 문자열이다.
* 하위 개념으로 URL, URN이 있다.
* 127.0.0.1:8080/users?name=배다슬 은 URI, name=배다슬 이라는 식별자가 있다. 

### URL(Uniform Resource Locator, 유일 자원 지시기)

* 네트워크 상에서 자원이 어디있는지를 알려주기 위한 규약이다.
* 127.0.0.1:8080/users/image.jpg

### URN(Uniform Resource Name, 통합 자원 이름)

* 특정 자원에 대해 그 자원의 위치에 영향을 받지 않는 유일무이한 이름 역할을 한다.
* 자원을 여기저기로 옮기더라도 문제없이 동작한다.
* DNS

### REST(REpresentational State Transfer) API

* 웹의 장점을 최대한 활용할 수 있는 아키텍처이다.
* 브라우저 뿐만 아니라 모바일 디바이스에서도 통신할 수 있어야 한다.
* REST 아키텍처는 Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
* Resource-URI, Method, Message 3가지 요소로 구성된다.
* 리소스 지향 아키텍쳐 스타일이라는 정의 답게 모든 것을 리소스, 명사로 표현한다.
* Uniform Interface : HTTP 표준에만 따른다면 어떠한 기술이라던지 사용이 가능한 인터페이스 스타일이다.
* 무상태성(Stateless) : 상태 정보를 저장하지 않고 각 API 서버는 들어오는 요청만을 들어오는 메시지로 처리하면 된다.
* 캐싱 가능(Cacheable) : HTTP 기존 웹 표준을 그대로 사용하기 때문에 HTTP가 가진 캐싱 기능이 적용 가능하다.
* 계층화 : 클라이언트는 서버와 직접 통신하는지, 중간 서버를 통해 통신하는지 알 수 없다.
* Self-Descriptiveness(자체 표현 구조) : REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다.
* Client-Server Architecture(클라이언트-서버 구조)
  * REST 서버는 API를 제공하고, 제공된 API를 이용해서 비즈니스 로직 처리 및 저장을 책임진다.
  * 클라이언트의 경우 사용자 인증이나 컨택스트(세션, 로그인 정보)등을 직접 관리하고 책임진다.
  * 서로간의 의존성이 줄어들게 된다.

### AJAX(Asychronous JavaScript And XML, 비동기식 자바스크립트와 xml)

* 자바스크립트를 통해서 서버에 비동기 식으로 요청하는 것이다.
* 동작 과정
  1. XMLHTTP Request Oject를 만든다.
     1. 요청을 보낼 준비를 브라우저에게 시키는 과정이다.
     2. 이것을 위해 필요한 method를 갖춘 object가 필요하다.
  2. Callback 함수를 만든다.
     1. 서버에서 응답이 왔을 때 실행시키는 함수이다.
     2. html 페이지를 업데이트한다.
  3. Open a Request
     * 브라우저에게 두 가지 정보를 넘긴다.
       1. 브라우저가 요청을 보내기 위해 사용할 method
       2. 요청이 갈 URL
  4. Send the Request
* 페이지 전체가 아닌 일부분만 갱신할 수 있도록 XML HttpRequest(XHR) 객체를 통해 서버에 요청한다.
* XHR 객체를 형성하고 이 객체의 콜백을 만들고, html 메소드와 url을 결정한 뒤 XHR 객체의 메소드로 정보를 보내는 방식이다.

### Authorization(어쏠라이제이션, 인가)

* 권한 부여
* 해당 자원에 대해서 사용자가 그 자원을 사용할 권한이 있는지 체크하는 권한 체크 과정이다.

### Authentation(어썬티케이션, 인증)

* 인증 과정
* 사용자가 서비스를 사용하는것이 가능하는지를 확인하는 절차이다.

### DNS(Domain Name Server)

* Host의 Domain이름을 Host의 네트워크로 바꾸거나 그 반대의 변환을 수행할 수 있도록 개발되었다.
* WWW이 가능하도록 만드는 기반 기술이다.
* 분산형 데이터베이스 시스템이다.

### POST & PUT & PATCH

* POST : 리소스의 위치를 지정하지 않았을 때 리소스를 생성하기 위해 사용하는 연산이다. Idempotent하지 않다.
* PUT : 리소스의 위치를 명확히 지정한 후 요청을 한다. Idempotent하다.
* PATCH : 리소스의 부분만을 업데이트하기 위해 사용한다. 리소스의 위치를 클라이언트가 알고 있을 때 사용한다.

## 자료구조

### Stack

* FILO
* 아키텍처 수준에서 스택은 메모리의 할당 및 접근 수단을 의미한다.
* 시간 복잡도 : O(n)
* 공간 복잡도 : O(n)

### Queue

* FIFO
* 프린터의 출력 처리, 윈도우의 메시지 처리기, 프로세스 괸리등 사용된다.
* 시간 복잡도 : O(n)
* 공간 복잡도 : O(n)

### Array

* 특정 자료형들이 메모리 공간상에서 연속적으로 이루어져 있는 자료구조이다.
* 데이터를 메모리상에 연속적으로 나열해두었다는 특성 때문에 데이터에 대한 접근이 엄청나게 빠르다.
* index 값을 통해 바로 원하는 공간에 가서 자료를 확인할 수 있기 때문에, 딱 한번의 접근만 필요하다.
* immutable이다.
* 메모리 공간활용에 제약이 있으며, 데이터의 추가와 삭제가 비 효율적이다.
* 시간 복잡도 : O(1)
* 공간 복잡도 : O(n)

### Linked List

* 여러개의 노드들이 연결된 형태의 자료구조이다.
* 메모리 공간상에서 각 노드들이 연속적으로 이루어져 있지 않고 흩어져 있으며, 각각의 노드가 자신의 다음 노드의 위치를 알고 있는 형태로 구현된다.
* 각 노드들이 메모리 공간상의 어디에 위치하는지 각각의 노드들만 알고 있고, 사용자는 제일 첫 노드의 위치만 알고 있게 된다.
* 노드를 추가하고 제고하는 과정을 통해 최대 노드의 수를 언제든지 변경할 수 있기 때문에 공간을 유동적으로 사용할 수 있다.
* 노드의 삽입, 삭제도 배열에 비해 간단하다.
* 데이터의 접근이 다소 느리지만, 메모리 공간 활용 및 데이터의 추가와 삭제가 효율적이다.
* 시간 복잡도 : O(n)
* 공간 복잡도 : O(n)

### Map

* key - value 형태로 삽입되는 자료구조이다.
* ket는 중복이 안된다.
* 1대1 매핑 구조이다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Set

* 순서와 상관없이 어떤 데이터가 존재, 집합하는 자료구조이다.
* 중복을 허용하지 않는다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Tree

* 여러 노드가 한 노드를 가리킬 수 없는 구조이다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Binary Tree(이진 트리)

* 각각의 노드가 최대 두 개의 자식 노드를 가지는 트리 자료구조이다.
* 시간 복잡도 : O(log n)
* 공간 복잡도 : O(n)

### B Tree(B- Tree)

* 데이터베이스와 파일 시스템에서 널리 사용되는 트리 자료구조이다.
* 하나의 노드가 가질 수 있는 자식 노드의 숫자가 2보다 큰 트리 구조이다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### B+ Tree

* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Heap

* 최대값 및 최소값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전 이진 트리이다.
* 최대 힙과 최소 힙이 있다.
* 시간 복잡도 : O(log n)
* 공간 복잡도 : O()

### Hash Table

* 데이터가 저장될 자리가 데이터 해시 값에 의해 결정되는 자료구조이다.
* 해시 테이블의 성능은 공간을 팔아 얻어낸 것이다.
* 저장할 데이터의 값으로 저장할 위치를 계산할 수 있다.
* 평균 상수 시간에 삽입, 삭제, 검색이 가능하다.
* 매우 빠르게 자료를 저장 / 검색해야 하는 경우에 유용하다.
* 충돌 해결 방법
  * Chaning : 저장할 데이터를 Linked List에 저장한다.
  * Linear Probing : 저장할 데이터를 그 다음 칸에 저장한다.
  * Quadratic Probing : 저장할 데이터를 그 다음 칸에 저장할 때, 제곲한 칸에 저장한다.
  * Double Hashing : 건너 뛰는 폭이 매번 다르게 해시를 두번한다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Hash Map

* 내부적으로 Entry<K, V>[] entry의 배열로 되어있다.
* 해당 배열에 인덱스는 내부 해쉬 함수를 통해 계산된다.
* 내부 해시값에 따라서 키 순서가 정해지므로 특정 규칙없이 출력된다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Tree Map

* 내부적으로 Red-Black Tree로 저장된다.
* 키값에 대한 Compartor 구현으로 정렬 순서를 바꿀 수 있다.
* 키값이 정렬된다.
* 트리에 저장되므로 키값은 일정한 기쥰으로 정렬된 상태로 출력된다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

### Linked Hash Map

* Linked List로 저장된다.
* 키값은 입력 순서대로 출력되어서 나온다.
* 시간 복잡도 : O()
* 공간 복잡도 : O()

## 알고리즘

### Bubble Sort(버블 정렬)

* 인접한 두 개를 비교하면서 교환하고 진행한다.
* 하나의 배열만 사용해서 정렬을 진행한다.
* 시간 복잡도 : O(n^2)
* 공간 복잡도 : O(n)

### Selection Sort(선택 정렬)

* 한번 순회를 하면서 가장 큰 수를 찾아서 배열의 마지막 위치와 교환한다.
* 시간 복잡도 : O(n^2)
* 공간 복잡도 : O(n)

### Insertion Sort(삽입 정렬)

* 현재 위치보다 아래쪽을 순회한다.
* 현재 위치의 값을 현재 위치보다 아래쪽으로 순회하며 알맞은 위치에 넣어준다.
* 시간 복잡도 : O(n^2), 이미 정렬이 되어있다면 O(n)
* 공간 복잡도 : O(n)

### Merge Sort(병합 정렬)

* 정렬할 리스트를 반으로 쪼개나가며 좌, 우 리스트를 계속하여 분할해 나간 후 각 리스트 내에서 정렬 후 병합, 정렬 후 병합하는 과정을 통해 정렬한다.
* 시간 복잡도 : O(n log n)
* 공간 복잡도 : O()

### Quick Sort(퀵 정렬)

* real-world 데이터에서 빠르다고 알려져 있어 가장 많이 쓰는 알고리즘이다.
* pivot을 선정하여 pivot을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 재배치를 하고 계속해서 분할하여 정렬한다.
* 시간 복잡도 : O(n log n), 최악의 경우 O(n^2)
* 공간 복잡도 : O()

### Heap Sort(힙 정렬)

- 최대값 및 최소값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전 이진트리를 사용하는 알고리즘
- 시간 복잡도 : O(n log n)
- 공간 복잡도 : O(n)

### Binary Search(바이너리 서치, 이진 탐색 알고리즘)

* O(log n)
* 정렬된 자료를 반으로 나누어 탐색하는 방법
* 자료는 반드시 오름차순으로 정렬된 자료여야 한다.

### 캐시 알고리즘

## OSI 7계층

* 국제 표준화기구에서 개발한 모델이다.
* 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것이다.
* 상호 이질적인 네트워크간의 연결에 어려움이 많은데 이러한 호환성의 결여를 막기 위해 OSI 참조 모델을 만들었다.
* 실제 인터넷에서 사용되는 TCP/IP는 OSI 참조 모델을 기반으로 상업적이고 실무적으로 이용될 수 있도록 단순화된 현실화의 과정에서 채택된 모형이다.
* 표준과 학습 도구가 가장 중요한 목적이다.

### 1계층 물리 계층(Physical Layer)

* 실제 장치들을 연결하기 위해 필요한 전기적, 물리적 세부사항들을 정의한다.
* 믈리적인 정보 전달 매개체에 대한 연결의 성립 및 종료
* 네트워크상에서 데이터 비트를 전송하는 계층이다.
* 데이터 링크 개체간의 비트 전송을 위한 물리적 연결을 설정, 유지, 해제하기 위한 수단을 제공한다.
* 장비 : 허브, 리피터
* 데이터 단위 : 비트(Bit)

### 2계층 데이터 링크 계층(Data Link Layer)

* Point to Point(포인트 투 포인트)간 신뢰성있는 전송을 보장하기 위한 계층이다.
* CRC 기반의 오류 제어와 흐름 제어가 필요하다.
* 오류없이 한 장치에서 다른 장치로 프레임을 전달하는 역할을 한다.
* 3계층에서 정보를 받아 주소와 제어정보를 헤더와 끝에 추가한다.
* 장비 : 브릿지, 스위치
* 프로토콜 : MAC, PPP
* 데이터 단위 : 프레임(Frames)

### 3계층 네트워크 계층(Network Layer)

* 여러개의 노드를 거칠때마다 경로를 찾아주는 역할을 하는 계층이다.
* 라우팅, 패킷 포워딩, 세그멘테이션 등을 수행한다.
* 다중 네트워크 링크에서 패킷을 발신지로부터 목적지로 전달할 책임을 갖는다.
* 장비 : 라우터
* 프로토콜 : IP, ICMP, IGMP, ARP
* 데이터 단위 : 패킷(Packets)

### 4계층 전송 계층(Transport Layer)

* End to End(종단 대 종단)의 사용자들이 신뢰성있는 데이터를 주고 받을 수 있도록 해 주는 계층이다.
* 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해준다.
* 발신지 대 목적지간 제어와 에러를 관리한다.
* 패킷들의 전송이 유효한지 확인하고 실패한 패킷은 다시 보내는 등 신뢰성있는 통신을 보장한다.
* 시퀀스 넘버 기반의 오류 제어 방식을 사용한다.
* 머릿말에는 세그먼트가 포함된다.
* 장비 : 게이트웨이
* 프로토콜 : TCP, UDP, ARP
* 데이터 단위 : 세그먼트(Segments)

### 5계층 세션 계층(Session Layer)

* 통신 세션을 구성하는 계층이다.
* 양 끝단의 응용 프로세스가 통신을 관리하기 위한 방법을 제공한다.
* 이 계층은 TCP/IP 세션을 만들고 없애는 책임을 진다.
* 포트 연결
* 통신 장치간의 상호작용을 설정하고 유지하며 동기화한다.
* 사용자간의 포트연결(세션)이 유효한지 확인하고 설정한다.
* 프로토콜 : SSH, TLS
* 데이터 단위 : 데이터(Data)

### 6계층 표현 계층(Presentation Layer)

* 운영체계의 한 부분으로 입력, 출력되는 데이터를 하나의 표현 형태로 변환한다.
* 필요한 번역을 수행하여 두 장치가 일관되게 전송 데이터를 서로 이해할 수 있도록 한다.
* 압축
* 프로토콜 : JPEG, MPEG, SMB, AFP
* 데이터 단위 : 데이터(Data)

### 7계층 응용 계층(Application Layer)

* 사용자가 네트워크에 접근할 수 있도록 해주는 계층이다.
* 사용자 인터페이스, 메일, 데이터베이스 관리 등 서비스를 제공한다.
* 프로토콜 : DHCP, DNS, FTP, HTTP
* 데이터 단위 : 데이터(Data)

## Network

### L4, L7 Switch

* 모두 서버들의 로드밸런싱을 위해 사용된다.
* 스위치로 들어온 패킷을 적절한 목적지(서버)로 전송해주는 것이다.
* L4는 IP, TCP/UDP 포트 정보를 참조해서 스위칭 장비이다.
* L7은 IP, TCP/UDP 포트 정보 및 패킷 내용까지 참조해서 스위칭해주는 장비이다.
* L4는 TCP/UDP 해더 정보를 분석해서 해당 패킷이 사용하는 서비스 종류별로 처리한다.(HTTP, FTP, SMTP)
* L7은 L4 스위치의 서비스 단위 로드밸런싱을 극복하기 위해 포트 + Payload 패턴을 이용해 패킷 스위칭을 한다.
* L4는 Payload(뒤 데이터)에 대해서는 전혀 신경쓰지 않지만 L7은 세션을 분리하고 자신이 철저하게 클라이언트/서버로 동작한다.
* L7은 보안 스위치라고도 불리는데, 데이터 분석을 통해 DDoS 공격 방어, 감염 패킷 필터링 등의 기능을 제공한다.
* L7은 URL 별로 스위칭이 가능하고, Http Header 값에 따른 특정 String을 기준으로 부하 분산이 가능하다.

### TCP

* 연결 지향형 프로토콜
* 데이터의 경계가 없다.
* 전화(받았는지 못 받았는지 확인이 가능하다.)
* UDP에 비해 전송 속도가 느리다.
* 서버 소켓과 클라이언트 소켓의 구분이 있다.
* 전송 순서대로 데이터가 송신되고 수신된다.
* 중간에 데이터가 소멸되지 않는다.
* 1대1 연결구조이다.

### 3 Way HandShake

* TCP 통신을 하기 위한 연결 방식이다.
* 최초에 서버에서 열려 있는 포트는 LISTEN, 클라이언트에서는 CLOSED 상태다.

1. 클라이언트에서 서버에 연결 요청을 하기 위해 연결하고자 하는 서버의 포트로 SYN을 보낸다.
2. 서버에서는 해당 포트는 LISTEN 상태에서 SYN을 받고 SYN_RCV 상태가 된다.
3. 클라이언트에게 요청을 정상적으로 받았다는 대답(ACK)과 클라이언트에게 포트를 열어달라는 SYN을 보낸다.
4. 클라이언트에서는 SYN+ACK를 받고 ESTABLISHED로 상태를 변경하고 서버에 요청을 잘 받았다는 ACK를 전송한다.
5. ACK를 받은 서버는 상태가 ESTABLISHED로 변경된다.

### 4 Way HandShake

* TCP 통신을 종료하기 위한 방식이다.
* 정상적인 종료 상황
* 최초에 서로 통신 상태이기 때무에 서버, 클라이언트 모두 ESTABLISHED 상태

1. 통신을 종료하고자 하는 클라이언트가 서버에게 FIN을 보내고, 자신은 FIN_WAIT_1 상태로 대기한다.

2. FIN을 받은 서버는 해당 포트를 CLOSE_WAIT로 바꾸고 잘 받았다는 ACK를 클라이언트에게 보내고, 그와 동시에 서버에서는 해당 포트에 연결되어있는 애플리케이션에 Close()를 요청한다.

3. ACK를 받은 클라이언트는 FIN_WAIT_2 상태로 변경하다.

4. Close() 요청을 받은 애플리케이션은 종료 프로세스를 진행시켜 최종적으로 close()가 되고 서버는 FIN을 클라이언트에게 전송 후 자신은 LAST_ACK 상태가 된다.

5. FIN_WAIT_2에서 서버가 연결을 종료했다는 신호를 기다리다가 FIN을 받으면 잘 받앗다고 ACK를 서버에게 전송하고 클라이언트는 TIME_WAIT로 상태를 바꾼다.

   TIME_WAIT 상태는 일정 시간이 지나면 CLOSED로 변경된다.

   최종 ACK를 받은 서버는 자신의 포트도 CLOSED로 닫게된다.

* 비 정상적인 종료 상황

* CLOSE_WAIT : 애플리케이션에서 Close()를 처리하지 못하면 서버 포트는 CLOSE_WAIT 상태로 계속 대기하게 된다. CLOSE_WAIT 상태가 statement에 많아지게 되면, Hang이 걸려 더 이상 연결을 하지 못하게 된다.

* FIN_WAIT_1 : 서버에 종료 요청을 했는데 ACK를 받지 못한 상태로 기다리고 있다. 서버를 찾을 수 없거나, 네트워크 방화벽의 문제일 수 있다. 일정 시간이 지나 TIME OUT이 되면 자동으로 닫힌다.

* FIN_WAIT_2 : 클라이언트가 서버에 종료를 요청한 후 서버에서 요청을 접수했다고 ACK를 받았지만, 서버에서 종료했다는 FIN 상태를 받지 못하고 기다리는 상태다.

  양방의 두번의 통신이 이미 이루어졌기 때문에 네트워크 문제는 아니라고 판단한다.

  서버에서 CLOSE를 처리하지 못하는 경우일 수도 있다.

  일정 시간이 지나 TIME OUT이 되면 스스로 CLOSE하게 된다.

* TIME OUT이 길어져서 많은 수의 소켓이 늘어만 난다면 메모리 부족 현상이 발생할 수 있다.

### UDP

* 비 연결 지향형 프로토콜
* 데이터의 경계가 있다.
* 우편(받았는지 못 받았는지 확인이 불가능하다.)
* TCP에 비해 전송 속도가 빠르다.
* 서버 소켓과 클라이언트 소켓의 구분이 없다. 1개만 있으면 된다.
* 전송 순서가 상관없다.
* 데이터 손실 및 파손의 우려가 있다.
* 한번에 보낼 수 있는 데이터의 크기가 제한된다.

### Load Balancing(로드밸런싱)

* 부하 분산을 위해서 가상(Virtual) IP를 통해 여러 서버에 접속하도록 분배하는 기능을 말한다.
* 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 여러 대의 서버가 분산처리하여 서버의 로드율 증가, 부하량, 속도 저하 등을 고려해 해결하는 서비스다.
* 동시에 오는 수 많은 커넥션을 처리하고 해당 커넥션이 요청 노드 중의 하나로 전달될 수 있게 하는 것이다.
* 단지 노드를 추가하는 것만으로 서비스가 확장성을 가질 수 있도록 한다.
* 소프트웨어 로드밸런서 : HAProxy
* 하나의 서비스를 여러 노드가 처리

### Clustering(클러스터링)

* 여러 개의 컴퓨터를 연결한 병렬 시스템으로 마치 하나의 컴퓨터처럼 사용하는 것이다.
* 특정 장비에 문제가 생기거나 특정 장비에서 실행중인 애플리케이션에 문제가 발생하더라도 전체 서비스에 영향을 미치지 않도록 제어가 가능하다.
* Virtual IP(가상 IP)를 기반으로 구현된다.
* 서비스를 제공하는 실제 장비는 물리적인 IP를 갖고, 데이터 처리는 Virtual IP를 통해 이루어진다.
* 내부 시스템은 철저하게 가려져 있다.
* 유연한 구성이 가능하다.
* 하나의 서비스를 여러 노드가 처리

### Gateway(게이트웨이)

* 컴퓨터 네트워크에서 서로 다른 통신만, 프로토콜을 사용하는 네트워크 간의 통신을 가능하게 하는 하드웨어, 소프트웨어다.
* 다른 네트워크로 들어가는 입구 역할을 하는 네트워크 포인트다.
* 종류가 다른 네트워크 간의 통로 역할을 하는 장치이다.
* 게이트웨이를 지날 때마다 트래픽도 증가한다.
* 서로 다른 네트워크 상의 프로토콜을 적절히 변환해주는 역할을 한다.

### SSL(Secure Socket Layer)

* 웹 서버와 브라우저 간에 암호화된 링크를 설정하기 위한 표준 보안 기술이다.
* 통신 내용이 노출, 변경되는 것을 방지한다.
* 클라이언트가 접속하려는 서버가 신뢰할 수 있는 서버인지 확인이 가능하다.
* SSL 통신에 사용할 공개키를 클라이언트에게 제공한다.
* 인증서의 내용은 CA의 공개키를 이용해서 암호화되어 웹 브라우저에게 제공된다.
  * 서비스 정보(인증서 발급자, CA의 디지털 서명, 서비스 도메인)
  * 서버측 공개키
* CA(Certificate Authority)
  * SSL 인증서를 기준으로 클라이언트가 접속한 서버가 클라이언트가 의도한 서버가 맞는지를 확인하는 일을 해주는 공인되 회사다.
  * CA는 브라우저가 리스트를 가지고 있고 이들의 공개키들을 가지고 있다.
* 공개키와 비밀키의 장점을 모아 안전하고 빠르게 암호화와 복호화를 할 수 있다.
  * 공개키로 암호화하면 비밀키를 가진 사람만 복호화할 수 있다는 장점을 살렸다.
  * 비밀키로 암호화하고 복호화하면 컴퓨터 자원을 적게 사용하고 빠르다는 장점을 살렸다.

### SSL HandShake

1. 클라이언트가 서버에게 클라이언트에서 생성한 랜덤 데이터를 서버에 제공한다.
2. 서버는 클라이언트에게 서버에서 생성한 랜덤 데이터와 CA에서 받은 인증서를 보낸다.
3. 클라이언트는 인증서를 발급한 CA가 자신이 가지고 있는 CA리스트에 있는지 확인한다.
4. 리스트에 있다면 클라이언트에 내장된 CA의 공개키를 이용해 인증서를 복호화한다.
4.1. 인증서를 복호화 할 수 있다는 것은 이 인증서가 CA의 비밀키에 의해 암호화 된 것을 의미한다. 즉 데이터를 제공한 사람의 신원을 보장하게 되는 것이다.
5. 1번과 2번 단계에서 얻은 클라이언트와 서버의 랜덤 데이터를 조합하여 비밀키를 작성한다.
6. 작성한 비밀키를 인증서를 복호화하여 얻은 서버의 공개키로 암호화한다.
6.1. 서버의 공개키로 암호화했기 때문에 이 키를 열어볼 수 있는 것은 비공개키를 가진 서버 뿐이다.
7. 클라이언트는 암호화한 비밀키를 서버에게 보낸다.
8. 서버는 자신이 가진 비공개키로 클라이언트가 준 비밀키를 복호화한다.
9. 이로서 클라이언트와 서버가 비밀키를 공유하게 되었기에 빠른 속도로 복호화와 암호화를 할 수 있게 되었다.

## OS

### 교착 상태(DeadLock, 데드락)

* 두 개 이상의 작업이 서로 상대방의 작업이 끝나기만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태를 가리킨다.
* 교착 상태의 조건
  1. 상호 배제(Mutual Exclusion) : 자원 자체를 동시에 쓸 수 없다.
  2. 점유 대기(Hold and Wait) : 자원을 붙잡은 상태에서 다른 자원을 기다리고 있다.
  3. 비 선점(No Preemption) : 다른 프로세스의 자원을 빼앗을 방법이 없다.
  4. 순환 대기(Circular wait) : 대기가 꼬리를 물고 사이클이 되었다.
* 교착 상태의 관리
  1. 예방 : 교착 상태의 조건 제거, 자원 낭비가 심하다.
     * 상호 배제 부정 : 여러 개의 프로세스가 공유 자원을 사용할 수 있도록 한다.
     * 점유 대기 부정 : 프로세스가 실행되기 전 필요한 모든 자원을 할당한다.
     * 비선점 부정 : 자원을 점유하고 있는 프로세스가 다른 자원을 요구할 때 점유하고 있는 자원을 반납하고, 요구한 자원을 사용하기 위해 기다리게 한다.
     * 순환 대기 부정 : 자원에 고유한 번호를 할당하고, 순서대로 자원을 요구하도록 한다.
  2. 회피 : 교착상태가 발생하면 피해나가는 방법이다. 은행원 알고리즘
     * 프로세스가 자원을 요구할 때 시스템은 자원을 할당한 후에도 안정 상태로 남아있게 되는지를 사전에 검사하여 교착 상태를 회피하는 기법이다.
     * 안정 상태에 있으면 자원을 할당하고, 그렇지 않으면 다른 프로세스들이 자원을 해지할 때 까지 대기한다.
  3. 발견 : 자원 할당 그래프를 통해 교착 상태를 탐지할 수 있다.
     * 자원을 요청할 때 마다 탐지 알고리즘을 실행하면 오버헤드가 발생한다.
  4. 회복 : 교착 상태를 일으킨 프로세스를 종료하거나, 할당된 자원을 해제함으로써 회복하는 것을 의미한다.
     * 프로세스를 종료하는 방법
       * 교착 상태의 프로세스를 모두 중지
       * 교착 상태가 제거될 때 까지 한 프로세스씩 중지
     * 자원을 선점하는 방법
       * 교착 상태의 프로세스가 점유하고 있는 자원을 선점하여 다른 프로세스에게 할당하며, 해당 프로세스를 일시 정지 시키는 방법
       * 우선 순위가 낮은 프로세스, 수행된 횟수가 적은 프로세스 등을 위주로 프로세스의 자원을 선점한다.

### Mutex(뮤텍스)

* 상화 배제(MUTual EXclustion)의 약자이다.
* 임계 영역에 무조건 접근 못하게 lock을 건다.
* 하나만 접속이 가능하고 끝나면 unlock된다.
* 스레드의 순서를 보장하지 않는다.
* lock : 현재의 임계 구역에 들어갈 권한을 얻어온다. 만일 다른 프로세스/스레드가 임계 구역을 수행 중이라면 종료할 때 까지 대기한다.(entry section)
* unlock : 현재의 임계 구역을 모두 사용했음을 알린다. 대기중인 다른 프로세스/스레드가 임계 구역에 진입할 수 있다.(exit section)
* 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없다.
* 동기화 대상이 오직 하나일 때 사용한다.

### Semaphore(세마포어)

* 스레드의 실행 순서를 지정할 수 있다.
* 스레드의 순서가 보장 가능하다.
* 카운트 값이 0이면 진입 불가, 0보다 크면 진입이 가능하다.
* 세마포어는 동시에 여러 개의 프로세스/스레드가 임계 구역에 접근할 수 있도록 카운트를 가지고 있는데 카운트가 1인 세마포어가 뮤텍스이다.
* 리소스의 상태를 나타내는 간단한 카운터
* 동기화 대상이 하나 이상일 때 사용한다.
* 세마포어는 뮤텍스가 될 수 있지만, 뮤텍스는 세마포어가 될 수 없다.

### 임계 영역(Cirtical Section)

* 서로 다른 두 프로세스, 혹은 스레드 등의 처리 단위가 같이 접근해서는 안 되는 공유 영역을 말한다.
* 보호되지 않는 임계 구역에 두 처리 단위가 동시에 접근할 때 발생하는 문제를 임계 구역 문제라고 한다.
* 임계 구역을 시작하는 코드 부분을 입장 구역(entry section), 임계 영역을 종료하는 코드 부분을 퇴장 구역(exit section)이라고 한다.
* 임계 영역이 아닌 나머지 구역(remainder section)이라고 한다.

### Paging(페이징) 기법

* 가상 메모리 사용, 외부 단편화 해결, 내부 단편화 존재
* 보조 기억 장치를 이용한 가상 메모리를 같은 크기의 블록으로 나눈 것을 페이지라고 정의한다.
* RAM을 페이지와 같은 크리고 나눈 것을 프레임이라고 정의한다.
* 사용하지 않는 프레임을 페이지에 옮기고, 필요한 메모리를 페이지 단위로 프레임에 옮기는 기법이다.
* 페이지와 프레임을 대응시키기 위해 page mapping 과정이 필요해서 paging table을 만든다.
* 페이징 기법을 사용하면 연속적이지 않은 공간도 활용할 수 있기 때문에 외부 단편화 문제를 해결할 수 있다.
* 페이지 단위에 알맞게 꽉 채워 쓰는 것이 아니므로 내부 단편화 문제는 여전히 존재한다.
* 페이지 단위를 작게 하면 내부 단편화 문제도 해결할 수 있겠지만 대신 page mapping 과정이 많아지므로 오히려 효율이 떨어질 수 있다.
* 페이징은 일정한 크기로 분할해서 메모리를 관리한다.
* 잘려있는 식빵 조각

### Segmentation(세그먼트) 기법

* 가상 메모리 사용, 내부 단편화 해결, 외부 단편화 존재
* 가상 메모리를 서로 크기가 다른 논리적 단위인 세그먼트로 분할하여 메모리를 할당하여 실제 메모리 주소로 변환하게 된다.
* 각 세그먼트는 연속적인 공간에 저장되어 있다.
* 세그먼트들의 크기가 다르기 때문에 미리 분할해 둘 수 없고, 메모리에 적재될 때 빈 공간을 찾아 할당하는 기법이다.
* Mapping을 위해 세그먼트 테이블이 필요하다.
* 프로세스가 필요한 메모리 만큼 할당해주기 때문에 내부 단편화는 일어나지 않으나 여전히 프로세스가 메모리를 해제할 때 발생하는 외부 단편화 문제는 여전히 존재한다.
* 세그멘테이션은 우리가 필요한 만큼 분할해서 메모리를 관리한다.
* 케잌

### 스케쥴링

* 사용중인 프로세스에서 자원을 빼앗을 수 잇는지의 여부에 따라서 선점, 비선점 스케줄링
* 선점(Preemptive)
* 

### 경쟁상태(Race Condition)

* 공유 자원에 대해 여러 개의 프로세스가 동시에 접근을 시도할 때 접근의 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태를 말한다.
* 해결 방법 : 세마포어

### fork

* fork 함수가 호출되면, 호출한 프로세스가 복사된다.
* fork 함수 호출 이후를 각각의 프로세스가 독립적으로 실행하게 된다.
* fork 함수 호출 이후의 반환 값
  * 부모 프로세스 : 자식 프로세스 ID
  * 자식 프로세스 : 0
* 보통 자식 프로세스가 먼저 실행된다.
* 부모 프로세스는 자식 프로세스의 반환값 요청 하지도 않고, 받지도 않는다.(sleep)
* 부모 프로세스가 종료되어야 자식 프로세스가 종료된다.

### Process(프로세스)

* 프로세스는 서로 완전히 독립적이다.
* 프로세스는 운영체제 관점에서의 실행 흐름을 구성한다.
* 저장소에 존재하는 프로그래밍 컴퓨터가 실행해서 CPU가 처리할 수 있게 메인 메모리로 올라온 상태이다.

### Thread(쓰레드)

* 프로세스보다 가벼운, 경량화된 프로세스이다.
* 문맥 교환이 빠르다.
* 스레드 별로 메모리 공유가 가능하다.
* 프로세스 내에서의 프로그램 흐름을 추가한다.
* 스레드는 프로세스 내에서의 실행 흐름을 가진다.
* 코드, 데이터, 힙 영역을 공유한다.
* 문맥 교환에 대한 부담이 덜하다.
* 공유하는 메모리 영역으로 인해서 스레드간 데이터 교환이 매우 쉽게 이루어진다.
* 데이터 영역 : 전역 변수 저장
* 힙 영역 : 동적 메모리 할당
* 스택 영역 : 각 함수의 지역 변수, 파라미터
* 스택 영역은 각 스레드마다 가진다.

### 문맥 교환(Context Switch)

* 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로스세의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업을 말한다.
* 한 프로세스의 문맥은 그 프로세스의 프로세스 제어 블록(PCB)에 기록 되어 있다.

## etc.

### 대칭키 암호화

- 암호화와 복호화에 같은 키를 사용한다.
- 암호화와 복호화가 빠르지만 다른 사람에게 키를 안전하게 전달할 방법이 없다.
- AES

### 공개키 암호화(비 대킹키 암호화)

* 암호화와 복호화에 다른 키를 사용한다.
* 대칰이 암호화에 비해 속도가 느리다.
* SSL/TLS, RSA
* 공개키 : 누구에게나 공개가 가능한 키다.
* 개인키(비공개키, 비밀키) : 자신만이 갖고 있는 키다.
* 공개키로 암호화를 하면 데이터 보안에 중점을 둔 것이다.
* 개인키로 암호화를 하면 인증 과정에 중점을 둔 것이다.

1. 공개키로 암호화를 하는 경우
   * 상대방의 공개키로 데이터를 암호화하고 데이터를 전달하면, 데이터를 받은 사람은 자신의 개인키로 데이터를 복호화한다.
   * A키로 암호화를 한다면 B키로 복호화가 가능하고, B키로 암호화를 한다면 A키로 복호화가 가능한 것이다.
2. 개인키로 암호화를 하는 경우
   * 개인키의 소유자가 개인키로 데이터를 암호화하고 공개키와 함께 전달한다.
   * 공개키와 데이터를 획득한 사람은 공개키를 이용해 복호화가 가능하다.
   * 데이터 보호의 목적보다는 공개키가 데이터 제공자의 신원을 보장해주기 때문이다.
   * 암호화된 데이터가 공개키로 복호화된다는 것은 공개키와 쌍을 이루는 개인키에 의해 암호화되었다는 것을 의미한다.
   * 데이터 제공자의 신원 확인이 보장된다는 것이다.
   * 전자서명의 원리

* **용량이 큰 정보는 대칭키로 암호화하고, 암호화에 사용된 대칭키는 공개키로 암호화하여 대상에게 전달하는 하이브리드 암호화 방법을 일반적으로 사용한다.**

### Git

* 소스코드를 효과적으로 관리하기 위해 개발된 분산형 버전 관리 시스템이다.
* fetch : 리모트 서버로부터 저장소 정보를 동기화한다.
* rebase : 커밋을 합친다.

### Git Flow

* Branch 관리 전략, Branching 기법
* 프로젝트를 진행하면서 발생하는 수 많은 Branch를 쉽게 다룰 수 있도록 해 주는 규칙, 전략이다.
* 기본 전략이기 때문에 커스터마이징 해서 사용하면 된다.
* feature branch를 이용해 기능 개발의 책임 소개를 명확히 한다.
* 개발 버전과 제품 버전을 개별 관리 할 수 있다.
* Pull Request를 이용하기 때문에, 이를 이용해 코드 리뷰를 쉽게 할 수 있다.
* feature branch와 hotfix의 commit message를 취합하게 되면, 이전 버전과의 변경 점을 쉽게 파악 할 수 있다.

### IO

* Input / Output
* 입출력 방식 : Stream
* 데이터 형식 : byte
* 동기
* Blocking
* 단방향 통신
* 접속하는 클라이언트 수가 적을때, 순차적, 대용량일때 적합

### NIO

* Java NIO는 Non-Blocking I/O가 아니다.
* New Input / Output이다.
* File 관련 NIO는 모두 Blocking I/O
* NIO2의 AsynchronousFileChannel은 Non-Blocking I/O
* 입출력 방식 : Channel
* 데이터 형식 : buffer
* 동기, 비동기
* Non-Blocking / Blocking
* 양방향 통신
* 접속하는 클라이언트 수가 많고, 하나의 I/O가 오래 걸리지 않을 때 적합

### Blocking

* System Call이 끝날 때 까지 프로그램은 대기해야 하고 System Call이 완료되면 그때야 Return 한다.
* Wait Queue에 들어간다.
* 스레드가 작업이 종료될 때 까지 대기한다.
* 호출된 함수가 자신의 작업을 모두 마칠 때 까지 호출한 함수에게 제어권을 넘겨주지 않고 대기하게 만든다

### NonBlocking

* System Call이 완료되지 않아도 대기하지 않고 Return 해버린다.
* Wait Queue에 들어가지 않는다.
* 스레드가 작업이 종료될 때 까지 기다리지 않는다.
* 호출된 함수가 바로 리턴해서 호출한 함수에게 제어권을 넘겨주고 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있다.

### Blocking & Non-Blocking

* 함수 호출 시 제어권 리턴 유무
* Non-Blocking은 제어문 수준에서 지체없이 반환하는 것
* 호출하는 입장에서의 특징

### Synchronous(동기)

* 호출되는 함수에게 Callback을 전달해서 호출되는 함수의 작업이 완료되면 호출되는 함수가 전달받은 callback을 실행하고 호출하는 함수는 작업 완료 여부를 신경쓰지 않는다.
* 호출되는 함수의 작업 완료를 호출한 함수가 신경쓴다.
* System Call이 끝날때까지 기다리고 결과물을 가져온다.

### Asynchronous(비동기)

* 호출하는 함수가 호출되는 함수의 작업 완료후 리턴을 기다리거나, 호출회는 함수로부터 바로 리턴 받더라도 작업 완료 여부를 호출하는 함수 스스로 계속 확인하며 신경쓴다.
* 호출되는 함수의 작업 완료를 호출한 함수가 신경쓰지 않는다.
* System Call이 완료되지 않아도 나중에 완료되면 그때 결과물을 가져온다.
* 주로 Callback 함수를 통해 결과물을 가져온다.
* 별도의 스레드로 빼서 실행하고, 완료되면 호출하는 측에 알려주는 것이다.

### Synchronous & Asynchronous

* 함수 호출 시 작업 완료 여부 신경 유무
* 처리되는 방식의 특징

### Node

* Non-Blocking I/O와 단일 스레드 이벤트 루프를 통해 높은 성능을 가진다.
* 내장 HTTP 서버 라이브러리를 포함하고 있다.
* 웹 서버에서 아파치 등 별도의 소프트웨어 없이 동작하는 것이 가능하다.
* V8 엔진으로 빌드된 JavaScript 런타임이다.

### V8

### ECMA 6

### Docker

* 컨테이너 기반의 오픈소스 가상화 플랫폼이다.
* 다양한 프로그램, 실행 환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해 준다.
* 이미지는 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있다.
* 레이어라는 개념을 사용한다.

### CI(Continuous Integration)

* 대표적으로 젠킨스(Jenkins)
* 지속적인 통한
* 프로젝트 빌드, 테스트 실행, 배포 등의 통합을 자동화한다.

### 함수형 프로그래밍

* map과 reduce는 함수형 프로그래밍의 도구일 뿐이지 함수형 프로그래밍이 아니다.
* 상태를 바꾸지 않고 입력 값에 따라 동일한 출력 값이 나오는 함수의 응용을 통해 프로그래밍을 하여 사이드 이펙트를 최소화 시키는 것이 함수형 프로그래밍이다.

### 반응형 프로그래밍

### JWT(Json Web Token)

* 자가 수용적이다.
* 필요한 모든 정보를 자체적으로 지니고 있다.
* 쉽게 전달 될 수 있다.
* Header(헤더), Payload(내용), Signature(서명)로 구성된다.

### Scale-out

* 수평 방향으로 확장
* 노드를 추가하는 방식으로 성능 업그레이드

### Scale-up

* 수직 방향으로 확장
* 장비의 성능을 높이는 방식으로 성능 업그레이드
* 프로세서를 추가하는 것이나 프로세서 그 자체를 고성능 모델로 옮기는 것이다.

### Server Side Rendering(서버 사이드 렌더링)

* 요청시마다 새로고침이 일어나며 서버에 새로운 페이지에 대한 요청을 하는 방식이다.
* View를 서버에서 렌더링해서 가져오기 때문에 첫 로딩이 매우 짧다.

### Client Side Rendering(클라이언트 사이드 렌더링)

* 서버는 단지 JSON만 보내주는 역할을 하며 HTML을 그리는 역할은 클라이언트 측에서 자바스크립트가 수행하였다.
* 서버에서 View를 렌더하지 않고 각종 리소스를 다운 받은 후 브라우저에서 렌더링을 하기 때문에 초기 View 로딩 속도가 오래 걸린다.
* 사용자의 행동에 따라 필요한 부분만 다시 읽어들이기 때문에 서버 측에서 렌더링하여 전체 페이지를 다시 읽어들이는 것보다 빠른 인터렉션이 가능하다.

### Single Page Application(SPA)

* 브라우저에 로드되고 난 뒤에 페이지 전체를 서버에 요청하는 것이 아니라 최초 한번 페이지 전체를 로딩한 이후부터는 데이터만 변경하여 사용할 수 있는 웹 애플리케이션이다.

### Overhead(오버헤드)

* 어던 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리 등을 말한다.

### AngularJS 1

* SPA 방식의 프론트엔드 웹 개발을 위한 프레임워크다.
* 코드의 유지 보수, 분리가 용이하다.
* 페이지간 전환 속도가 매우 빠르다.
* 초기 로딩 속도가 느리다.
* 검색엔진에 인덱싱이 제대로 되지 않는다.
