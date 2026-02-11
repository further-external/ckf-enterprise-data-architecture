# Strategic Plan: Next-Generation Data & ROI
**Initiative:** Project iOS 26 | Server-Side GTM Migration  
**Lead Architect:** Jaime Herrera  

## 🚨 The Challenge: A Critical Business Risk
Our current digital measurement setup relies exclusively on client-side (browser-based) tagging. This creates a "War on Two Fronts":

**1. The External Threat (Apple LTP & Signal Loss)**
Analysis shows that iOS users are our most valuable audience segment:
* `www.comfortkeepers.com`: iOS devices drive **37.1%** of all key conversions.
* `www.comfortkeepers.jobs`: iOS devices drive **53.1%** of all key conversions.

Apple's Link Tracking Protection (LTP) actively strips ad click IDs (`gclid`, `fbclid`). This creates inaccurate reporting, wastes Google/Meta AI ad spend, and breaks our view of the user journey.

**2. The Internal Threat (Agency Shadow IT)**
Currently, 10+ autonomous agencies deploy unverified pixels directly to our site. This causes site bloat, compliance liabilities (bypassing OneTrust), and data leakage.

## 💡 The Solution: Data Sovereignty Architecture
We are transitioning to a **Server-Side Architecture**—a "One-Way Pipe."

Instead of the user's browser sending 10+ public data requests to different vendors, the browser sends **one single secure stream** to our Google Cloud Run server (`analytics.comfortkeepers.com`). 

Our server acts as a "Multiplexer," automatically hashing PII, restoring lost iOS click IDs, and securely distributing the data to Meta CAPI, Google Ads, and BigQuery.

## 🔄 Phase 2: The New Agency Workflow
Agencies will **no longer deploy Custom HTML pixels**. 
1. **Request:** The agency requests a data feed (e.g., Bing Ads).
2. **Feeder:** They configure a simple, standardized GA4 event (`agency_conversion`).
3. **Routing:** Our central server "catches" the event, checks the `agency_id`, and routes it via API to their specific vendor.

**Outcomes:** Zero site speed impact, 100% consent compliance, and complete Data Sovereignty for Comfort Keepers.