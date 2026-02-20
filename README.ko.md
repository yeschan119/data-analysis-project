# AWS BI Reporting System Project  
**AWS QuickSight 기반 Analytics & Reporting 플랫폼**

---

## 개요 (Overview)

이 프로젝트는 **Amazon QuickSight**를 활용하여 구축한  
**대규모 교육 환경을 위한 Production-Grade AWS BI 리포팅 플랫폼**.

본 시스템은 운영 모니터링, 출결 관리, 건강 관리, 교육청 단위 통계 분석 등을 포함한  
**30개 이상의 인터랙티브 리포트**를 웹 애플리케이션에 완전 임베딩 방식으로 제공.

설계 시 다음 요소를 핵심 가치에 집중:

- 성능 최적화
- 비용 효율성
- 멀티 테넌트 권한 관리
- 확장 가능한 AWS 네이티브 아키텍처
- 데이터베이스 레벨 튜닝

---

# High-Level Architecture

```
Angular (UI)
   ↓
.NET API
   ↓
AWS Lambda
   ↓
AWS QuickSight
   ↓
AWS Lambda (Embed URL)
   ↓
.NET API
   ↓
Angular (Iframe Embed)
```

---

# 기술 스택 (Tech Stack)

| 구성 레이어 | 기술 |
|-------------|------|
| Frontend | Angular |
| Backend | ASP.NET Core (.NET, C#) |
| BI 엔진 | Amazon QuickSight |
| Database | AWS RDS (MySQL) |
| 메타데이터 저장소 | DynamoDB |
| 스토리지 | Amazon S3 |
| 연동 계층 | AWS Lambda |
| 데이터 파이프라인 | Lambda, Step Functions, SQS, EC2 |
| 배포 | Azure DevOps |

---

# 시스템 규모 (System Scale)

## 데이터 규모

- COVID 시기부터 최근 5년간 사건·사고 데이터 관리
- 수십만 건 이상의 Incident 데이터
- Incident 주요 필드:
  - 고유 Incident ID
  - 유형 (Positive / Suspected / Negative 등)
  - 사용자 정보
  - 백신 접종 상태
  - 시설(Site)
  - 생성 시각 (CreatedAt)
  - 운영 메타데이터

---

## 사용량 및 트래픽

- 일 평균 리포트 조회 수: 수천 ~ 수만 건
- 평균 응답 시간: 약 3초
- 총 리포트 수: 약 30개
- 시각화 유형: 약 10가지 (Table, Insight, Graph, KPI, Tab 등)
- 사용자 규모:
  - 약 2,000개 학교
  - 수천 명의 관리 직원

---

# 리포트 카테고리

## 1️⃣ 출결 및 출입 관리

- 학생 출석 추적
- 교직원 체크인 / 체크아웃 로그
- 지각 모니터링
- 지각 통계 분석

---

## 2️⃣ 학교 건강 및 감염 관리

- 백신 접종 현황 리포트
- 감염 사례 추적
- 감염자 시설 사용 이력 로그
- 학교 / 교육청 / 기간 단위 집계

---

## 3️⃣ 교육청 및 학교 단위 요약 리포트

- 그래프 기반 대시보드
- 학교 단위 / 교육청 단위 집계 통계
- 역할 기반 데이터 접근 제어:
  - 학교 사용자 → 본인 학교 데이터만 접근
  - 교육청 사용자 → 전체 학교 데이터 접근

---

# 데이터 모델링 전략

## 복잡한 로직 처리

- AWS RDS 내 **Database View**로 구현
- QuickSight Dataset 단순화
- 조인 비용 최소화

---

## 단순 계산 처리

- QuickSight Calculated Field 활용

---

## DAX 대신 Custom SQL 사용

- Power BI DAX 대신
- DB 레이어에서 Custom SQL 기반 집계 처리
- BI 레이어는 시각화에 집중

---

# 성능 최적화 전략

## 1️⃣ 교육청 단위 리포트 최적화

### 문제

- 모든 학교 데이터를 집계해야 함
- 수천 건의 학교 메타데이터 조회
- RDS Join 비용 증가

### 해결 방안

- 학교 메타데이터를 DynamoDB로 분리 저장
- RDS ↔ DynamoDB 동기화 로직 구현
- 리포트는 DynamoDB에서 학교 정보 조회

### 결과

- RDS 부하 감소
- 교육청 단위 리포트 응답 시간 개선

---

## 2️⃣ Daily 리포트 SPICE 최적화

### 특성

- 실시간 확인 목적 아님
- 전날 사건 요약 리포트

### 해결 방안

- QuickSight SPICE 엔진에 데이터 적재
- 하루 1회 Refresh
- Direct Query 비활성화

### 결과

- 실시간 DB 부하 제거
- 안정적인 응답 속도 확보

---

## 3️⃣ 데이터베이스 레벨 튜닝

- 인덱스 최적화
- 실행 계획 분석
- 조인 구조 개선
- View 기반 모델링
- 쿼리 비용 절감

---

# 임베디드 리포팅 흐름

1. QuickSight에서 리포트 생성
2. Angular에서 Iframe 방식으로 임베딩
3. .NET API + Lambda가 보안 Embed URL 생성
4. 역할 기반 권한 검증 수행
5. 멀티 테넌트 데이터 격리 유지

---

# 권한 및 멀티 테넌트 설계

- API 레벨 권한 검증
- QuickSight Parameter 기반 필터링
- 학교 / 교육청 단위 접근 범위 분리
- 보안 임베디드 접근 제어

---

# 비용 최적화 전략

## 문제

QuickSight 세션 비용:
- **세션당 $0.50**

일일 수만 건 리포트 조회 시:
- 하루 수천 달러 비용 발생 가능

---

## Snapshot 기반 렌더링 아키텍처

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Snapshot 제공 (세션 비용 없음)
   └─ No
        ↓
   Headless Browser 렌더링
        ↓
   파라미터 적용
        ↓
   Snapshot 생성
        ↓
   S3 저장
        ↓
   Snapshot 제공
```

---

### Cost Optimization Details

[![Cost Optimization](https://img.shields.io/badge/Docs-Cost%20Optimization-2ea44f?style=for-the-badge)](./COST_OPTIMIZATION.md)

<img width="600" height="400" alt="Cost Optimization Architecture" src="https://github.com/user-attachments/assets/559fa96e-7fd8-4795-b7f1-75e49595aa4d" />

---

### 결과

- 세션 생성 횟수 대폭 감소
- 예상 비용 대비 약 90% 절감
- 비인터랙티브 리포트에 대해 동일한 시각적 결과 유지

---

# 기술적 의사결정: 왜 QuickSight인가?

## 배경

- 기존 BI 도구: Power BI
- 회사 클라우드 환경: AWS

---

## 평가 요약

| 평가 요소 | 결론 |
|------------|------|
| 클라우드 정합성 | AWS Native 우선 |
| 리포트 수 (~30개) | QuickSight로 충분히 구현 가능 |
| UI 커스터마이징 | Power BI가 강점이나 필수 요건 아님 |
| 비용 모델 | Snapshot 전략으로 해결 가능 |

---

### 최종 결정

AWS 네이티브 BI 플랫폼인 QuickSight 선택

---

# 프로젝트 임팩트 (Impact)

## 비용 절감

- 예상 리포팅 비용 대비 약 90% 절감

---

## 성능

- 평균 3초 응답 시간 유지
- 수십만 건 데이터 처리

---

## 확장성

- 2,000개 이상 학교 지원
- 수천 명 사용자
- 멀티 테넌트 구조

---

## 엔지니어링 깊이

- 인덱스 튜닝
- View 기반 데이터 모델링
- SPICE 전략 분리
- Snapshot 캐싱 아키텍처
- RDS-DynamoDB 동기화 설계
- 역할 기반 보안 설계

---

# 결론

이 프로젝트는 단순한 대시보드 구현이 아니라,

- Production-Grade AWS BI 플랫폼
- 비용 최적화된 임베디드 분석 시스템
- 멀티 테넌트 보안 리포팅 구조
- 데이터베이스 튜닝 기반 분석 플랫폼

을 구현한 클라우드 네이티브 엔지니어링 사례입니다.
