# Cross
신한대학교 마이크로스톤 AI를 이용한 여행지 설정 프로그램

이 프로젝트는 **사용자 성향 테스트 → 추천 → 경로 산출**의 흐름을 갖는 여행 추천 서비스의 백엔드를 구현합니다.  
Spring Boot 기반으로 **보안, 데이터 모델, API 구조**를 정돈했고, 실제 서비스 운영을 고려해 **확장성과 유지보수성**을 우선합니다.

## 왜 이 구조인가
- **역할 분리**: Controller(요청/응답) ↔ Service(비즈니스) ↔ Repository(DB)로 계층을 나눠 변경에 강하게 설계했습니다.
- **신뢰성**: 인증/인가 흐름을 표준 Spring Security + JWT로 구성해 서비스 안정성과 확장성에 대응합니다.
- **협업 지향**: 프론트와의 명세 협업을 위해 REST API를 명확하게 분리했습니다.

## 기술 스택
- **Framework**: Spring Boot 3.5
- **Security**: Spring Security + JWT
- **DB**: JPA(Hibernate), H2(개발용)
- **Build**: Gradle, Java 21

## 핵심 기능
- **인증/계정**
  - 회원가입, 로그인
  - JWT 기반 인증
- **여행 성향 테스트**
  - 테스트 결과 저장
- **추천**
  - 성향 + 테마 + 지역 기반 점수화 추천
- **경로 추천(기본형)**
  - 입력된 경유지를 기준으로 거리/시간 산출

## 프로젝트 구조
```
src/main/java/kr/ac/shinhan
├─ config                # 보안/설정/시드 데이터
├─ controller            # API 엔드포인트
├─ domain                # JPA Entity
├─ dto                   # 요청/응답 모델
├─ repository            # JPA Repository
├─ security              # JWT/인증 필터
└─ service               # 핵심 비즈니스 로직
```

## API 요약
- `POST /api/auth/register` : 회원가입
- `POST /api/auth/login` : 로그인
- `POST /api/tests` : 성향 테스트 저장
- `POST /api/recommendations` : 여행지 추천
- `POST /api/routes` : 경로 추천

## 신뢰성과 구조에 대한 판단 포인트
- **보안**: JWT 필터로 인증 상태를 분리하고 세션 비저장을 기본값으로 설정했습니다.
- **도메인 중심 설계**: User, PersonalityTest, Destination 엔티티로 핵심 도메인을 분명히 표현했습니다.
- **확장성**: 추천 로직은 서비스 단에 분리되어 AI 모델/외부 API 연동 시 교체가 쉽습니다.
- **유지보수성**: DTO를 분리해 API 스펙 변경이 엔티티에 영향을 주지 않도록 설계했습니다.

## 로컬 실행
```bash
cd w06-1
./gradlew.bat bootRun
```

## 향후 확장 계획
- 공공 API 연동(관광지, 교통, 날씨)
- 추천 점수 모델 고도화(가중치, ML 모델 연계)
- 경로 최적화 알고리즘(실시간 교통 데이터 반영)
