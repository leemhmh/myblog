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
> 끝에는 동사 대신에 명사를 사용할 것, 복수형을 사용할 것, Status Code 활용하기 등등..







﻿

﻿

