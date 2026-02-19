# AWS BI Reporting System Project  
**AWS QuickSight–Driven Incident Management Reporting System**

[한국어 🇰🇷](README.ko.md)

## Overview
This repository showcases a **data analysis and reporting platform** built on **AWS QuickSight**, designed to support **incident management and operational monitoring** for educational institutions (High Schools & Middle Schools).

The system delivers **10+ interactive reports** covering attendance, health incidents, and district-level insights, embedded seamlessly into a web application.

---

## High Level System Architecture

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

## Tech Stack

| Layer | Technology |
|------|-----------|
| Frontend (UI) | Angular |
| Backend API | ASP.NET Core (.NET, C#) |
| Data Analysis | AWS QuickSight |
| Data Source | AWS RDS, AWS DDB, AWS S3 |
| Integration | AWS Lambda |
| Data Pipeline | Lambda, Step Functions, SQS, EC2 |
| Visualization | Tables, Graphs, Insights, Tabs |
| Deploy | Azure Deveops |

---

## Report Generation Strategy

### Data Modeling
- **Complex Joins**
  - Implemented as **Database Views** in AWS RDS
  - Improves performance and simplifies QuickSight datasets

- **Simple Transformations**
  - Handled using **QuickSight Calculated Fields**
  - Enables flexible, temporary column creation

### Report Components
- Parameters
- Filters
- Tables
- Graphs & Charts
- Insights
- Tab-based dashboards

---

## Embedded Reporting Flow

1. Reports are created and managed in **AWS QuickSight**
2. Dashboards are embedded into Angular using **Iframe**
3. Secure access is handled via **.NET API + AWS Lambda**
4. User permissions dynamically control visible data scope

---

## Report Clients
- High Schools
- Middle Schools
- District-level Administrators

**Primary Purpose:**  
Incident Management System Monitoring & Analytics

---

## Report Categories

### 1️⃣ Attendance & Access Management (Daily / Monthly / Yearly)
- Student attendance tracking
- Teacher & staff check-in / check-out reports
- Late arrival monitoring
- Student tardiness management enhancements

---

### 2️⃣ School Health & Disease Management (e.g., COVID)
- Vaccination status reporting
  - Vaccinated or not
  - Vaccination dates
- Facility-based exposure tracking
  - Logs all facilities used by infected individuals
- Aggregated statistics by:
  - School
  - District
  - Date range

---

### 3️⃣ District & School-Level Summary Reports
- Graph-based summary dashboards
- Data aggregation by:
  - Individual school
  - Entire district
- **Role-based visibility**
  - School users → Access only their school data
  - District officials → Access all schools data

---

## Authorization & Access Control
- Permission logic enforced at API level
- Embedded QuickSight dashboards respect user scope
- Supports multi-tenant educational environments

---

## Cost Optimization (Important)
- QuickSight charges **$0.50 per session**
- Implemented a **session caching system**
- Reduced redundant session creation
- Significantly lowered reporting costs

### Cost Optimization Details
-> [![Cost Optimization](https://img.shields.io/badge/Docs-Cost%20Optimization-2ea44f?style=for-the-badge)](./COST_OPTIMIZATION.md)

<img width="600" height="400" alt="Screenshot 2026-02-19 at 23 49 28" src="https://github.com/user-attachments/assets/559fa96e-7fd8-4795-b7f1-75e49595aa4d" />

---

## 🔄 End-to-End Optimized Flow

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Serve Snapshot (Fast, No Cost)
   └─ No
        ↓
   Headless Browser Render
        ↓
   Apply QuickSight Parameters
        ↓
   Snapshot Capture
        ↓
   Store Snapshot
        ↓
   Serve Snapshot
```

---

## Key Outcomes

- Significant reduction in QuickSight session cost
- High-performance report delivery
- Scales efficiently with user growth
- Maintains functional parity with interactive reports where needed

---

## Design Considerations

- Snapshot cache invalidation strategy (time-based or data-change-based)
- Secure snapshot access aligned with user permissions
- Clear separation between **public static** and **private dynamic** reports

---

This approach ensures **cost efficiency without sacrificing usability**, making QuickSight viable at scale for large educational districts.

---

## Key Takeaways
- Production-grade data analytics platform
- Focus on performance, security, and cost efficiency
- Demonstrates full-stack and cloud expertise
