# AWS BI Reporting System Project  
**AWS QuickSight–Driven Analytics & Reporting Platform**

[한국어 🇰🇷](README.ko.md)
---

## Overview

This project demonstrates a **production-grade AWS BI reporting platform** built using **Amazon QuickSight**, designed for large-scale educational environments.

The platform delivers **30+ interactive reports** for operational monitoring, attendance tracking, health management, and district-level analytics — fully embedded into a web application.

It is engineered with strong emphasis on:

- Performance optimization
- Cost efficiency
- Multi-tenant authorization
- Scalable AWS-native architecture
- Database-level tuning

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

# Tech Stack

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

# System Scale

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

---

## Usage & Traffic

- Daily traffic: Thousands ~ Tens of thousands report views
- Average response time: ~3 seconds
- Total reports: ~30
- Visualization types: ~10 (Table, Insight, Graph, KPI, Tabs)
- Users:
  - ~2,000 schools
  - Thousands of administrative staff

---

# Report Categories

## 1️⃣ Attendance & Access Management
- Student attendance tracking
- Staff check-in / check-out logs
- Late arrival monitoring
- Tardiness analytics

---

## 2️⃣ School Health & Disease Monitoring
- Vaccination reporting
- Infection tracking
- Facility exposure logging
- Aggregation by school, district, date range

---

## 3️⃣ District & School-Level Summary Reports
- Graph-based dashboards
- Aggregated statistics
- Role-based visibility:
  - School users → Access only their school
  - District users → Access all schools

---

# Data Modeling Strategy

## Complex Logic
- Implemented as **Database Views in RDS**
- Reduces dataset complexity
- Improves QuickSight performance

## Simple Calculations
- Implemented using QuickSight Calculated Fields

## Custom SQL Instead of DAX
- Heavy aggregation handled at DB layer
- Visualization layer kept lightweight

---

# Performance Optimization

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

---

# Embedded Reporting Flow

1. Reports created in QuickSight
2. Dashboards embedded via Iframe
3. .NET API + Lambda generate secure embed URLs
4. Role-based permission enforced
5. Multi-tenant isolation maintained

---

# Authorization & Multi-Tenant Design

- API-level permission enforcement
- Parameter-based filtering
- School-level & District-level scope separation
- Secure embedded access

---

# Cost Optimization Strategy

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
---
 Cost Optimization Details
   - [![Cost Optimization](https://img.shields.io/badge/Docs-Cost%20Optimization-2ea44f?style=for-the-badge)](./COST_OPTIMIZATION.md)

<img width="600" height="400" alt="Screenshot 2026-02-19 at 23 49 28" src="https://github.com/user-attachments/assets/559fa96e-7fd8-4795-b7f1-75e49595aa4d" />

### Result

- Reduced session creation dramatically
- Lowered cost to approximately 1/10 of projected usage
- Maintains visual equivalence for non-interactive reports

---

# Technical Decision: Why QuickSight?

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

---

# Impact

## Cost Reduction
- Reduced projected reporting cost by ~90%

## Performance
- Maintains ~3s response time
- Handles hundreds of thousands of records

## Scalability
- 2,000+ schools supported
- Thousands of users
- Multi-tenant architecture

## Engineering Depth
- Index tuning
- View-based modeling
- SPICE optimization
- Snapshot caching
- RDS-DynamoDB synchronization
- Role-based embedded security

---

# Conclusion

This project represents more than a dashboard system.

It is a:

- Production-grade AWS BI platform
- Cost-optimized embedded analytics architecture
- Multi-tenant secure reporting system
- Performance-tuned database-driven analytics solution

Built with full-stack engineering discipline and cloud-native architecture principles.
