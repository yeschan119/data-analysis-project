# 1️⃣ Public Static Reports Processing (High-Traffic)

<img width="600" height="450" alt="public-url" src="https://github.com/user-attachments/assets/0cddd163-c054-4754-a6e4-1fe719d99111" />

🔄 End-to-End Flow (Simplified)
	1.	System Trigger
	•	A scheduled or event-based trigger starts the snapshot workflow.
	2.	Data Preparation
	•	Required data is fetched from:
	•	AWS RDS (MySQL) for core datasets
	•	DynamoDB for campus and metadata information
	3.	Parallel Job Orchestration
	•	AWS Step Functions coordinate the entire process.
	•	Campus-specific jobs are pushed into SQS for parallel execution.
	4.	Auto-Scaling Snapshot Workers
	•	EC2 Auto Scaling Group spins up Node.js workers dynamically.
	•	Each worker:
	•	Opens a QuickSight report via a headless browser
	•	Applies parameters (school, region, filters)
	•	Renders the report
	•	Captures a snapshot
	5.	Snapshot Storage
	•	Generated snapshots are stored in Amazon S3
	•	Snapshots are indexed for fast reuse
	6.	Scale Down
	•	Once all snapshot jobs are completed:
	•	EC2 instances are automatically scaled down
	•	No idle infrastructure cost remains
---

# 2️⃣ Private Reports Processing (School / Region Specific)

<img width="500" height="400" alt="private-url" src="https://github.com/user-attachments/assets/618d0b70-1e36-481b-a779-669a60ccb1cf" />

🔄 High-Level Flow
	1.	User Requests a Private Report
	•	Request originates from the Angular UI
	•	Routed through .NET API to a Lambda trigger
	2.	Snapshot Metadata Check (DynamoDB)
	•	Lambda checks whether a snapshot already exists for:
	•	Report ID
	•	School / Region
	•	Applied filter parameters
	3.	Decision Branch
	•	Snapshot Exists & Ready
	•	Generate a pre-signed S3 URL
	•	Return snapshot immediately (no QuickSight session)
	•	Snapshot Missing or Failed
	•	Create or reset snapshot metadata
	•	Proceed to snapshot generation workflow

⸻

⚙ Snapshot Generation Workflow
	4.	Step Functions Orchestration
	•	Coordinates snapshot lifecycle
	•	Handles retries and failure states
	5.	EC2 Snapshot Worker (Node.js)
	•	Snapshot code is pre-deployed to EC2
	•	EC2 launches a headless browser
	•	Opens the QuickSight report
	•	Applies parameters:
	•	School
	•	Region
	•	User-selected filters
	6.	Snapshot Capture
	•	On success:
	•	Snapshot is stored in Amazon S3
	•	Metadata status updated to READY
	•	On failure:
	•	Metadata updated to FAILED

⸻

📦 Snapshot Delivery
	7.	S3 Pre-Signed URL
	•	.NET API receives the snapshot location
	•	Generates a secure, time-limited URL
	•	Snapshot is displayed instantly in the UI
