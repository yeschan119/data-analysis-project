# AWS BI Reporting System Project  
### AWS QuickSight–Driven Analytics & Reporting Platform

[한국어 🇰🇷](https://github.com/yeschan119/aws-bi-reporting-system/blob/main/README.ko.md)

---

## Executive Summary

Production-grade **AWS Native Embedded BI Platform** designed for large-scale educational environments.

- 30+ Interactive Reports
- Hundreds of thousands of incident records (5-year dataset)
- ~3s average response time
- 2,000+ schools / Thousands of users
- ~90% QuickSight session cost reduction
- Database-level tuning + View-based modeling
- Multi-tenant isolation architecture

---

## High-Level Architecture

```
Angular → .NET API → Lambda → QuickSight
        ← Secure Embed URL ←
```

- Fully embedded reporting
- Role-based access control
- Snapshot-based cost optimization

---

## Tech Stack

| Layer | Technology |
|------|-----------|
| Frontend | Angular |
| Backend | ASP.NET Core (.NET, C#) |
| BI Engine | Amazon QuickSight |
| Database | AWS RDS (MySQL) |
| Metadata Store | DynamoDB |
| Storage | Amazon S3 |
| Integration | AWS Lambda |
| Pipeline | Lambda, Step Functions, SQS, EC2 |
| Deployment | Azure DevOps |

---

# Detailed Sections (Click to Expand)

---

<details>
<summary><strong>📈 System Scale</strong></summary>

## Data Volume

- 5 years of incident data (since COVID period)
- Hundreds of thousands of records
- Each incident contains:
  - Unique Incident ID
  - Type (Positive / Suspected / Negative)
  - User Information
  - Vaccination Status
  - Site / Facility
  - CreatedAt Timestamp
  - Operational metadata

## Usage & Traffic

- Daily traffic: Thousands ~ Tens of thousands report views
- Average response time: ~3 seconds
- Total reports: ~30
- Visualization types: ~10 (Table, Insight, Graph, KPI, Tabs)
  
   <img width="700" height="350" alt="Screenshot 2026-02-20 at 08 12 42" src="https://github.com/user-attachments/assets/d4d865f4-35d5-4b2e-a0ed-42ee1933df47" />
   
   <img width="700" height="390" alt="Screenshot 2026-02-20 at 08 13 33" src="https://github.com/user-attachments/assets/f87cb71a-8223-4b74-8bbd-afdc67bbdbc8" />
   
   <img width="700" height="350" alt="Screenshot 2026-02-20 at 08 29 10" src="https://github.com/user-attachments/assets/3e902417-3808-490c-99cf-c60b2b505d34" />


- Users:
  - ~2,000 schools
  - Thousands of administrative staff

</details>

---

<details>
<summary><strong>📊 Report Categories</strong></summary>

## 1️⃣ Attendance & Access Management
- Student attendance tracking
- Staff check-in / check-out logs
- Late arrival monitoring
- Tardiness analytics

## 2️⃣ School Health & Disease Monitoring
- Vaccination reporting
- Infection tracking
- Facility exposure logging
- Aggregation by school, district, date range

## 3️⃣ District & School-Level Summary Reports
- Graph-based dashboards
- Aggregated statistics
- Role-based visibility:
  - School users → Access only their school
  - District users → Access all schools

</details>

---

<details>
<summary><strong>🧠 Data Modeling Strategy</strong></summary>

## Complex Logic
- Implemented as **Database Views in RDS**
- Reduces dataset complexity
- Improves QuickSight performance

## Simple Calculations
- Implemented using QuickSight Calculated Fields

## Custom SQL Instead of DAX
- Heavy aggregation handled at DB layer
- Visualization layer kept lightweight

</details>

---

<details>
<summary><strong>⚡ Performance Optimization</strong></summary>

## 1️⃣ District-Level Heavy Reports

### Problem
- Aggregation across all schools
- Thousands of school metadata reads
- Expensive RDS joins

### Solution
- Isolated school metadata in DynamoDB
- Implemented RDS ↔ DynamoDB sync logic
- Reports fetch school metadata from DynamoDB

### Result
- Reduced RDS join cost
- Improved district-level performance

---

## 2️⃣ Daily Report Optimization (SPICE)

### Characteristics
- Not real-time
- Daily summary of previous day's incidents

### Solution
- Load dataset into SPICE
- Schedule daily refresh
- Disable direct query

### Result
- Eliminated real-time DB pressure
- Stable performance

---

## 3️⃣ Database-Level Tuning

- Index optimization
- Execution plan analysis
- Join restructuring
- View-based modeling
- Query cost reduction

</details>

---

<details>
<summary><strong>🔐 Embedded Reporting Flow & Multi-Tenant Design</strong></summary>

## Embedded Reporting Flow

1. Reports created in QuickSight
2. Dashboards embedded via Iframe
3. .NET API + Lambda generate secure embed URLs
4. Role-based permission enforced
5. Multi-tenant isolation maintained

## Authorization & Multi-Tenant Design

- API-level permission enforcement
- Parameter-based filtering
- School-level & District-level scope separation
- Secure embedded access

</details>

---

<details>
<summary><strong>💰 Cost Optimization Strategy</strong></summary>

## Problem

QuickSight charges:
- **$0.50 per session**

With tens of thousands of daily opens:
- Potential cost: Thousands of dollars per day

---

## Snapshot Rendering Architecture

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Serve Snapshot (No Session Cost)
   └─ No
        ↓
   Headless Browser Render
        ↓
   Apply Parameters
        ↓
   Capture Snapshot
        ↓
   Store Snapshot (S3)
        ↓
   Serve Snapshot
```

Cost Optimization Details: 
[![Cost Optimization](https://img.shields.io/badge/Docs-Cost%20Optimization-2ea44f?style=for-the-badge)](./COST_OPTIMIZATION.md)
- <img width="600" height="400" alt="Screenshot 2026-02-19 at 23 49 28" src="https://github.com/user-attachments/assets/559fa96e-7fd8-4795-b7f1-75e49595aa4d" />

---

### Result

- Reduced session creation dramatically
- Lowered cost to approximately 1/10 of projected usage
- Maintains visual equivalence for non-interactive reports

</details>

---

<details>
<summary><strong>🧩 Technical Decision: Why QuickSight?</strong></summary>

## Context
- Client previously used Power BI
- Company cloud platform is AWS

## Evaluation Summary

| Factor | Decision |
|--------|----------|
| Cloud Alignment | AWS-native preferred |
| Report Count (~30) | Fully achievable in QuickSight |
| UI Customization | Power BI stronger but not required |
| Cost Model | Snapshot logic mitigates session cost |

### Final Decision
QuickSight selected as AWS-native BI replacement.

</details>

---

## Impact

- Reduced projected reporting cost by ~90%
- Maintains ~3s response time
- 2,000+ schools supported
- Thousands of users
- Multi-tenant architecture
- Index tuning + View-based modeling
- Cloud-native BI architecture

---

## 🏁 Conclusion

This project represents more than a dashboard system.

It is a:

- Production-grade AWS BI platform
- Cost-optimized embedded analytics architecture
- Multi-tenant secure reporting system
- Performance-tuned database-driven analytics solution

Built with full-stack engineering discipline and cloud-native architecture principles.
