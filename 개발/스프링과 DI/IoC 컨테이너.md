# IoC 컨테이너 (Inversion of Control Container)

**IoC 컨테이너**는 객체의 생성, 의존성 관리, 생명주기 제어를 통해 애플리케이션 구성 요소 간의 결합도를 줄이고, 유연하고 확장 가능한 설계를 가능하게 하는 Spring Framework의 핵심 개념입니다.

---

## IoC(Inversion of Control)란?
- 객체의 생성과 의존성 관리를 개발자가 아닌 **컨테이너**에 위임하는 설계 원칙.
- 객체는 자신이 사용할 의존성을 생성하거나 찾지 않고, 외부에서 주입받음.
- 의존성 주입(Dependency Injection, DI)을 통해 구현.

---

## IoC 컨테이너의 역할
1. **객체 생성 및 관리**
   - 애플리케이션에서 필요한 객체를 생성하고 그 생명주기를 관리.
   
2. **의존성 주입**
   - 객체 간의 의존성을 컨테이너가 주입함으로써 결합도를 낮춤.

3. **설정 관리**
   - XML, Java Config, 또는 어노테이션을 통해 구성 정보를 읽고 객체를 생성 및 설정.

4. **빈(Bean) 검색**
   - 생성된 객체(빈)를 애플리케이션에 제공.

---

## IoC 컨테이너의 주요 구현체

### 1. BeanFactory
- Spring의 IoC 컨테이너의 가장 기본적인 구현체.
- **Lazy Initialization**: 필요한 시점에 빈을 초기화.
- 상대적으로 가볍고 단순하지만, 고급 기능(이벤트 처리, AOP 등)은 부족.

### 2. ApplicationContext
- BeanFactory를 확장한 IoC 컨테이너.
- **Eager Initialization**: 애플리케이션 시작 시점에 모든 빈 초기화.
- 국제화(MessageSource), 이벤트 처리(ApplicationEvent), AOP 지원 등 고급 기능 제공.

---

## IoC 컨테이너 구성 방법

### 1. XML 기반 설정
```xml
<!-- applicationContext.xml -->
<beans>
    <bean id="myBean" class="com.example.MyBean" />
</beans>
```

### 2. Java Config 기반 설정
```java
@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```

### 3. 어노테이션 기반 설정
```java
@Component
public class MyBean {
    // Bean 정의
}
```

---

## IoC 컨테이너의 동작 과정
1. **구성 파일 읽기**
   - XML, Java Config, 어노테이션을 통해 애플리케이션 구성을 읽음.
   
2. **빈 정의**
   - 설정 정보에 따라 빈을 정의하고 IoC 컨테이너에 등록.
   
3. **빈 생성 및 의존성 주입**
   - 객체를 생성하고 필요한 의존성을 주입.

4. **빈 사용**
   - 컨테이너에서 관리하는 빈을 애플리케이션이 요청하여 사용.

5. **빈 소멸**
   - 애플리케이션 종료 시 빈의 생명주기를 관리하고 소멸 메서드 호출.

---

## IoC 컨테이너 사용 예제

### XML 설정과 BeanFactory 사용
```java
Resource resource = new ClassPathResource("applicationContext.xml");
BeanFactory factory = new XmlBeanFactory(resource);
MyBean myBean = factory.getBean(MyBean.class);
```

### Java Config와 ApplicationContext 사용
```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
MyBean myBean = context.getBean(MyBean.class);
```

---

## IoC 컨테이너의 이점
1. **결합도 감소**
   - 객체 생성과 의존성 관리를 외부에서 처리하므로 객체 간 결합도가 낮아짐.

2. **유연성 향상**
   - 객체 교체 및 변경이 용이.
   
3. **테스트 용이성**
   - 의존성을 주입받기 때문에 단위 테스트가 쉬워짐.
   
4. **재사용성 증가**
   - 컨테이너가 객체를 관리하므로 여러 컴포넌트 간 재사용이 가능.

---

Spring의 IoC 컨테이너는 애플리케이션 구조를 더 유연하고 모듈화할 수 있도록 도와줍니다. 이를 통해 개발자는 비즈니스 로직에 집중할 수 있습니다.
