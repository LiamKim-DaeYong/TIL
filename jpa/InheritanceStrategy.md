# JPA 상속관계 매핑 전략

관계형 데이터베이스에서는 객체 지향의 상속이라는 개념이 존재하지 않는다. 대신 슈퍼타입과 서브타입 관계라는 
모델링 기법이 존재하는데 이러한 상속에 대한 패러다임 불일치를 해결하여 매핑해주는 것을 상속관계 매핑이라 한다.
JPA는 상속 관계 매핑을 3가지 방법으로 지원한다.

## 조인 전략
엔티티 각각을 테이블로 만들고 자식 테이블이 부모 테이블의 기본 키를 받아 기본 키 + 외래 키로 사용하는 전략

![images_cham_post_53e6d766-f2d8-4595-aeb7-ccd4cbdd9bb6_image](https://user-images.githubusercontent.com/55070039/179357568-61792e73-bbb9-4ef7-8ec5-2c8dcfb17c8a.png)

```java
@Entity
@Getter
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {
    @Id @GeneratedValue
    @Colum(name = "item_id")
    private Long id;
    
    private String name;
    
    private BigDecimal price;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Album")
@PrimaryKeyJoinColumn(name = "album_id")
public class Album extends Item {
    private String artist;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Movie")
@PrimaryKeyJoinColumn(name = "movie_id")
public class Movie extends Item {
    private String director;
    
    private String actor;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Book")
@PrimaryKeyJoinColumn(name = "book_id")
public class Book extends Item {
    private String author;
    
    private String isbn;
}
```

### 장점
 - 테이블의 정규화
 - 외래 키 참조 무결성 제약조건 활용 가능
 - 저장공간을 효율적으로 사용 가능

### 단점
 - 조회 시 잦은 조인으로 인해 성능 저하 가능성
 - 복잡한 조회 쿼리
 - 데이터 등록 시, 두번 실행되는 INSERT문

## 단일 테이블 전략
하나의 테이블을 사용하며 구분 컬럼을 이용해 데이터를 활용하는 전략

![images_cham_post_55a73a0e-dc74-41dd-bbe2-3dfe39f64414_image](https://user-images.githubusercontent.com/55070039/179358079-21043dd2-63b6-49c6-a3db-958c5b831aa6.png)

```java
@Entity
@Getter
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {
    @Id @GeneratedValue
    @Colum(name = "item_id")
    private Long id;
    
    private String name;
    
    private BigDecimal price;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Album")
public class Album extends Item {
    private String artist;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Movie")
public class Movie extends Item {
    private String director;
    
    private String actor;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Book")
public class Book extends Item {
    private String author;
    
    private String isbn;
}
```

### 장점
 - 조인이 사용되지 않아 빠른 조회 성능
 - 단순한 조회 쿼리

### 단점
 - 자식 엔티티가 매핑한 컬럼은 모두 NULL 허용
 - 테이블의 크기가 커져 오히려 조회 성능이 떨어질 수 있음

## 구현 클래스마다 테이블 전략
자식 엔티티마다 테이블을 생성하는 전략

![images_cham_post_4c1e96fb-3ca2-4353-87c4-1a2538b2f65a_image](https://user-images.githubusercontent.com/55070039/179358184-1defb6e2-dbb2-43ef-b375-08aaf712a51d.png)

```java
@Entity
@Getter
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {
    @Id @GeneratedValue
    @Colum(name = "item_id")
    private Long id;
    
    private String name;
    
    private BigDecimal price;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Album")
public class Album extends Item {
    private String artist;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Movie")
public class Movie extends Item {
    private String director;
    
    private String actor;
}
```

```java
@Entity
@Getter
@DiscriminatorValue("Book")
public class Book extends Item {
    private String author;
    
    private String isbn;
}
```

### 장점
 - 서브 타입을 구분하여 처리할 때 효과적
 - not null 제약조건 사용 가능

### 단점
 - 여러 자식 테이블을 함께 조회 시 성능 문제 (UNION 사용)
