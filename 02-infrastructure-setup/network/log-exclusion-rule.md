# Network & FinOps Configuration
### Cloud Logging & Cost Governance



## 1. Load Balancer & First-Party Context
* **Endpoint:** `analytics.comfortkeepers.com`
* **SSL/TLS:** Terminated at the Google Cloud HTTP(S) Load Balancer level.
* **Purpose:** Ensures the sGTM container operates in a First-Party context, allowing cookies to bypass Safari ITP (Intelligent Tracking Prevention) limits.

## 2. Cloud Logging Governance (FinOps)
Out-of-the-box, sGTM logs every HTTP request and system event to Google Cloud Logging. At CKF's enterprise traffic volume, this creates massive Cloud Storage bloat (~$1,500+/mo). 

To reduce operational expenses to **<$100/mo**, a strict Log Router configuration was implemented at the GCP project level.

### A. Baseline Inclusion Filter
To prevent noisy, high-volume GCP system and audit logs from being ingested and billed, the following base filter is applied to the default log sink:

```text
NOT LOG_ID("cloudaudit.googleapis.com/activity") 
AND NOT LOG_ID("externalaudit.googleapis.com/activity") 
AND NOT LOG_ID("cloudaudit.googleapis.com/system_event") 
AND NOT LOG_ID("externalaudit.googleapis.com/system_event") 
AND NOT LOG_ID("cloudaudit.googleapis.com/access_transparency") 
AND NOT LOG_ID("externalaudit.googleapis.com/access_transparency")
```

### B. Success Log Exclusion Rules
We retain 100% of 4xx and 5xx error logs for debugging, but actively drop successful ping and HTTP requests at the edge using two distinct exclusion rules:

* **Rule 1:**  Exclude Standard Success Logs (exclude-success-logs) Drops all standard requests that result in a successful HTTP status code.

```text
NOT (LOG_ID("requests") AND httpRequest.status < 400)
```

* **Rule 2:**  Exclude Cloud Run 200s (Exclude-Success-200) Specifically targets and drops successful telemetry and load-balancer health checks hitting the Cloud Run revision.

```text
resource.type="cloud_run_revision"
httpRequest.status=200
```
