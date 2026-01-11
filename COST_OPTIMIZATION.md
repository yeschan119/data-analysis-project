# 💰 Cost Optimization Logic  
**AWS QuickSight Session Cost Control Strategy**

AWS QuickSight charges **$0.50 per session creation**.  
To control operational cost while maintaining fast report access, this project implements a **multi-tier session optimization and snapshot caching strategy**.

---

## 🎯 Cost Optimization Goal

- Keep QuickSight session usage **within 1,000 sessions per month**
- Avoid unnecessary session creation for frequently accessed reports
- Provide fast report rendering even after session limits are exceeded

---

## ✅ Phase 1: Standard Usage (≤ 1,000 Sessions / Month)

### Strategy
- Use **QuickSight IFrame Embed** directly
- Each user interaction creates a QuickSight session
- Suitable for low to moderate traffic

### Characteristics
- Real-time interactive dashboards
- Full QuickSight functionality
- Acceptable cost within monthly session quota

---

## 🚨 Phase 2: Optimized Usage (> 1,000 Sessions / Month)

Once the session threshold is exceeded, the system dynamically switches to **snapshot-based rendering**.

---

## 1️⃣ Public Static Reports (High-Traffic)

### Use Case
- Reports frequently accessed by many users
- Same report is clicked repeatedly with no user-specific filtering

### Optimization Logic
1. Generate the full report once in QuickSight
2. Render the report using a **headless browser**
3. Capture a **snapshot (image or HTML render)**
4. Store the snapshot in persistent storage (e.g., S3)
5. Serve snapshots directly to users

### Benefits
- Zero additional QuickSight sessions
- Extremely fast load times
- Ideal for dashboards and summary reports

---

## 2️⃣ Private Reports (School / Region Specific)

### Use Case
- Reports scoped by:
  - School
  - Region
  - User permissions
- Requires dynamic filtering

---

### Initial Access Flow

1. User clicks a private report
2. System checks for an existing snapshot
3. If not found:
   - Launch report in a **headless browser**
   - Apply required QuickSight parameters
   - Capture snapshot
   - Store snapshot
4. Snapshot is immediately returned to the user

---

### Reuse Logic

- If another user requests the **same report with identical parameters**:
  - Serve the pre-generated snapshot
  - No QuickSight session is created

---

### Dynamic Filter Handling

1. User modifies filters or conditions in the UI
2. Filter values are passed as **QuickSight parameters**
3. Headless browser renders the filtered report
4. A new snapshot is generated and stored
5. Snapshot is reused for subsequent identical requests

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
