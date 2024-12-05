# 스프링과 의존성 주입(Dependency Injection, DI)

## **스프링 IoC 컨테이너**
스프링의 **IoC(Inversion of Control) 컨테이너**는 객체(빈)의 생성, 관리, 의존성 주입, 그리고 라이프사이클 관리를 책임지는 스프링의 핵심 컴포넌트입니다. 스프링에서는 `ApplicationContext`와 `BeanFactory` 인터페이스를 통해 IoC 컨테이너가 구현됩니다.

### **IoC의 개념**
- **Inversion of Control**:
  - 객체가 필요한 의존성을 스스로 생성하지 않고, 외부에서 주입받는 설계 원칙.
  - 객체 간의 결합도를 줄이고 코드의 유연성과 재사용성을 높이는 데 기여.

---

## **ApplicationContext**
`ApplicationContext`는 스프링 IoC 컨테이너의 고급 구현체로, 대규모 애플리케이션 개발에 적합한 기능을 제공합니다.

### **1. 주요 기능**
- **빈 생성 및 관리**:
  - 빈을 생성하고, 의존성을 주입하며, 객체의 라이프사이클을 관리.
- **애플리케이션 이벤트 발행/구독**:
  - 이벤트 기반 프로그래밍을 지원.
  - 예: 사용자 가입 이벤트 → 이메일 전송 리스너.
- **메시지 소스(MessageSource)**:
  - 다국어 지원을 위한 국제화 메시지 처리.
- **환경 변수 및 프로파일 관리**:
  - 개발, 테스트, 프로덕션 환경에 따라 다른 설정을 적용.

### **2. 주요 메서드**
- `getBean(String name)`: 빈 이름으로 객체 검색.
- `getBean(Class<T> requiredType)`: 빈 타입으로 객체 검색.
- `publishEvent(Object event)`: 애플리케이션 이벤트 발행.
- `getEnvironment()`: 현재 환경(Environment) 객체 반환.

### **3. 주요 구현체**
- `AnnotationConfigApplicationContext`: Java Config 설정 클래스 기반.
- `ClassPathXmlApplicationContext`: XML 설정 파일 기반.
- `GenericWebApplicationContext`: 웹 애플리케이션 컨텍스트.

---

## **의존성 주입(DI)**

### **1. 의존성 주입의 개념**
의존성 주입은 객체가 필요한 의존성을 외부에서 주입받는 설계 방식으로, 객체 간 결합도를 낮추고 코드의 유연성과 테스트 용이성을 높이는 데 기여합니다.

### **2. DI 방식**
#### **2.1 필드 주입(Field Injection)**
- `@Autowired`를 사용하여 필드에 직접 주입.
```java
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
}
```

#### **2.2 생성자 주입(Constructor Injection)**
- 생성자를 통해 의존성을 주입.
```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

#### **2.3 세터 주입(Setter Injection)**
- 세터 메서드를 통해 의존성을 주입.
```java
@Service
public class UserService {
    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

---

## **DI 동작 원리**
1. **설정 로드**:
   - 스프링은 XML, Java Config, 또는 애노테이션 설정을 읽어들임.
2. **빈 정의**:
   - 빈의 메타데이터(이름, 타입, 의존성, 스코프 등)를 컨테이너에 등록.
3. **빈 생성 및 의존성 주입**:
   - 빈을 생성하고 필요한 의존성을 주입.
4. **라이프사이클 관리**:
   - 초기화 및 소멸 콜백(`@PostConstruct`, `@PreDestroy`)을 호출.

---

## **ApplicationContext와 BeanFactory의 차이**

| **특징**           | **ApplicationContext**             | **BeanFactory**               |
|---------------------|------------------------------------|--------------------------------|
| **초기화 시점**    | 애플리케이션 시작 시 모든 빈 초기화 | 빈 요청 시점에 지연 초기화     |
| **고급 기능**      | 이벤트 처리, 국제화, AOP 지원      | 없음                           |
| **사용 사례**      | 대규모 애플리케이션               | 경량 애플리케이션, 테스트용    |

---

## **DI의 장점과 한계**

### **1. 장점**
1. **결합도 감소**:
   - 의존성을 외부에서 관리하여 코드 간의 결합도를 낮춤.
2. **유연성과 확장성**:
   - 의존성 변경이 필요할 때 애플리케이션 코드 수정 없이 설정만 변경 가능.
3. **테스트 용이성**:
   - Mock 객체를 쉽게 주입하여 단위 테스트 가능.
4. **라이프사이클 관리**:
   - 객체 생성, 초기화, 소멸까지 자동으로 관리.

### **2. 한계**
1. **초기 설정 복잡성**:
   - 설정 파일 작성이나 설정 클래스 정의가 필요.
2. **런타임 의존성**:
   - 의존성이 런타임에 주입되므로 컴파일 타임 오류를 발견하기 어려움.
3. **잠재적 남용**:
   - DI 컨테이너의 기능을 남용하면 구조가 복잡해질 위험.

---

## **결론**
스프링의 IoC 컨테이너와 DI 메커니즘은 객체 간의 의존성을 관리하고, 코드의 결합도를 낮춰 유지보수성과 확장성을 극대화하는 강력한 도구입니다. `ApplicationContext`는 대규모 애플리케이션 개발에 적합하며, 의존성 주입을 통해 개발자는 비즈니스 로직에만 집중할 수 있습니다. 그러나 DI와 IoC 컨테이너의 강력함을 올바르게 활용하려면 설계 단계에서 신중하게 적용해야 합니다.
