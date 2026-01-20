# <img src="https://github.com/user-attachments/assets/cf684a77-bb13-475a-9cba-b639ece52c8e" width="30" style="vertical-align: middle;" />  AI Powered Healthcare Operations Design & Automation Using n8n


An **AI-driven, event-based healthcare operations workflow** built in **n8n** that automates **insurance verification and pre-authorization** — one of the most manual, delay-prone processes in hospital revenue cycle management.
This system mirrors **real hospital pre-authorization pipelines**, combining scheduling data, insurance eligibility checks, AI-generated clinical justification, automated submissions, status tracking, alerts, and analytics — all orchestrated visually in **n8n**.


## <img src="https://github.com/user-attachments/assets/f3dcee8e-e008-457a-97fb-d3848b425713" height="30px" style="vertical-align:text-bottom;"> Repository Structure
```bash
 AI Powered Healthcare Operations Design & Automation Using n8n
├── 📁 workflow
├── 📁 prompts 
├── 📁 docs
├── 📁 screenshots
└── README.md
```

## <img src="https://github.com/user-attachments/assets/1e084ddf-423a-4ce8-8853-d917c0a96579" height="28px" style="vertical-align:text-bottom;"> Problem Statement

In hospitals, insurance pre-authorization for procedures such as **MRIs, CT scans, and surgeries** is one of the most inefficient workflows.
### Observed Issues
- Manual review of upcoming appointments  
- Separate logins for eligibility and payer portals  
- Clinical justification written manually for each request  
- Status tracking via Excel sheets and follow-up calls  
- High denial rates due to missing or weak documentation  
- Staff burnout and delayed patient care  

**Root Cause:**  
Manual effort + fragmented systems + no intelligence layer

##  <img src="https://github.com/user-attachments/assets/d3a7713c-0fa3-4fba-a8b3-1cb60e4dafce"  height="25px" style="position: bottom;"> Objective
To design an **autonomous, safe, and auditable workflow** that:

- Reacts to scheduled clinical events  
- Applies insurance logic automatically  
- Uses AI to generate medical justifications  
- Reduces human involvement to **exceptions only**  
- Produces analytics for **operational insight**

## 🏥 What This System Automatically Does ?

- Runs every **15 minutes**
- Fetches **upcoming scheduled procedures**
- Extracts **patient, insurance, and procedure details**
- Checks **insurance eligibility** in real time
- Determines whether **pre-authorization is required**
- Uses **AI to generate structured pre-auth requests**
- Submits requests to **insurance portals**
- Tracks **approval / denial status**
- Alerts staff **only when action is needed**
- Sends **patient notifications**
- Calculates and stores **operational analytics**


## 🔁 End-to-End Workflow (As Built)

This workflow executes a **complete insurance pre-authorization lifecycle**:
```bash
Schedule → Verify → Decide → Generate → Submit → Track → Notify → Analyze
```
---

## <img src="https://github.com/user-attachments/assets/6672ee8c-15ed-4fb5-9cd5-63c04ac747c1" height="24px" style="vertical-align:bottom;">  System Architecture 
https://github.com/user-attachments/assets/1835ed0c-7c26-4740-bcb7-bb5d4f84a5b1


### 1️⃣ Scheduler — Every 15 Minutes
**Node:** Schedule - Every 15 Minutes  
- Triggers the workflow periodically  
- Ensures upcoming appointments are proactively processed

### 2️⃣ Workflow Configuration
**Node:** Workflow Configuration  
Centralized configuration for:
- Scheduling system API  
- Insurance eligibility API  
- Pre-authorization portal API  
Ensures the workflow is **environment-agnostic**
<img width="928" height="383" alt="image" src="https://github.com/user-attachments/assets/4fee2b34-76a9-4fe3-ae5c-74e31e0cf049" />


### 3️⃣ Fetch Scheduled Appointments
**Node:** Fetch Scheduled Appointments  
Retrieves:
- Patient ID & name  
- Procedure codes (CPT)  
- Diagnosis codes (ICD)  
- Insurance provider  
- Appointment date  

### 4️⃣ Extract Patient & Procedure Data
**Node:** Extract Patient & Procedure Data  
- Normalizes appointment data  
- Prepares inputs for eligibility and policy checks  

### 5️⃣ Check Insurance Eligibility
**Node:** Check Insurance Eligibility API  
Validates:
- Active coverage  
- Plan validity  
- Basic procedure coverage  
Acts as the **first decision gate**

### 6️⃣ Pre-Auth Required?
**Node:** Is Pre-Auth Required?  
Decision based on:
- Insurance provider
- Procedure risk level  
- Policy logic  
- **NO** → Log and exit  
- **YES** → Continue to AI generation

  <img width="937" height="394" alt="image" src="https://github.com/user-attachments/assets/bd190b1a-b501-480c-8b51-832ea3076794" />
### 7️⃣ AI Pre-Authorization Request Generation
**Node:** AI Pre-Auth Request  
Uses **OpenAI GPT-4** to generate:
- Clinical justification  
- Medical necessity statement  
- Urgency classification  
**Output Node:** Structured Pre-Auth Output  
- Ensures consistent, portal-ready JSON formatting
  
### 8️⃣ Submit Pre-Auth to Insurance Portal
**Node:** Submit Pre-Auth to Insurance Portal  
- Submits via HTTP API  
- Supports API-based and RPA-style payer portals


### 9️⃣ Log Pre-Auth Request to Database
**Node:** Log Pre-Auth Request to Database  
Stored in **PostgreSQL**:
- Request ID  
- Patient  
- Insurance  
- Procedure  
- Submission timestamp  
Creates a **fully auditable trail**

<img width="357" height="302" alt="image" src="https://github.com/user-attachments/assets/52b64617-e680-408b-b3e7-fe29f88eabe0" />


### 🔟 Wait for Insurance Response
**Node:** Wait for Insurance Response  
- Introduces controlled delay  
- Prevents unnecessary polling  

### ⏸️ Check Pre-Auth Status
**Node:** Check Pre-Auth Status  
- Queries insurance portal for current status

### 1️⃣2️⃣ Approval Decision Routing
**Node:** Check Approval Status  
#### ✅ Approved
- **Node:** Update Status - Approved  
- Updates database  
- Proceeds to analytics  

### ❌ Denied / ⏳ Pending
- Update Status - Denied/Pending  
- Alert Staff - Denial / Action Required  
- Sends Slack alert with full context  
- Logs and exits cleanly  

### 1️⃣3️⃣ Patient Notification
**Node:** Notify Patient via Automated Email  
- Sends real-time status updates  
- Improves transparency and patient experience  

### 1️⃣4️⃣ Analytics & Metrics
**Nodes:**
- Calculate Analytics Metrics  
- Store Analytics Data  
Tracks:
- Approval & denial rates  
- Processing time  
- Time saved  
- High-volume procedures  
- Insurance performance  

## 📊 Impact

| Metric | Before | After |
|------|-------|------|
| Time per request | 45–60 mins | ~3–8 mins |
| Approval turnaround | 5–7 days | < 24 hrs |
| Manual errors | High | ~80% reduction |
| Staff workload | Repetitive admin | Exception handling |
| Visibility | None | Real-time analytics |

---

## <img src="https://github.com/user-attachments/assets/612137fd-b2de-411c-acd7-f94c4811e9f2" height="30px" style="vertical-align:text-bottom;"> Tech Stack

- **n8n** – Workflow orchestration  
- **OpenAI GPT-4** – Clinical reasoning & generation  
- **HTTP APIs** – Scheduling & insurance systems  
- **PostgreSQL** – Request & analytics storage  
- **Slack** – Human-in-the-loop alerts  
- **Email (SMTP / Gmail)** – Patient communication  


## <img src="https://github.com/user-attachments/assets/dcdcffb4-c4e2-40ee-84cc-aca8612d257e" height="30px" style="vertical-align: text-bottom; margin-bottom:-3050px;"> Key Takeaways

- Designing **event-driven healthcare workflows**
- Applying AI to **operational decision-making**, not just text
- Building **safe automation** with human escalation
- Translating real hospital pain points into system design
- Creating **analytics-driven healthcare operations**


## <img src="https://github.com/user-attachments/assets/cf684a77-bb13-475a-9cba-b639ece52c8e" width="35" style="vertical-align: middle;" /> AI Usage Disclosure

AI tools were used as **design collaborators** to support this project in the following ways:

- Assisting with **clinical text generation** for insurance pre-authorization requests  
- Helping structure **medical necessity justifications** and urgency classifications  
- Validating **workflow logic**, edge cases, and safety constraints  
- Improving documentation clarity and technical articulation  

All **core system design and implementation decisions**, including:

- Workflow architecture  
- n8n node selection and sequencing  
- Business logic and routing rules  
- Integration strategy (EHR, insurance, database, alerts)  
- Error handling and human-in-the-loop safeguards  

were **designed, implemented, and configured manually** using n8n.

AI was used to **augment reasoning and productivity**, not to autonomously design, deploy, or operate the system.


## <img src="https://github.com/user-attachments/assets/64abffeb-9a67-4e47-a3ec-69036aa3a343" height="25px" style="position: bottom;"> Disclaimer

- No real patient data is included  
- Credentials and API keys are not committed  
- External systems are mocked for demonstration  
- Built strictly for **educational and architectural demonstration purposes**

---
