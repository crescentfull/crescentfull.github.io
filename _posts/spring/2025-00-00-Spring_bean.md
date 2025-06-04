---
title: "Instance Method, Static Method, Class Method"
description: "파이썬 메서드 정리"
date: 2021-08-06 10:00:00 +0900
categories: [Language, Python]
tags: [Language, Python, Method]
image: https://luckydavekim.github.io/static/01d44fb486822261054e340f45003a56/8ff83/cover.webp
---

# Spring에서 객체를 Bean으로 관리하는 이유

bean에서 대해서 설명하기 전, java의 이름 유래부터 알고 가면 흥미로울 것이다.  
왜 빈이라고 작명했냐면 일단 자바라고 이름 지은 것도 자주 마시는 커피가 인도네시아 자바 섬 커피였기 때문이라고 한다.
(그래서 자바의 브랜드 이미지가 커피잔이 있는 것이다!)  
이 때문에 재사용이 가능한 자바 객체를 커피콩에 비유해서 Bean이라고 정정했다.   
자바언어에서 스프링 프레임워크로 확장되면서 스프링 bean이라는 명칭까지 이어진게 된 것이다.  


## 1. 의존성 관리 자동화
빈으로 등록된 객체들은 Spring 컨테이너(BeanFactory, ApplicationContext)가 자동으로 의존성을 주입해준다.  
개발자가 직접 객체를 생성하고 의존성을 연결할 필요가 없어진다.  
또 컨테이너가 빌드 시점에 순환 의존성을 감지하여 설계 오류를 조기에 발견할 수 있다.

```java
@Service
class OrderService(
    private val productRepository: ProductRepository,  // 자동 주입
    private val paymentGateway: PaymentGateway        // 자동 주입
)
```

## 2. 싱글톤 패턴 구현
기본적으로 Spring은 빈을 싱글톤으로 관리하여 메모리 사용을 최적화하고, 불필요한 객체 생성을 방지한다.

```java
// 아래 두 userRepository는 동일한 인스턴스
@Service
class UserService(private val userRepository: UserRepository)

@Service
class AuthService(private val userRepository: UserRepository)
```

## 3. 생명주기 관리
Spring은 빈의 초기화와 소멸 과정을 자동으로 관리합니다. 이를 통해 리소스 할당 및 해제를 체계적으로 처리할 수 있습니다.

```java
@Component
class DatabaseConnection {
    @PostConstruct
    fun initialize() {
        // 초기화 로직
    }
    
    @PreDestroy
    fun cleanup() {
        // 리소스 정리 로직
    }
}
```


## 4. AOP(관점 지향 프로그래밍) 지원
빈으로 관리되는 객체들은 트랜잭션 관리, 로깅, 보안 등의 공통 관심사를 쉽게 적용할 수 있다.

```java
@Service
class TransferService(private val accountRepository: AccountRepository) {
    @Transactional  // AOP를 통한 트랜잭션 관리
    fun transferMoney(from: String, to: String, amount: BigDecimal) {
        // 송금 로직
    }
}
```


## 5. 테스트 용이성
빈으로 관리되는 컴포넌트는 모킹(mocking)이나 테스트용 구현체로 쉽게 대체할 수 있어 단위 테스트와 통합 테스트가 용이하다.

```java
@SpringBootTest
class UserServiceTest {
    @MockBean
    lateinit var userRepository: UserRepository
    
    @Autowired
    lateinit var userService: UserService
    
    @Test
    fun testGetUser() {
        // given
        val userId = 1L
        whenever(userRepository.findById(userId)).thenReturn(User(userId, "Test User"))
        
        // when
        val result = userService.getUser(userId)
        
        // then
        assertEquals("Test User", result.name)
    }
}
```

## 6. 설정의 중앙화
애플리케이션의 구성 요소들을 Bean으로 관리함으로써 설정을 중앙화하고 일관된 방식으로 관리할 수 있다.

```java
@Configuration
class AppConfig {
    @Bean
    fun dataSource(): DataSource {
        return HikariDataSource().apply {
            jdbcUrl = "jdbc:postgresql://localhost:5432/mydb"
            username = "user"
            password = "password"
            maximumPoolSize = 10
        }
    }
}
```