# EntityGraph in JPA

**EntityGraph**는 JPA(Java Persistence API)에서 N+1 문제를 방지하고 연관된 엔티티를 효율적으로 페치(Fetch)하기 위해 사용하는 기능입니다. 이를 통해 JPQL이나 메서드 쿼리 없이 명시적으로 페치 전략을 정의할 수 있습니다.

---

## 주요 기능

1. **N+1 문제 해결**
   - 기본적으로 JPA는 연관된 엔티티를 지연 로딩(Lazy Loading)합니다. 이로 인해 N+1 문제가 발생할 수 있는데, `EntityGraph`를 사용하면 이를 효과적으로 방지할 수 있습니다.

2. **명시적 페치 전략 지정**
   - EntityGraph를 사용하면 특정 연관 엔티티를 즉시 로딩(Eager Loading)으로 설정하여 필요할 때만 데이터를 가져오도록 제어할 수 있습니다.

3. **코드 가독성 향상**
   - JPQL을 사용하지 않고도 간결하게 페치 전략을 정의할 수 있습니다.

---

## EntityGraph 선언 방법

### 1. @EntityGraph 어노테이션
JPA 엔티티나 레포지토리 메서드에서 사용됩니다.

#### 엔티티에 정의
```java
@Entity
@NamedEntityGraph(
    name = "User.withRoles",
    attributeNodes = @NamedAttributeNode("roles")
)
public class User {
    @Id
    private Long id;

    private String name;

    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Role> roles;
}
```

#### 레포지토리 메서드에서 사용
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @EntityGraph(value = "User.withRoles", type = EntityGraph.EntityGraphType.LOAD)
    List<User> findAll();
}
```

### 2. 동적 EntityGraph 정의
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Override
    @EntityGraph(attributePaths = {"roles"})
    List<User> findAll();
}
```

---

## EntityGraph 타입

1. **FETCH**
   - 현재 엔티티의 페치 전략을 무시하고 명시된 속성을 즉시 로딩(Eager Loading)으로 설정.

2. **LOAD**
   - 현재 엔티티의 페치 전략을 따르며, 명시된 속성만 추가적으로 즉시 로딩.

---

## 사용 예제

### N+1 문제 해결 전
```java
List<User> users = userRepository.findAll();
for (User user : users) {
    System.out.println(user.getRoles()); // N+1 문제 발생
}
```

### N+1 문제 해결 후 (EntityGraph 사용)
```java
List<User> users = userRepository.findAll(); // roles도 함께 로드됨
for (User user : users) {
    System.out.println(user.getRoles()); // 추가 쿼리 없음
}
```

---

## 장점

1. **N+1 문제 해결**
   - 연관 엔티티를 한 번의 쿼리로 페치하여 성능 문제를 완화.

2. **명시적 제어**
   - JPQL 없이 페치 전략을 선언적으로 설정 가능.

3. **유연성**
   - 동적 및 정적 페치 전략 모두 지원.

---

## 주의사항

1. **과도한 즉시 로딩 주의**
   - 즉시 로딩을 잘못 사용하면 대량의 데이터를 한 번에 로드하여 메모리 사용량이 증가할 수 있습니다.

2. **복잡한 그래프 관리**
   - 복잡한 엔티티 그래프를 정의할 때 가독성이 떨어질 수 있습니다.

3. **EntityGraph의 한계**
   - 복잡한 조건을 포함한 페치 전략은 JPQL이나 QueryDSL로 구현해야 할 수도 있습니다.

---

**EntityGraph**는 JPA 연관 엔티티 로딩의 성능 문제를 해결하고 코드의 가독성을 높이는 데 매우 유용한 도구입니다. 올바르게 사용하면 효율적인 데이터베이스 접근과 성능 최적화를 동시에 달성할 수 있습니다.
