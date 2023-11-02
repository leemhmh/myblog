# TC Tech Acamdemy week 7
#### Preview
---

## Redis(remote dictionary server)란

Redis는

데이터베이스, 캐시 및 메시지 브로커로 사용되는 BSD 라이센스인 오픈 소스 데이터 구조 저장소입니다.

문자열, 해시, 목록, 집합, 범위 쿼리와 함께 정렬된 집합, 비트맵, 하이퍼로그, 반경 쿼리와 스트림이 있는 지리공간 인덱스 등의 데이터 구조를 지원합니다.

Redis는 기본 제공 복제, Lua 스크립팅, LRU 제거, 트랜잭션 및 다양한 수준의 온디스크 지속성을 가지고 있으며 Redis Sentinel을 통한 고가용성 및 Redis Cluster를 통한 자동 파티셔닝 기능을 제공합니다.

- 레디스 공홈, https://redis.io/

일반적으로 mysql, oracle 등의 RDB들이 하드디스크에 데이터를 저장하고 관리하는 것과는 달리 레디스는 데이터를 주기억장치인 ram, 즉 In-memory에서 관리

# In-memory 방식의 장단점

장점: 데이터를 저장하고 조회할 때, 하드디스크를 오고 가는 과정을 거치지 않고 메모리 내부에서 처리가 되므로, 기존 RDB보다 훨씬 더 빠른 성능을 발휘. 병목 현상이 발생하지 않음

단점: 서버에 장애가 발생할 경우, 데이터 유실이 발생할 수 있다는 점

서버의 메모리 용량을 초과하는 데이터를 redis에서 처리할 경우, 서버 자체에도 문제가 생기며, ram의 특성인 휘발성에 따라 ram에 저장되었던 레디스의 데이터들은 유실

실제 문제 사례: 쿠팡

# Redis 사용


implementation 'org.springframework.boot:spring-boot-starter-data-redis'

- build.gradle 에 spring-boot-starter-data-redis 추가하고 빌드


spring:
  redis:
    host: localhost
    port: 6379

- application.yaml 에 host 와 port 를 설정
- localhost:6379 는 기본값이기 때문에 만약 Redis 를 localhost:6379 로 띄웠다면 따로 설정하지 않아도 연결
- 일반적으로 운영 서버에서는 별도의 Host 를 사용하기 때문에 값을 이렇게 별도의 값을 세팅하고 Configuration 에서 Bean 에 등


@Configuration
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String host;

    @Value("${spring.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

- Redis 사용을 위한 기본 Configuration
- application.yaml 에 설정한 값을 @Value 어노테이션으로 주입

Spring Data Redis 의 Redis Repository 를 이용하면 간단하게 Domain Entity 를 Redis Hash 로 만들 수 있음
다만 트랜잭션을 지원하지 않기 때문에 만약 트랜잭션을 적용하고 싶다면 RedisTemplate 을 사용

# Entity

@Getter
@RedisHash(value = "people", timeToLive = 30)
public class Person {

    @Id
    private String id;
    private String name;
    private Integer age;
    private LocalDateTime createdAt;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
        this.createdAt = LocalDateTime.now();
    }
}

- Redis 에 저장할 자료구조인 객체를 정의
- 일반적인 객체 선언 후 @RedisHash 를 붙임
  - value : Redis 의 keyspace 값으로 사용
  - timeToLive : 만료시간을 seconds 단위로 설정 가능. 기본값은 만료시간이 없는 -1L
- Id 어노테이션이 붙은 필드가 Redis Key 값이 되며 null 로 세팅하면 랜덤값이 설정
  - keyspace 와 합쳐져서 레디스에 저장된 최종 키 값은 keyspace:id 가 됨

# Repository

public interface PersonRedisRepository extends CrudRepository<Person, String> {
} 

- CrudRepository 를 상속받는 Repository 클래스 추가

RedisTemplate 을 사용하면 특정 Entity 뿐만 아니라 여러가지 원하는 타입을 넣을 수 있음
template 을 선언한 후 원하는 타입에 맞는 Operations 을 꺼내서 사용

# Config 설정 추가
@Configuration
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String host;

    @Value("${spring.redis.port}")
    private int port;

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory(host, port);
    }

    @Bean
    public RedisTemplate<?, ?> redisTemplate() {
        RedisTemplate<?, ?> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        return redisTemplate;
    }
}
- RedisTemplate 에 LettuceConnectionFactory 을 적용해주기 위해 설정




## 성능 테스트란
쉽게 말하면 내가 만든 웹 서버(API)가 얼마나 많은 사용자들을 감당할 수 있는지 테스트하는 것
두 가지 설계에 대해서 어떤 설계가 더 좋은 성능을 내는지 확인하기 위해서도 사용

성능을 판단하는 대표적인 지표로는 Throughput(처리량)과 Latency(지연시간)

# Throughput
간당 처리량을 의미하고 TPS(Transaction per seconds), RPS(Request per seconds) 와 같은 세부항목이 있음 
이러한 수치들은 해당 서비스가 1초당 어느정도의 작업을 처리할 수 있는지 나타내기 때문에 높은 수치일수록 성능이 좋다고 말함
*가장 낮은 구간이 전체 시스템의 성능이 됨

# Latency
지연시간을 의미하고 이는 시스템이 클라이언트로부터 Request 를 받아서 Response 를 보내주기까지 걸리는 시간을 의미 
말 그대로 특정 작업을 얼마나 빨리 처리할 수 있는지 나타내는 성능 지표 
Latency 는 당연히 낮은게 더 빨리 응답한 것이기 때문에 낮은 수치일수록 성능이 좋다고 말함
*모든 구간을 더한 값으로 성능 측정



