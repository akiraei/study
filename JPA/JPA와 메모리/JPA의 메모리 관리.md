
JPA는 영속성 컨텍스트, 1차 캐시, 2차 캐시 등을 통해 데이터를 관리하며, 이 과정에서 메모리를 활용합니다. 하지만 대량 데이터 처리나 비효율적인 설계로 인해 메모리 사용량이 증가할 수 있습니다. 이를 방지하고 성능을 최적화하기 위해 다음과 같은 메모리 관리 방안을 적용할 수 있습니다.

## **요약**

1. **1차 캐시 관리**:
    - 지연 로딩(Lazy Loading)으로 불필요한 데이터 로드 방지.
    - JPQL과 DTO를 활용해 필요한 데이터만 조회.
    - 대량 작업 시 `flush()`와 `clear()`를 주기적으로 호출.
2. **2차 캐시 관리**:
    - 필요한 엔티티만 캐싱하고, 정적 데이터는 읽기 전용 캐시로 설정.
    - TTL(유효 기간)과 캐시 크기 제한으로 메모리 사용량 제어.
3. **DTO 활용**:
    - 엔티티 전체를 로드하지 않고 필요한 데이터만 전송하여 메모리와 성능 최적화.
4. **배치 처리와 트랜잭션 관리**:
    - 배치 크기를 적절히 설정하고, 트랜잭션 범위를 최소화하여 메모리 과부하를 방지.


## **1. 1차 캐시(영속성 컨텍스트) 관리**

### **1.1 지연 로딩(Lazy Loading) 활용**
- 필요할 때만 데이터를 로드하여 메모리 사용을 최적화.
- 대용량 데이터(`@Lob`) 또는 연관 엔티티는 기본적으로 `FetchType.LAZY`로 설정.
- **예제**:
```java
  @Entity
  public class Product {
      @Id
      private Long id;

      private String name;

      @Lob
      @Basic(fetch = FetchType.LAZY) // 지연 로딩 설정
      private String description;

      @OneToMany(mappedBy = "product", fetch = FetchType.LAZY) // 연관 엔티티 지연 로딩
      private List<Review> reviews;
  }
```


### **1.2 JPQL로 필요한 필드만 조회**

- 엔티티 전체를 로드하지 않고 필요한 필드만 선택적으로 조회.
- **예제**:
```java
String jpql = "SELECT new com.example.dto.ProductDTO(p.id, p.name) FROM Product p"; 

List<ProductDTO> products = entityManager.createQuery(jpql, ProductDTO.class).getResultList();
```

### **1.3 트랜잭션 범위 최소화**

- 트랜잭션이 길어지면 영속성 컨텍스트에 쌓이는 데이터가 증가하여 메모리 부담이 커짐.
- 짧은 트랜잭션 단위로 작업을 나누어 관리.

### **1.4 영속성 컨텍스트 주기적 초기화**

- 대량 데이터 작업 시, 영속성 컨텍스트를 초기화하지 않으면 메모리 누수가 발생할 수 있음.
- `flush()`와 `clear()`를 통해 영속성 컨텍스트를 주기적으로 비움.
- **예제**:
  ```java
  for (int i = 0; i < dataList.size(); i++) {     
  
  entityManager.persist(dataList.get(i));    
   
   if (i % 50 == 0) { 
   // 50개 처리 후 초기화         
   entityManager.flush();         
   entityManager.clear();    
   }}
```

### **1.5 대량 데이터 작업 시 배치 처리**

- 대량 데이터 작업에서 한 번에 많은 데이터를 처리하면 메모리 초과가 발생할 수 있음.
- Hibernate의 배치 크기를 설정하여 데이터를 나누어 처리.
- 설정 예제:
  ```properties
  spring.jpa.properties.hibernate.jdbc.batch_size=50
```

## **2. 2차 캐시(L2 캐시) 관리**

### **2.1 필요한 엔티티만 캐싱**
- 2차 캐시에 모든 엔티티를 저장하면 메모리 사용량이 급증.
- 캐싱이 필요한 엔티티만 `@Cacheable`로 설정.
- **예제**:
  ```java
  @Entity
  @Cacheable 
  public class Product {     
  
  @Id     
  private Long id;      
  private String name;      
  @Lob     
  private String description; 
  }
```

### **2.2 캐시 정책 설정**

- 캐싱 크기를 제한하거나 TTL(유효 기간)을 설정해 메모리 사용량을 제어.
- **Ehcache 설정 예제**:
  ```xml
  <cache alias="com.example.Product">     
  <key-type>java.lang.Long</key-type>     
  <value-type>com.example.Product</value-type>     
  <expiry>         
  <ttl unit="seconds">3600</ttl> <!-- 1시간 -->     
  </expiry>     
  <heap units="entries">1000</heap> <!-- 최대 1000개 엔트리 --> 
  </cache>
```

### **2.3 정적 데이터는 읽기 전용 캐시 사용**

- 자주 변경되지 않는 데이터를 읽기 전용 캐시로 설정해 추가적인 캐시 관리 비용을 줄임.
- **예제**:
  ```java
@Entity 
@Cacheable 
@org.hibernate.annotations.Cache(usage = CacheConcurrencyStrategy.READ_ONLY) public class Country {     

@Id     
private Long id;      
private String name; 
}
```

## **3. DTO(Data Transfer Object) 활용**

### **3.1 DTO로 필요한 데이터만 전달**

- 엔티티 전체를 로드하지 않고 필요한 데이터만 DTO로 조회하여 메모리 사용량을 절감.
- **예제**:
  ```java
  public class ProductDTO {     
  
  private Long id;     
  private String name;      
  
  public ProductDTO(Long id, String name) {         
  this.id = id;         
  this.name = name;     } 
  
  }
  
  String jpql = "SELECT new com.example.dto.ProductDTO(p.id, p.name) FROM Product p"; 
  
  List<ProductDTO> products = entityManager.createQuery(jpql, ProductDTO.class).getResultList();
```

## **4. 성능 분석 도구 활용**

- Hibernate의 통계 기능(`hibernate.generate_statistics=true`)을 사용해 캐시 적중률과 쿼리 실행 수를 분석.
- 메모리 사용량과 캐싱 정책을 지속적으로 모니터링하고 튜닝.