# TC Tech Acamdemy week 2
#### Preview
---

**1. Bean 주입**

- Bean이란?
  -스프링 IoC 컨테이너가 관리하는 객체
  -@Bean 어노테이션을 통해 메소드로부터 반환된 객체를 스프링 컨테이너에 등록
  -빈은 클래스의 등록정보, Getter/Setter 메서드를 포함하며, 컨테이너에 사용되는 설정 메타데이터로 생성됨

- 주입 방법
  - 생성자 기반 주입
  > 종속성을 나타내는 인수로 생성자를 호출하는 컨테이너에 의해 수행, 컨테이너가 알아서 생성자에게 객체를 넣어줌
  > 스프링 4.3 부터는 단일 생성자인 경우 생성자에 @Authowired를 붙이지 않아도 된다.
  ```
    public class MyApp {
      private final Application application;

      public MyApp(Application application) {
          this.application = application;
      }
    }
  ```
  - Setter 기반 주입
  > 인수가 없는 생성자를 호출한 후 빈에서 setter를 호출해 주입하는 방식
  > 필드 final로 선언 불가능(가변 객체로만 사용 가능)
  > 생성자 기반 DI와 setter기반 DI를 혼합해 사용할 수 있기 때문에 필수적 종속성에서는 생성자를 사용하고 선택적인 종속성은 setter 기반 주입을 추천
  ```
  public class MyApp {
    private Application application;

    @Autowired
    public void setApplication(Application application) {
        this.application = application;
    }
  }
  ```
  - 필드 주입
    ```
    public class MyApp {
      @Autowired
      private Application application;
    }
    ```
  > 필드에 @Autowired 어노테이션을 붙여준다.
  > @Autowired란 스프링 컨테이너에 등록한 빈에게 의존관계주입이 필요할 때, DI(의존성 주입)을 도와주는 어노테이션

**2. Bean과 Component의 차이**
|@Bean|@Component|
|:----:|:-------|
|메소드레벨에서 선언|클래스 레벨에서 선언|
|외부 라이브러리가 제공하는 객체를 사용할 때 활용|내부에서 직접 접근 가능한 클래스를 사용|
|반환되는 객체(인스턴스)를 개발자가 수동으로 빈으로 등록하는 어노테이션|스프링이 런타임시에 컴포넌트스캔을 하여 자동으로 빈을 찾고(detect) 등록하는 어노테이션| 

**3. Field injection과 Constructor injection의 차이**
|Field injection|Constructor injection|
|:----:|:-------|
|순환 참조 위험 있음|순환 참조 방지 가능|  
|빈 생성 먼저, 다음 필드에 주입|먼저 빈을 생성 X, 생성자의 인자에 사용되는 빈을 찾거나 빈 팩터리에서 만드는 순서|
|필드를 final로 선언 불가능(가변 객체로만 사용 가능)|final로 선언 가능(가변 객체로 일어날 수 있는 오류 방지)|

**4. @Primary, @Qualifier annotation**
  - @Primary
  > 대상이 되는 클래스 주입하고 싶은 클래스에 @Primary 어노테이션을 붙이게 되면 스프링이 자동으로 해당 객체를 주입해준다.
  > 여러 개의 동일한 타입의 빈이 존재할 때 주입할 기본 빈을 지정할 때 사용
  ```
  @Component
  @Primary
  public class MyApp implements Application {
  }
  ```
  - @Qualifier
  > 주입하는 곳에서 @Qualifier 어노테이션을 통해 지정. 어노테이션의 value에 주입을 원하는 빈의 클래스 명을 lowerCamelCase로 입력하면 된다.
  > 여러 개의 동일한 타입의 빈 중에서 어떤 빈을 주입할지 명시적으로 지정할 때 사용
  > 어떤 빈을 주입할지 지정할 수 있는 값을 함께 제공해야 함
  ```
  public class MyApp {
    @Autowired
    private List<Application> application;
  }
  ```
**@Configuration**
  - 스프링 프레임워크에서 Java 기반의 설정 클래스를 정의할 때 사용
  - 클래스가 스프링 애플리케이션 컨텍스트에서 빈(Bean) 정의를 제공하는 클래스임을 표시할 수 있음
  ```
  @Configuration
  public class MyConfig {

    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
  }
  ```
  - myBean 메소드의 반환 값인 MyBean 객체가 스프링 빈으로 등록됨

  
