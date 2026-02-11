# Container Logic & The Multiplexer Architecture
### Catalyst sGTM Enterprise Standard

This directory contains the exported JSON configurations for the Google Tag Manager (GTM) ecosystem. We have shifted from a decentralized **"Many-to-Many"** client-side model to a centralized **"One-to-Many"** server-side model.



## 📡 1. The "Feeders" (Client-Side Zones)
**Goal:** Deprecate "Shadow IT" and establish a single first-party data pipeline.

**The Architectural Refactor:**
1. **Tag Cleanup:** Hundreds of legacy third-party tags (Meta, Campaign Manager) were paused and renamed to `ZZZ - ...`.
2. **The Server Feeders:** We established dedicated transport tags (e.g., `3PT | Google | Master Form Tag`) that route directly to `G-SERVERFEEDER` via our first-party endpoint (`analytics.comfortkeepers.com`).
3. **Payload Enrichment:** The feeders capture the Data Layer schemas (including unhashed PII) and pass them securely over HTTPS to the server.

## 🧠 2. The "Multiplexer" (Server-Side Core)
**Goal:** Route generic data to specific vendor destinations dynamically, supporting a Multi-Tenant business structure (Corporate, Franchise, Recruiting).

### Multi-Tenant Hostname Routing



The Server Container (`GTM-KDG6S98`) acts as a switchboard. We use **Lookup Tables** tied to the `Page Location` to dynamically swap API tokens and Pixel IDs.
* *If Hostname = `www.comfortkeepers.com` -> Route to NBF Meta Pixel.*
* *If Hostname = `www.comfortkeepersfranchise.com` -> Route to FRANDEV Meta Pixel.*
* *If Hostname = `www.comfortkeepers.jobs` -> Route to StackAdapt Jobs Conversion ID.*

### Enterprise Transformations (Privacy & LTP)
Instead of fragile custom code, we utilize native GTM Transformations to manipulate the data stream before it reaches vendor tags:
1. **PII Redaction (`T | Exclude parameters - Redact PII`):** Strictly strips raw parameters (`email`, `phone`, `first_name`, `street`) from the stream, ensuring raw PII never leaks into BigQuery backups. (Hashing is handled natively by the Meta/StackAdapt templates).
2. **iOS Signal Restoration (`T | Augment - Restore Click IDs`):** Combats Apple LTP by intercepting the event stream and replacing stripped URLs with 1st-party cookie values (mapping `c_gci` to `gclid` and `c_fbclid` to `fbclid`).