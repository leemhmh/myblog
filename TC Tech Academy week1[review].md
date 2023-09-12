# TC Tech Acamdemy week 1
#### Review
---

> ## 소프트웨어 개발 프로세스
﻿
요구사항 분석 - 요구사항 수집&분석

APL 스펙 개발 - API 스펙 정의하여 프론트엔드에 공유 & 개발

테스트 & QA - 요구사항과 비교하여 테스트와 QA

배포 - 각 환경 별로 버젼 배포

> ## REST API
REST API란?
- **서버와 데이터베이스에 대한 출입구 역할**

  (허용된 사람들에게만 접근성 부여)
- **프로그램끼리의 통신 가능**

   (애플리케이션과 기기간 데이터 수신 및 전송 원활하게)
- **모든 접속을 표준화**

   (기계/운영체제 상관없이 동일한 액세스 얻을 수 있음)

**웹의 장점을 최대한 활용 가능한 아키텍처**

*but 완벽히 지켜지는 것은 아님, 꼭 Restful한게 좋은 것도 아님(예: 카카오)*

---
**구성**
- 자원(Resource)
  - URI : ex) https://www.yonsei.ac.kr/sc/etc/calendar.jsp
- 행위(Verb)
  - HTTP METHOD : ex) GET, PUT
- 표현(Representations)

**특징**
- Uniform(유니폼 인터페이스 => 통일되고 한정적인 인터페이스로 조작 수행)
- Stateless(무상태성 => 작업을 위한 상태정보를 따로 저장X 관리X, 중요한 장점(서비스 자유도 증가, 구현의 단순화)이자 특성)
- Casheable(캐시 가능 => 웹에서 사용하는 인프라 그대로 사용 가능)
- Self-descriptiveness(자체 표현 구조 => REST API 메시지만 보고도 쉽게 이해 가능)
- Client - Server 구조 => 서버와 클라이언트의 각 역할이 확실히 구분, 의존성 감소
- 계층형 구조 => ex) 보안 ,로드 밸런싱, 암호화 계층을 통해 구조상의 유연성 확보

**REST API Best Practice**
https://www.freecodecamp.org/news/rest-api-best-practices-rest-endpoint-design-examples/
> 끝에는 동사 대신에 명사를 사용할 것(중요), 동작은 HTTP 메소드를 사용할 것, Status Code 활용하기(보안적 측면에도 중요) 등등..

---
**Spring Boot 특징**
- 제어 역전
> IoC 컨테이너 제공, 객체의 생성과 관계 설정을 스프링에 위임
- 의존성 주입
> 객체 간 관계 설정, 결합도를 낮추고 유연성과 테스트 용이성 향상
- AOP(관점지향 프로그래밍) 지원
> 이를 통해 애플리케이션의 핵심 비즈니스 로직과 부가적인 기능(로깅, 트랜잭션 관리 등)을 분리 가능 => 모듈화
- 웹 개발 지원
> MVC 아키텍처 지원

**MVC**
- Model
  - Data와 애플리케이션이 무엇을 할 것인지를 정의하는 부분, 내부 비즈니스 로직을 처리하기 위한 역할을 함.
- View
  - 사용자에게 보여주는 화면(UI), 여러 개 존재 가능, 상태가 없고 데이터 조작 X
- Controller
  - Model과 View 사이를 이어주는 인터페이스 역할

*어떤 곳으로 분류할지 실무에선 많은 고민이 필요함*

**Controller와 RestController의 차이
https://www.geeksforgeeks.org/difference-between-controller-and-restcontroller-annotation-in-spring/
> Controller는 view를 반환, 데이터를 반환하려면 @Controller와 @ResponseBody를 조합하여 사용
> RestController는 Data(주로 json)를 반환, @RestController는 @Controller와 @ResponseBody의 동작을 하나로 결합한 것  

- @Service
  - 비즈니스 로직, 트랜잭션 처리 담당, Repository에서 얻어온 정보를 가공 후 다시 Controller에 보냄
 
- @Repository
  - 해당 클래스가 DB에 접근하는 클래스임을 명시함

**예측을 하기 쉽도록 빠른 피드백**
Swagger등을 이용하여 API 스펙을 정해두어야 업무가 수월해짐





﻿

﻿

