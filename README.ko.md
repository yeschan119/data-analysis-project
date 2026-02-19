# AWS BI Reporting System Project  
**AWS QuickSight 기반 Incident Management 리포팅 시스템**

---

## 📌 Overview

이 프로젝트는 **AWS QuickSight 기반 데이터 분석 및 리포팅 플랫폼**을 구현한 사례입니다.  
중·고등학교 대상 **Incident Management 및 운영 모니터링**을 지원하도록 설계되었습니다.

총 **10개 이상의 인터랙티브 리포트**를 제공하며, 출결, 건강 사고, 학군(District) 단위 통계 등을 웹 애플리케이션에 자연스럽게 임베딩합니다.

---

# 🏗 High Level System Architecture

```
Angular (UI)
   ↓
.NET API
   ↓
AWS Lambda
   ↓
AWS QuickSight
   ↓
AWS Lambda (Embed URL 생성)
   ↓
.NET API
   ↓
Angular (Iframe Embed)
```

---

# 🧰 Tech Stack

| Layer | Technology |
|------|-----------|
| Frontend (UI) | Angular |
| Backend API | ASP.NET Core (.NET, C#) |
| Data Analysis | AWS QuickSight |
| Data Source | AWS RDS, AWS DynamoDB, AWS S3 |
| Integration | AWS Lambda |
| Data Pipeline | Lambda, Step Functions, SQS, EC2 |
| Visualization | Tables, Graphs, Insights, Tabs |
| Deploy | Azure DevOps |

---

# 📊 Report Generation Strategy

## 1️⃣ Data Modeling

### ✅ Complex Joins
- AWS RDS 내 **Database View**로 구현
- QuickSight Dataset 단순화
- 쿼리 성능 개선

### ✅ Simple Transformations
- **QuickSight Calculated Fields** 활용
- 유연한 컬럼 생성 가능
- 요구사항 변경에 빠르게 대응

---

## 2️⃣ Report Components

- Parameters
- Filters
- Tables
- Graphs & Charts
- Insights
- Tab 기반 대시보드

---

# 🔐 Embedded Reporting Flow

1. 리포트는 **AWS QuickSight**에서 생성 및 관리
2. Angular에서 **Iframe 방식으로 임베딩**
3. 보안 접근은 **.NET API + AWS Lambda**를 통해 처리
4. 사용자 권한에 따라 데이터 Scope 동적 제어

---

# 👥 Report Clients

- High Schools
- Middle Schools
- District-level Administrators

**Primary Purpose:**  
Incident Management System Monitoring & Analytics

---

# 📂 Report Categories

## 1️⃣ Attendance & Access Management (Daily / Monthly / Yearly)

- 학생 출결 추적
- 교직원 체크인 / 체크아웃 리포트
- 지각 모니터링
- 학생 지각 관리 기능 개선

---

## 2️⃣ School Health & Disease Management (e.g., COVID)

- 백신 접종 현황
  - 접종 여부
  - 접종 날짜
- 시설 기반 노출 추적
  - 확진자의 사용 시설 로그 기록
- 집계 기준:
  - 학교별
  - 학군별
  - 날짜 범위별

---

## 3️⃣ District & School-Level Summary Reports

- 그래프 기반 요약 대시보드
- 집계 기준:
  - 개별 학교
  - 전체 학군
- **Role-based visibility**
  - 학교 사용자 → 해당 학교 데이터만 접근
  - 학군 관리자 → 전체 학교 데이터 접근

---

# 🔒 Authorization & Access Control

- API 레벨에서 Permission Logic 적용
- Embedded QuickSight 대시보드는 사용자 Scope를 준수
- Multi-tenant 교육 환경 지원

---

# 💰 Cost Optimization (중요)

AWS QuickSight는 **세션당 $0.50** 과금됩니다.

대규모 사용자 환경에서 세션 비용을 제어하기 위해 다음 전략을 도입했습니다:

- 세션 캐싱 전략
- Snapshot 기반 렌더링 구조
- 중복 세션 생성 최소화
- S3 기반 Snapshot 재사용 구조

---

## 📷 Snapshot 기반 비용 절감 전략

### 핵심 개념

- 동일 리포트 반복 접근 시 세션 생성 방지
- Headless Browser 기반 Snapshot 생성
- Amazon S3 저장 후 재사용

---

## 🔄 End-to-End Optimized Flow

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Serve Snapshot (Fast, No Session Cost)
   └─ No
        ↓
   Headless Browser Render
        ↓
   Apply QuickSight Parameters
        ↓
   Snapshot Capture
        ↓
   Store Snapshot (S3)
        ↓
   Serve Snapshot
```

---

## 📈 Key Outcomes

- QuickSight 세션 비용 대폭 절감
- 빠른 리포트 로딩 속도
- 사용자 증가에 따른 수평 확장 가능
- 인터랙티브 기능 유지
- 비용 예측 가능성 확보

---

# 🧠 Design Considerations

- Snapshot Cache Invalidation 전략
  - TTL 기반
  - 데이터 변경 기반
- 사용자 권한과 일치하는 Snapshot 접근 제어
- Public Static vs Private Dynamic 리포트 분리
- Step Functions 기반 재시도 및 장애 복구
- EC2 Auto Scaling 기반 무유휴 인프라 운영

---

# 🏁 Key Takeaways

- Production-grade 데이터 분석 플랫폼
- 성능, 보안, 비용 효율성을 모두 고려한 설계
- Full-stack + Cloud 아키텍처 역량 입증
- 대규모 교육 기관 환경에서 확장 가능한 BI 시스템 구현
