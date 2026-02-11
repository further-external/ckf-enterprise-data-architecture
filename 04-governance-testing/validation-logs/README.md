# Validation Logs & Deployment Artifacts
### The R²IV Governance Standard

This directory is the artifact repository for our **R²IV (Request, Review, Implement, Validate)** deployment model. 

To maintain the integrity of our Data Sovereignty architecture, **no changes can be published to the Server-Side or Client-Side production containers without documented proof of validation.**

## 📋 The Validation Standard
Before a "Safe Launch" publish sequence is initiated, QA Engineers or Architects must deposit screenshots or log exports in this folder proving the following scenarios have passed:

1. **Server Debugger Status (`200 OK`):** * A screenshot of the sGTM preview mode confirming the vendor tag (e.g., Meta CAPI, StackAdapt) fired successfully with a `200 OK` HTTP status.
2. **PII Hashing Verification:**
   * A snapshot of the outgoing request body proving that `user_data.em` (Email) and `user_data.ph` (Phone) have been successfully converted to **SHA256 strings** prior to leaving our First-Party endpoint.
3. **Data Layer Parity:**
   * Confirmation that the `event_id` matches identically between the Web browser payload and the Server-Side outgoing payload to ensure downstream deduplication works.

**Current Phase Artifacts:** All Phase 1 execution logs are stored in the `/phase-1-cutovers/` sub-directory.