# AWS BI Reporting System  
### Production-Grade Embedded Analytics Platform (AWS QuickSight)

[한국어 🇰🇷](README.ko.md)

---

## 🚀 Executive Summary

Built a **scalable AWS-native BI platform** supporting:

- 🏫 2,000+ schools
- 👥 Thousands of users
- 📊 30+ interactive reports
- 🗂 Hundreds of thousands of incident records
- ⚡ ~3s average response time
- 💰 ~90% cost reduction via session optimization

Focused on **Performance, Cost Efficiency, Multi-Tenant Security, and Database Optimization.**

---

## 🏗 Architecture (Simplified)

```
Angular → .NET API → Lambda → QuickSight
        ← Secure Embed URL ←
```

- Fully embedded analytics
- Role-based access control
- Multi-tenant isolation
- Snapshot-based cost optimization

---

## 🛠 Tech Stack

**Frontend:** Angular  
**Backend:** ASP.NET Core (.NET, C#)  
**BI:** Amazon QuickSight  
**Database:** AWS RDS (MySQL)  
**Metadata:** DynamoDB  
**Infra:** Lambda, Step Functions, SQS, EC2  
**Storage:** S3  
**CI/CD:** Azure DevOps  

---

## 📈 Scale

| Metric | Value |
|--------|-------|
| Data Period | 5 years (COVID onward) |
| Records | Hundreds of thousands |
| Reports | ~30 |
| Visual Types | ~10 |
| Daily Traffic | Thousands ~ Tens of thousands |
| Avg Response | ~3 seconds |

---

## 💡 Key Engineering Decisions

### 1️⃣ Database-First Modeling
- Complex joins handled via RDS Views
- Custom SQL instead of BI-layer heavy logic
- Index & execution plan tuning

### 2️⃣ Performance Optimization
- District-level heavy queries optimized
- School metadata offloaded to DynamoDB
- RDS ↔ DynamoDB sync architecture
- SPICE used for non-real-time reports

### 3️⃣ Cost Optimization (Critical)

QuickSight pricing:
- **$0.50 per session**

Projected cost under heavy usage:
- Thousands per day

**Solution:** Snapshot Rendering Strategy

```
Check Snapshot → Render if needed → Store → Reuse
```

✔ Reduced cost by ~90%  
✔ Maintained performance  
✔ Scalable embedded analytics  

 Cost Optimization Details:
        -> [![Cost Optimization](https://img.shields.io/badge/Docs-Cost%20Optimization-2ea44f?style=for-the-badge)](./COST_OPTIMIZATION.md)

- <img width="600" height="400" alt="Screenshot 2026-02-19 at 23 49 28" src="https://github.com/user-attachments/assets/559fa96e-7fd8-4795-b7f1-75e49595aa4d" />

---

## 📊 Report Scope

- Attendance & Access Management
- School Health & Infection Monitoring
- District-Level Summary Dashboards
- Role-Based Data Visibility

---

## 🎯 Impact

- AWS-native replacement for Power BI
- Enterprise-grade embedded BI architecture
- Multi-tenant secure reporting system
- Database performance tuning at scale
- Cost-aware cloud system design

---

<details>
<summary>🔍 Detailed Architecture & Flow (Click to Expand)</summary>

### Embedded Flow

1. Reports created in QuickSight
2. .NET + Lambda generate secure embed URL
3. Angular renders via Iframe
4. Permission scope applied
5. Snapshot cache reduces session usage

### Snapshot Architecture

```
User Click
   ↓
Snapshot Exists?
   ├─ Yes → Serve (No Session Cost)
   └─ No
        ↓
   Headless Render
        ↓
   Capture
        ↓
   Store in S3
        ↓
   Serve
```

</details>

---

## 🏁 Conclusion

This project demonstrates:

- Production-grade AWS BI engineering
- Cost-optimized embedded analytics
- Multi-tenant architecture
- Database performance tuning
- Scalable cloud-native system design
