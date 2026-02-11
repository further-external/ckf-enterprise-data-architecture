```markdown
# Cloud Run Provisioning Guide
### Server-Side GTM Infrastructure Runbook



This document outlines the exact specifications required to provision the Server-Side Tag Manager environment in Google Cloud Platform. This ensures the environment can be accurately reproduced for disaster recovery or geographic expansion.

## 1. Container Configuration
The environment relies on the official Docker image provided by Google.

* **Service Name:** `ckf-sgtm-multiplexer`
* **Region:** `us-central1` (Standard deployment)
* **Container Image URL:** `gcr.io/cloud-tagmanager/sgtm:latest`

## 2. Environment Variables
To link the Cloud Run instance to the GTM interface, the following environment variable must be passed to the container upon deployment.

* **Name:** `CONTAINER_CONFIG`
* **Value:** *(Stored securely in GCP Secret Manager. Extracted from the GTM Server Container interface).*

## 3. Compute & Scaling Parameters
To ensure 99.9% availability during traffic spikes while governing baseline costs, the autoscaling parameters are strictly defined:

* **CPU Allocation:** CPU is only allocated during request processing (reduces idle costs).
* **Capacity:** * Memory: `512 MiB`
  * CPU: `1 vCPU`
* **Autoscaling:**
  * Minimum instances: `3` (Ensures High Availability and prevents cold-start latency).
  * Maximum instances: `50` (Prevents runaway scaling costs during sustained bot traffic or DDoS events).
* **Maximum Concurrent Requests per Instance:** `80`

## 4. Security & Networking
* **Ingress:** Allow all traffic (Required for public web tracking).
* **Authentication:** Allow unauthenticated invocations (The container must accept payloads from anonymous web visitors).