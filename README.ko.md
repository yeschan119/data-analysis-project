# AWS BI Reporting System Project  
### AWS QuickSight 기반 Production-Grade Analytics Platform

---

## 🚀 Executive Summary

대규모 교육 환경(2,000+ 학교)을 지원하는  
**AWS Native Embedded BI 플랫폼**을 설계·구현.

- 📊 30+ Interactive Reports
- 🗂 수십만 건 Incident 데이터
- ⚡ 평균 응답 속도 ~3초
- 💰 QuickSight 세션 비용 약 90% 절감
- 🏫 Multi-Tenant (School / District 분리 구조)
- 🛠 DB View + Index 기반 성능 튜닝

> 단순 대시보드가 아닌 **비용·성능·확장성까지 고려한 엔터프라이즈 BI 시스템**

---

## 🏗 Architecture (High-Level)

```
Angular → .NET API → Lambda → QuickSight
        ← Secure Embed URL ←
```

- 완전 임베디드 구조
- Role-Based Access Control
- Snapshot 기반 비용 최적화

---

## 🛠 Tech Stack

| Layer | Technology |
|------|-----------|
| Frontend | Angular |
| Backend | ASP.NET Core (.NET, C#) |
| BI | Amazon QuickSight |
| Database | AWS RDS (MySQL) |
| Metadata | DynamoDB |
| Infra | Lambda, Step Functions, SQS, EC2 |
| Storage | S3 |
| CI/CD | Azure DevOps |

---

## 📈 System Scale

| 항목 | 규모 |
|------|------|
| 데이터 기간 | 최근 5년 (COVID 이후) |
| Incident 데이터 | 수십만 건 |
| 리포트 수 | 약 30개 |
| 사용자 | 수천 명 |
| 학교 수 | 2,000+ |
| 평균 응답 시간 | ~3초 |
| 일일 조회 수 | 수천 ~ 수만 건 |

---

# 🔍 Detailed Sections (Click to Expand)

---

<details>
<summary><strong>📊 Report Categories</strong></summary>

### 1️⃣ 출결 및 출입 관리
- 학생 출석 추적
- 교직원 체크인 / 체크아웃
- 지각 모니터링
- 통계 분석

### 2️⃣ 학교 건강 및 감염 관리
- 백신 접종 현황
- 감염 사례 추적
- 시설 사용 로그
- 학교/교육청 단위 집계

### 3️⃣ 교육청 및 학교 요약 리포트
- 그래프 기반 대시보드
- Role-Based Data Visibility
  - 학교 사용자 → 자기 학교 데이터만
  - 교육청 사용자 → 전체 데이터

</details>

---

<details>
<summary><strong>🧠 Data Modeling Strategy</strong></summary>

### 복잡한 로직
- AWS RDS View 기반 구현
- QuickSight Dataset 단순화
- Join 비용 최소화

### 단순 계산
- Calculated Field 활용

### DAX 대신 Custom SQL
- Power BI DAX 대신 DB 레이어에서 집계 처리
- BI는 시각화에 집중

</details>

---

<details>
<summary><strong>⚡ Performance Optimization</strong></summary>

### 1️⃣ District-Level Heavy Reports

**문제**
- 모든 학교 집계
- 수천 건 메타데이터 조회
- RDS Join 비용 증가

**해결**
- School Metadata를 DynamoDB로 분리
- RDS ↔ DynamoDB Sync 설계
- Report는 DynamoDB 조회

**결과**
- RDS 부하 감소
- 응답 시간 개선

---

### 2️⃣ Daily Report SPICE 전략

- 실시간 필요 없음
- SPICE 적재 후 하루 1회 Refresh
- Direct Query 제거

→ 실시간 DB 부하 제거

---

### 3️⃣ DB Level Tuning

- Index 최적화
- Execution Plan 분석
- Join 구조 개선
- Query Cost 절감

</details>

---

<details>
<summary><strong>💰 Cost Optimization Architecture</strong></summary>

### 문제

QuickSight 세션 비용:
- $0.50 / session

대량 트래픽 시 하루 수천 달러 발생 가능

---

### Snapshot Rendering 전략

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Serve (No Session Cost)
   └─ No
        ↓
   Headless Render
        ↓
   Capture Snapshot
        ↓
   Store (S3)
        ↓
   Serve
```

---

### 결과

- 세션 생성 대폭 감소
- 비용 약 90% 절감
- 비인터랙티브 리포트 동일 UX 유지

</details>

---

<details>
<summary><strong>🧩 Authorization & Multi-Tenant Design</strong></summary>

- API 레벨 권한 검증
- QuickSight Parameter 기반 필터링
- School / District 데이터 범위 분리
- Secure Embedded Access

</details>

---

<details>
<summary><strong>🎯 Technical Decision: Why QuickSight?</strong></summary>

### 배경
- 기존 BI: Power BI
- 회사 클라우드: AWS

### 판단
- AWS Native 정합성
- 30개 리포트 QuickSight로 충분
- UI Customization 요구 높지 않음
- Snapshot 전략으로 비용 해결

→ QuickSight 채택

</details>

---

# 🏁 Impact

- 💰 비용 90% 절감
- ⚡ 3초 응답 유지
- 🏫 2,000+ 학교 지원
- 🧠 DB 튜닝 + View 모델링
- 🔐 Multi-Tenant 보안 구조
- ☁ Cloud-Native BI 설계

---

# Conclusion

이 프로젝트는 단순 대시보드가 아닌,

- Production-Grade AWS BI 플랫폼
- 비용 최적화 Embedded Analytics
- 멀티 테넌트 보안 설계
- 데이터베이스 성능 튜닝 기반 분석 시스템

을 구현한 클라우드 네이티브 엔지니어링 사례입니다.
