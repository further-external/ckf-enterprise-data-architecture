# Data Layer Event Schemas & Taxonomy

This directory contains the individual event specifications required to power the Comfort Keepers (CKF) Data Sovereignty Architecture.

## 📁 Directory Layout
* `/authentication`: Login, Logout, and Account Creation events.
* `/forms-and-leads`: All critical business conversion events (`generate_lead`, `form_complete`).
* `/job-applications`: Specialized tracking for the `comfortkeepers.jobs` recruiting flow.
* `/page-views`: The foundation of the Single Page Application (SPA) tracking sequence.
* `/interactions`: Auto-tracked engagement events (clicks, scrolls, video progress).

## 🤖 Automated Validation (JSON Schemas)
In addition to human-readable Markdown files, this directory contains strict JSON Schemas (e.g., `user-data-pii.json`). 

Development and QA teams must use these schemas in their CI/CD pipelines to validate that the `dataLayer.push()` payloads are formatted perfectly before any frontend code is deployed to production. If the PII schema breaks, the Server-Side Hashing module will fail.