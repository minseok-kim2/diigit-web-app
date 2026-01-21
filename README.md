# Cross
신한대학교 마이크로스톤 AI 기반 여행지 추천 프로젝트

이 프로젝트는 사용자 성향 테스트 → 추천 → 경로 산출의 흐름을 갖는 여행 추천 서비스의 백엔드를 구현합니다.  
Spring Boot 기반으로 보안, 데이터 모델, API 구조를 정돈했고, 실제 서비스 운영을 고려해 확장성과 유지보수성을 우선합니다.

## 왜 이 구조인가
- 역할 분리: Controller(요청/응답) ↔ Service(비즈니스) ↔ Repository(DB)로 계층을 나눠 변경에 강하게 설계했습니다.
- 신뢰성: 인증/인가 흐름을 표준 Spring Security + JWT로 구성해 서비스 안정성과 확장성에 대응합니다.
- 협업 지향: 프론트와의 명세 협업을 위해 REST API를 명확하게 분리했습니다.

## 기술 스택
- Framework: Spring Boot 3.5
- Security: Spring Security + JWT
- DB: JPA(Hibernate), MySQL
- Build: Gradle, Java 21

## 핵심 기능
- 인증/계정
  - 회원가입, 로그인
  - JWT 기반 인증
- 여행 성향 테스트
  - 테스트 결과 저장 및 사용자 유형 도출
- 추천
  - 성향 + 목적지 + 공공 API 태그 기반 점수화 추천
  - 공공 API 실패 시 DB 폴백
- 경로 추천
  - 추천 리스트 기반 순서/경로 반환

## 프로젝트 구조
```
src/main/java/com/example/cross
├─ config                # 보안/설정/시드 데이터
├─ controller            # API 엔드포인트
├─ dto                   # 요청/응답 모델
├─ entity                # JPA Entity
├─ repository            # JPA Repository
└─ service               # 핵심 비즈니스 로직
```

## API 요약
- `POST /api/auth/login` : 로그인
- `GET /api/recommendations` : 여행지/경로 추천
- `GET /api/places` : 장소 목록
- `GET /api/places/{id}` : 장소 상세
- `POST /api/test/submit` : 성향 테스트 결과 저장

## 신뢰성과 구조에 대한 판단 포인트
- 보안: 세션 기반 웹 로그인 + JWT 기반 API 인증을 분리하여 운영 안정성을 확보했습니다.
- 도메인 분리: DTO로 응답을 분리해 엔티티 변경이 API에 영향을 주지 않도록 설계했습니다.
- 확장성: 추천 로직을 서비스 단에 분리해 AI 모델/외부 API 연동 시 교체가 쉽습니다.
- 복구 전략: 공공 API 실패 시 DB 후보로 폴백하여 서비스 연속성을 유지합니다.

## 로컬 실행
```bash
./gradlew.bat bootRun
```

## 문서
- 추천 API/시퀀스: `cross/docs/recommendation.md`
- 인증 정책: `cross/docs/auth.md`
