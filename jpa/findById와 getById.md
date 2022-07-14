# findById와 getById

Spring Data JPA는 ID를 활용하여 엔티티를 조회할 수 있는 `findById`와 `getById` 메서드를 제공한다.

`findById`는 ID를 활용하여 데이터 저장소에서 **실제 객체를 반환** 받는 메서드이고 
`getById`는 실제 객체를 반환하는게 아니라 **프록시를 반환**하는 메서드이다.
(기존 `getOne` 메서드가 사용되었으나 Deprecated 되고 `getById`로 대체)

```java
@Repository
@Transactional(readOnly = true)
public class SimpleJpaRepository<T, ID> implements JpaRepositoryImplementation<T, ID> {
    @Override
    public Optional<T> findById(ID id) {

        Assert.notNull(id, ID_MUST_NOT_BE_NULL);

        Class<T> domainType = getDomainClass();

        if (metadata == null) {
            return Optional.ofNullable(em.find(domainType, id));
        }

        LockModeType type = metadata.getLockModeType();

        Map<String, Object> hints = new HashMap<>();
        getQueryHints().withFetchGraphs(em).forEach(hints::put);

        return Optional.ofNullable(type == null ? em.find(domainType, id, hints) : em.find(domainType, id, type, hints));
    }

    @Override
    public T getById(ID id) {

        Assert.notNull(id, ID_MUST_NOT_BE_NULL);
        return em.getReference(getDomainClass(), id);
    }
}
```
위 코드는 SimpleJpaRepository 클래스에 정의 된 `findById`와 `getById` 메서드 이다. 
코드를 확인해보면 `findById`의 경우 `em.find`를 사용하여 실제 객체를 조회하고 `getById`는 `em.getReference`를 사용하여 프록시를 조회하는 것을 알 수 있다. 

# 연관 관계를 가진 엔티티 생성

Member와 Team 두개의 엔티티가 있다고 가정하자. 신규로 Member를 생성 하기 위해서는 Team을 조회해야 한다. 이때 `findById`와 `getById`는 어떤 차이가 발생될까?

```java
@Getter
@Entity
@Table(name = "members")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "member_id")
    private Long id;

    @Column(length = 50, nullable = false)
    private String memberNm;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "team_id")
    private Team team;

    // 생성로직
    public static Member newMember(String memberNm, Team team) {
        Member member = new Member();
        member.memberNm = memberNm;
        member.team = team;
        return member;
    }
}
```

```java
@Getter
@Entity
@Table(name = "teams")
@NoArgsConstructor(access = AccessLevel.PROTECTED)
public class Team {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "team_id")
    private Long id;

    @Column(length = 50, nullable = false)
    private String teamNm;
}
```

## findById
```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class MemberService {

    private final MemberRepository memberRepository;
    private final TeamRepository teamRepository;

    @Transactional
    public Long save(String memberNm, Long teamId) {
        Team team = teamRepository.findById(teamId)
                .orElseThrow(EntityNotFoundException::new);

        Member member = Member.newMember(memberNm, team);
        memberRepository.save(member);

        return member.getId();
    }
}
```

<img width="710" alt="화면 캡처 2022-07-14 225233" src="https://user-images.githubusercontent.com/55070039/178998936-aef2981a-a58d-4455-b8a2-e1acc814c868.png">

로그를 확인해보면 select 이후에 insert가 된것을 알 수 있다.

## getById
```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class MemberService {

    private final MemberRepository memberRepository;
    private final TeamRepository teamRepository;

    @Transactional
    public Long save(String memberNm, Long teamId) {
        Team team = teamRepository.getById(teamId);

        Member member = Member.newMember(memberNm, team);
        memberRepository.save(member);

        return member.getId();
    }
}
```

<img width="464" alt="화면 캡처 2022-07-14 225744" src="https://user-images.githubusercontent.com/55070039/178999835-540dad94-ff9d-401a-848c-a2c22e9322e0.png">

getById의 경우 insert 쿼리만 발생된 것을 알 수 있다. 프록시를 조회할 경우 ID 외 다른 컬럼을 사용할 때 조회 쿼리가 발생된다.
