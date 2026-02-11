# 🏛️ Enterprise Data Sovereignty Architecture
**Comfort Keepers (CKF) | sGTM & Data Layer Monorepo**

Welcome to the central repository for the Comfort Keepers Enterprise Data Architecture. This repository houses the code, infrastructure schemas, and governance policies for our Server-Side Tagging (sGTM) and Event-Driven Data Layer.



## 📖 How to Use This Repository

This repository is designed to be read sequentially, mirroring the lifecycle of our data supply chain—from the user's browser (Input), to the cloud server (Processing), and finally to the business strategy (Governance).

### 📂 Directory Guide

* **[STRATEGY.md](./STRATEGY.md)**
  * **Read this first.** It explains *why* this architecture exists (combating Apple's 37% iOS signal loss and managing agency "Shadow IT").
  
* **📁 [01-data-layer-specs](./01-data-layer-specs/) (The Input)**
  * **For Web Developers.** Contains the required JSON schemas and JavaScript specifications for frontend event tracking. 
  * *Critical:* Look here for the `user_data` PII schemas required for server-side hashing, and the Server-to-Server Measurement Protocol webhooks from Franconnect.

* **📁 [02-infrastructure-setup](./02-infrastructure-setup/) (The Hardware)**
  * **For Cloud Engineers.** Explains the Google Cloud Run autoscaling configuration, Load Balancer setup (`analytics.comfortkeepers.com`), and our FinOps Log Exclusion rules that keep server costs under $100/mo.

* **📁 [03-container-logic](./03-container-logic/) (The Software)**
  * **For Analytics Architects.** Contains the exported JSON files for our GTM environments. 
  * It explains the **Multiplexer Logic**: How we replaced 50+ client-side agency pixels with a single server-side routing table.

* **📁 [04-governance-testing](./04-governance-testing/) (The Quality Assurance)**
  * **For QA and Stakeholders.** Contains our R²IV (Request, Review, Implement, Validate) deployment plans, Vendor Capability Matrices, and the 4 strict QA Scenarios (A-D) required before any code goes to production.

## 📞 Support & Contacts
**Lead Architect:** Jaime Herrera  
**Environment:** Production / Google Cloud Platform  
**Documentation Status:** Up-to-Date (Phase 1 & Phase 2)