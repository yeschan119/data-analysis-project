# 💰 AWS QuickSight Cost Optimization & Snapshot Architecture

## Overview

AWS QuickSight charges **$0.50 per session creation**.  
In high-traffic environments, uncontrolled embedding can significantly increase operational cost.

This system implements:

- **Session-aware cost control logic**
- **Snapshot-based rendering architecture**
- **Auto-scaling distributed processing**
- **Reusable S3 caching strategy**

The goal is to maintain high-performance report delivery while keeping session usage within predictable budget limits.

---

# 🎯 Cost Optimization Goals

- Keep QuickSight sessions **≤ 1,000 per month**
- Avoid redundant session creation
- Maintain fast report access even after threshold
- Preserve functional parity for dynamic reports
- Eliminate idle infrastructure cost

---

# 🧠 Cost Optimization Logic (System-Level)

## ✅ Phase 1 — Standard Mode (≤ 1,000 Sessions / Month)

### Strategy
- Use QuickSight IFrame Embed directly
- Each interaction creates a QuickSight session
- Suitable for low to moderate traffic

### Characteristics
- Fully interactive dashboards
- Real-time filtering
- Acceptable cost within quota

---

## 🚨 Phase 2 — Optimized Mode (> 1,000 Sessions / Month)

When threshold is exceeded, the system dynamically switches to:

> **Snapshot-Based Rendering Mode**

Instead of generating new sessions for every request:
1. Render once
2. Capture snapshot
3. Store in S3
4. Reuse indefinitely

---

# 🏗 Snapshot-Based Cost Optimization Architecture

Two optimized report categories:

1. **Public Static Reports (High-Traffic)**
2. **Private Parameterized Reports (School / Region Specific)**

---

# 1️⃣ Public Static Reports (High-Traffic Dashboards)

<img width="600" height="400" alt="public-url" src="https://github.com/user-attachments/assets/0cddd163-c054-4754-a6e4-1fe719d99111" />

## 📌 Use Case

- Frequently accessed dashboards
- No user-specific filtering
- Same report requested repeatedly

---

## 🔄 End-to-End Processing Flow

### 1️⃣ System Trigger
- Scheduled job or event trigger starts snapshot workflow

### 2️⃣ Data Preparation
- Core datasets from **AWS RDS (MySQL)**
- Campus & snapshot metadata from **Amazon DynamoDB**

### 3️⃣ Parallel Job Orchestration
- **AWS Step Functions** coordinates workflow
- Campus-specific jobs pushed to **Amazon SQS**

### 4️⃣ Auto-Scaling Snapshot Workers
- **EC2 Auto Scaling Group** launches Node.js workers
- Each worker:
  - Opens QuickSight report via headless browser
  - Applies parameters
  - Renders report
  - Captures snapshot

### 5️⃣ Snapshot Storage
- Stored in **Amazon S3**
- Metadata indexed in DynamoDB for fast lookup

### 6️⃣ Scale Down
- EC2 instances terminate automatically
- No idle infrastructure cost

---

## 💰 Cost Impact

- Only one QuickSight session per scheduled generation
- Unlimited snapshot reuse
- Near-zero cost for repeated access

---

# 2️⃣ Private Reports (School / Region Specific)

<img width="600" height="500" alt="private-url" src="https://github.com/user-attachments/assets/618d0b70-1e36-481b-a779-669a60ccb1cf" />

## 📌 Use Case

Reports scoped by:
- School
- Region
- Role permissions
- User-selected filters

---

## 🔄 Request Flow

### 1️⃣ User Request
Angular UI → .NET API → Lambda trigger

---

### 2️⃣ Snapshot Metadata Check (DynamoDB)

Lambda checks whether snapshot exists for:

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

## 📊 Key Outcomes

- Significant reduction in QuickSight session cost
- High-performance report delivery
- Scales efficiently with user growth
- Maintains functional parity with interactive reports where needed

---

## 🧠 Design Considerations

- Snapshot cache invalidation strategy (time-based or data-change-based)
- Secure snapshot access aligned with user permissions
- Clear separation between **public static** and **private dynamic** reports

---

This approach ensures **cost efficiency without sacrificing usability**, making QuickSight viable at scale for large educational districts.
