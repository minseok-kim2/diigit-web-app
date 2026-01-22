# Cross
신한대학교 마이크로스톤 AI 기반 여행지 추천 프로젝트

사용자 성향 테스트 → 추천 → 경로 산출 흐름을 갖는 여행 추천 서비스의 백엔드입니다.  
Spring Boot 기반으로 보안, 데이터 모델, API 구조를 정돈했고, 실제 서비스 운영을 고려해 확장성과 유지보수성을 우선합니다.

## 목차
- [프로젝트 개요](#프로젝트-개요)
- [핵심 기능](#핵심-기능)
- [기술 스택](#기술-스택)
- [프로젝트 구조](#프로젝트-구조)
- [API 요약](#api-요약)
- [설계 포인트](#설계-포인트)
- [로컬 실행](#로컬-실행)
- [문서](#문서)

## 프로젝트 개요
- 문제 정의: 사용자 성향 기반으로 여행지를 빠르고 안정적으로 추천
- 해결 방식: 성향 테스트 결과 + 목적지 + 공공 API 태그를 점수화하여 추천
- 서비스 흐름: 성향 테스트 → 추천 → 경로 산출

## 핵심 기능
- 인증/계정
  - 회원가입, 로그인
  - JWT 기반 인증
- 여행 성향 테스트
  - 테스트 결과 저장 및 사용자 유형 도출
- 추천
  - 성향 + 목적지 + 공공 API 태그 기반 점수화 추천
  - 공공 API 실패 시 DB 폴백
- 경로 추천(기본형)
  - 추천 리스트 기반 순서/경로 반환

## 기술 스택
- Framework: Spring Boot 3.5
- Security: Spring Security + JWT
- DB: JPA(Hibernate), MySQL
- Build: Gradle, Java 21

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

## 설계 포인트
- 역할 분리: Controller ↔ Service ↔ Repository 계층으로 변경에 강한 구조
- 보안: 표준 Spring Security + JWT로 인증/인가 구성
- 도메인 분리: DTO로 응답을 분리해 엔티티 변경 영향 최소화
- 확장성: 추천 로직을 서비스 단에 분리해 교체가 쉬움
- 복구 전략: 공공 API 실패 시 DB 후보 폴백

## 로컬 실행
```bash
./gradlew.bat bootRun
```

## 문서
- 추천 API/시퀀스: `cross/docs/recommendation.md`
- 인증 정책: `cross/docs/auth.md`
