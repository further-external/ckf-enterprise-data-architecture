# Server-Side Transformations

This architecture relies heavily on native Server-Side GTM Transformations to enforce Data Governance and Signal Restoration at the edge, prior to tag execution.

### 1. Signal Restoration (LTP Fix)
**Name:** `T | Augment - Restore Click IDs`
**Type:** Augment Event
**Logic:** Intercepts the `page_location` parameter from the `GA4 | Client`. It utilizes a Query Replacer variable (`QR | Restore Click IDs`) to map 1st-party cookie values back into the URL string to bypass iOS 17 Link Tracking Protection:
* `c_gci` -> `gclid`
* `c_wbr` -> `wbraid`
* `c_fbclid` -> `fbclid`

### 2. PII Redaction
**Name:** `T | Exclude parameters - Redact PII`
**Type:** Exclude Parameters
**Logic:** Acts as a strict firewall. It explicitly drops the following keys from the event data object: `email`, `phone`, `first_name`, `last_name`, `street`, `postcode`, `city`, `region`. This guarantees that plain-text PII cannot be accidentally forwarded to downstream analytics or data warehousing tools (like BigQuery).