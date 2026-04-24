# Collections KPI Dashboard & Analytics Project

## 📊 Project Overview
This project simulates a real-world **collections analytics environment**, focusing on tracking key performance indicators (KPIs) and analyzing account-level and operational data.

The goal is to demonstrate how data can be used to monitor collections performance, identify inefficiencies, and support data-driven decision-making in a financial or operations setting.

---

## 🧩 Dataset Structure

The dataset is structured using a relational model with two main tables:

### 1. Accounts Table
- Customer_ID  
- Balance  
- Days_Past_Due  
- Segment (Low / Medium / High risk)  
- Aging_Bucket (0–30, 31–60, 61–90, 90+)  
- Next_Bucket  
- Rolled_Forward (1 = account moved to worse delinquency)

### 2. Contact Activity Table
- Customer_ID  
- Contacted (Yes/No)  
- Call_Attempts  
- Contact_Date  
- Promise_to_Pay (PTP)  
- Paid  
- Agent_ID  

These tables are linked using **Customer_ID**, enabling relational analysis through SQL joins.

---

## 🔗 Data Modeling

Data was split into separate tables to reflect real-world database structures.  
SQL joins were used to combine account-level and activity-level data for KPI analysis.

Example:
```sql
SELECT
	ca.Agent_ID,
	a.Aging_Bucket, 
	COUNT(*) AS Total_Accounts,
	AVG(ca.Call_Attempts) AS AVG_Call_Attemps,
	COUNT(CASE WHEN ca.Contacted = 'Yes' THEN 1 END) * 1.0 / COUNT(*) AS Contact_Rate,
	SUM(a.Rolled_Forward) * 1.0 / COUNT(*) AS Roll_Rate
FROM contact_activity ca 
	JOIN accounts a 
	ON ca.Customer_ID = a.Customer_ID
GROUP BY 1,2
ORDER BY 1;
