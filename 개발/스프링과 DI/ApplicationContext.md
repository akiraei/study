# Spring ApplicationContext

**ApplicationContext**는 Spring Framework에서 핵심적인 IoC(Inversion of Control) 컨테이너로, 애플리케이션 구성 및 빈(Bean)의 생성, 관리, 의존성 주입 등을 담당합니다. 이는 BeanFactory의 확장판으로 더 많은 고급 기능을 제공합니다.

---

## 주요 특징

### 1. 빈 관리
- 빈(Bean)의 라이프사이클 관리 (생성, 초기화, 의존성 주입, 소멸 등).
- 싱글톤(Singleton) 스코프를 기본으로 하지만, 프로토타입(Prototype) 등 다양한 스코프 제공.

### 2. 국제화 지원
- **MessageSource**를 통해 다국어 메시지를 관리.
- 지역화된 메시지 번들을 로드하여 다국어 애플리케이션 지원 가능.

### 3. 이벤트 처리
- **ApplicationEvent**와 **ApplicationListener**를 사용하여 이벤트 기반 프로그래밍 가능.
- 애플리케이션 생명주기 이벤트 및 사용자 정의 이벤트 처리.

### 4. 환경 설정 관리
- **Environment** 객체를 통해 프로파일(Profile) 및 속성(Properties)을 관리.
- 외부 설정 파일 (`application.properties`, `application.yml`)과의 연동 지원.

### 5. AOP 및 트랜잭션 관리
- **AOP (Aspect-Oriented Programming)**와 선언적 트랜잭션 관리 기능 제공.
- 애플리케이션 내에서 횡단 관심사(로깅, 보안 등)를 효과적으로 처리.

---

## 주요 구현체

### 1. ClassPathXmlApplicationContext
- 클래스 경로에서 XML 파일을 로드하여 ApplicationContext를 초기화.
```java
ApplicationContext context = 
    new ClassPathXmlApplicationContext("applicationContext.xml");
MyBean myBean = context.getBean(MyBean.class);
```

### 2. FileSystemXmlApplicationContext
- 파일 시스템 경로에서 XML 파일을 로드하여 ApplicationContext를 초기화.

### 3. AnnotationConfigApplicationContext
- Java 기반 설정(@Configuration, @Bean)을 사용하여 ApplicationContext 초기화.
```java
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}

ApplicationContext context = 
    new AnnotationConfigApplicationContext(AppConfig.class);
MyBean myBean = context.getBean(MyBean.class);
```

### 4. WebApplicationContext
- Spring MVC와 통합된 웹 애플리케이션 전용 ApplicationContext.
- DispatcherServlet과 함께 사용되어 HTTP 요청을 처리.

---

## ApplicationContext와 BeanFactory의 차이

| **기능**            | **ApplicationContext** | **BeanFactory**        |
| ----------------- | ---------------------- | ---------------------- |
| **초기화 시점**        | 모든 빈을 즉시 초기화           | 필요한 시점에 초기화(Lazy Init) |
| **국제화 지원**        | 지원                     | 지원하지 않음                |
| **이벤트 처리**        | 지원                     | 지원하지 않음                |
| **AOP 및 트랜잭션 지원** | 지원                     | 지원하지 않음                |

---

## 사용 예제

### XML 기반 설정
```xml
<!-- applicationContext.xml -->
<beans>
    <bean id="myBean" class="com.example.MyBean" />
</beans>
```

```java
ApplicationContext context = 
    new ClassPathXmlApplicationContext("applicationContext.xml");
MyBean myBean = context.getBean(MyBean.class);
```

### Java 기반 설정
```java
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}

ApplicationContext context = 
    new AnnotationConfigApplicationContext(AppConfig.class);
MyBean myBean = context.getBean(MyBean.class);
```

---

**ApplicationContext**는 단순히 빈을 관리하는 역할을 넘어, 애플리케이션의 설정, 관리, 확장성 제공에서 중요한 역할을 합니다.
