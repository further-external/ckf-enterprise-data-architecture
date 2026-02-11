# Cloud Infrastructure & Cost Governance
### Catalyst Architecture | Hardware & Network

This directory outlines the Google Cloud Platform (GCP) infrastructure required to host the Server-Side GTM (sGTM) engine.



## 🏗️ Core Architecture
To bypass Safari ITP (Intelligent Tracking Prevention) and Ad Blockers, the server must operate in a **First-Party Context**.

* **Service:** Google Cloud Run (Containerized sGTM Engine)
* **Scaling:** Autoscaling enabled (Min: 3 instances for 99.9% High Availability, Max: 50 instances for traffic spikes).
* **Load Balancer:** Configured with a custom First-Party Domain (`analytics.comfortkeepers.com`).
* **Security:** SSL Termination handled at the Load Balancer level.

## 💰 FinOps & Cost Governance (The $81/mo Solution)
**The Problem:** Out-of-the-box sGTM logs every single HTTP request to Google Cloud Logging. At enterprise traffic levels, logging millions of "200 OK" success hits can cost over $1,500/month in useless storage fees.

**The Solution:** We implemented a **Log Exclusion Sink** in GCP.
* **Rule:** Filter out and drop 90% of `200 OK` success logs.
* **Exceptions:** Retain 100% of `4xx` and `5xx` error logs for debugging.
* **Result:** Operational cloud costs were reduced to **~$81.00/month** (Compute: $41.48, Networking/Egress: $25.12, BigQuery Storage: $14.60).

## 🛡️ Edge Privacy & Signal Restoration
Because this infrastructure sits entirely under our control, it executes two critical functions before data ever reaches a vendor:
1. **SHA-256 PII Hashing:** The Custom Privacy Module hashes all user data (Email, Phone, Address) at the edge.
2. **iOS Signal Restoration:** Intercepts HTTP request headers to map `c_gclid` and `c_fbclid` back into outgoing requests, effectively restoring the **37% signal loss** caused by Apple's Link Tracking Protection (LTP).