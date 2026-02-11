# 🚀 V6 Phase 1: Server-Side GTM Implementation Master Plan
**Document:** FTR | JH | CKF sGTM | V6 Phase 1  
**Status:** 🟢 Code Complete / Ready for QA  
**Target Completion:** End of Sprint 4 (Friday)  

## 🎯 Executive Goals (OKRs)
**Goal:** Reach "Feature Complete" status and prepare for full production validation.

* **OKR 1:** Finalize Use Case Documentation (Due: Tomorrow).
* **OKR 2:** Build and Document Test Suites (Due: Monday).
* **OKR 3:** Execute End-to-End QA with validated Data Layer (Target: Next Week).
* **OKR 4:** Execute "Safe Launch" Publishing Sequence (Target: Post-QA Green Light).

---

## 🏗️ Part 1: Implementation Status Report

### 1. Infrastructure (Foundations)
| Component | Status | Notes |
| :--- | :--- | :--- |
| **Cloud Run Service** | ✅ Active | Autoscaling enabled. Mapped to `analytics.comfortkeepers.com`. |
| **Load Balancer** | ✅ Active | SSL secured. Health check passing. |
| **LTP Fix** | ✅ Built | `QR` logic configured for iOS Signal Restoration. |
| **Cost Governance** | ✅ Done | Log Sink created to drop `200 OK` logs (Verified). |

### 2. Marketing Zone (Web GTM)
| Component | Status | Notes |
| :--- | :--- | :--- |
| **Feeder Pipeline** | ✅ Done | GA4 Config mapped to Server URL. |
| **Agency Cleanup** | ✅ Done | Redundant StackAdapt pixels removed. |
| **Tag Migration** | ✅ Done | Moved Clarity & Listeners from Parent/Analytics to Marketing Zone. |
| **Data Layer** | 🟡 Pending | Dependency: Devs to push `user_data` (Email/Phone) update by Monday. |

### 3. Server Container (Vendor Tags)
| Vendor | Status | Logic / Action Required |
| :--- | :--- | :--- |
| **Meta (CAPI)** | ✅ Ready | Maps `generate_lead` → Lead. Auto-hashes PII. |
| **StackAdapt** | ✅ Ready | Universal Conversion Tag logic applied. |
| **Floodlight** | ✅ Ready | Deduplication keys mapping correctly. |
| **BigQuery** | ✅ Ready | All event stream backup active. |

---

## 🛡️ Part 2: QA Validation Scenarios

* **Scenario A: Data Layer Validation**
  * Verify `user_data` array is populated with raw PII on conversions.
* **Scenario B: Infrastructure Integrity**
  * Verify `analytics.comfortkeepers.com` responds with HTTP `200 OK`.
  * Ensure 1st-party cookies are set as `HttpOnly`.
* **Scenario C: Vendor Accuracy**
  * Verify `event_id` matches exactly between Browser and Server for deduplication.
* **Scenario D: Negative Testing**
  * Goal: Ensure rejected/blocked tags DO NOT fire (e.g., Invoca, Bing WAF limits).

---

## 📅 Part 3: The "Safe Launch" Deployment Strategy (OKR 4)
*⚠️ Do not publish until Scenarios A-D are 100% Green.*

1. **Step 1: The "Server-First" Publish**
   * **Container:** Server Zone (`GTM-KDG6S98`)
   * **Action:** Publish Version.
   * **Risk:** Zero. The server will sit idle, ready to receive.
2. **Step 2: The "Web-Feeder" Publish**
   * **Container:** Marketing Zone (`GTM-N4W3MLB`)
   * **Action:** Publish Version.
   * **Impact:** The "On Switch." Traffic starts flowing to the server.
3. **Step 3: The "Agency-Off" Publish (Cleanup)**
   * **Action:** Pause/Remove legacy client-side agency pixels.