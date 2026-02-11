# Data Layer Specifications & Standards
### Catalyst Architecture | Input Stream

This directory defines the **Contract** between the Development Team and the Data Architecture Team. For the Server-Side GTM (sGTM) architecture to function, the Data Layer must adhere to these schemas.

## ⚠️ Critical Requirement: User Data (PII)
To support the **Privacy Hashing Module** on the server, all conversion events (Leads, Applications) must include the `user_data` object.

**Why?**
The server automatically hashes this data (SHA256) before sending it to Meta CAPI or Google Ads. If this data is missing, match rates will drop by ~40%.

### Schema: `user_data`
```json
"user_data": {
  "email_address": "john.doe@example.com",  // REQUIRED
  "phone_number": "15551234567",            // REQUIRED (E.164 format)
  "address": {
    "first_name": "John",
    "last_name": "Doe",
    "city": "Austin",
    "region": "TX",
    "postal_code": "78701",
    "country": "US"
  }
}
```