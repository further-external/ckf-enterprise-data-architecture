# Job Application Complete

Fire when a candidate successfully completes the job application (Caregiver form) through the Paradox/Olivia chatbot. This corresponds to the `capture:capture-complete` event in Paradox DataHub.

**Source**: Paradox DataHub (`further-ga4` processor)
**Paradox Token**: `capture:capture-complete`
**Paradox Label**: `Capture: Capture Complete`

## Request Info

```
POST /mp/collect?api_secret=<API_SECRET>&measurement_id=<MEASUREMENT_ID> HTTP/1.1
HOST: www.google-analytics.com
Content-Type: application/json
```

## Payload

```json
{
  "client_id": "<client_id>",
  "timestamp_micros": "<timestamp_micros>",
  "user_data": [{
    "sha256_first_name": "<hashed_first_name>",
    "sha256_last_name": "<hashed_last_name>",
    "sha256_user_email": "<hashed_user_email>",
    "sha256_user_phone_number": "<hashed_user_phone_number>",
    "sha256_street": "<hashed_street>",
    "sha256_city": "<hashed_city>",
    "sha256_region": "<hashed_region>",
    "sha256_postal_code": "<hashed_postal_code>",
    "sha256_country": "<hashed_country>"
  }],
  "events": [
    {
      "name": "job_application_complete",
      "params": {
        "detailed_event": "Job Application Complete",
        "job_req_id": "<paradox_job_req_id>",
        "job_title": "<job_title>",
        "office_id": "<group_id>",
        "candidate_journey": "<candidate_journey>",
        "utm_source": "<utm_source>",
        "utm_medium": "<utm_medium>",
        "engagement_time_msec": 1
      }
    }
  ]
}
```

## Variable Definitions

| Field | Type | Required | Description | Example |
| --- | --- | --- | --- | --- |
| `api_secret` | string | **Required** | GA4 Measurement Protocol API secret. Found in GA4 Admin > Data Streams > Measurement Protocol API secrets. | `fKhnzB9URSqghrauTtjGMw` |
| `measurement_id` | string | **Required** | GA4 data stream measurement ID. Found in GA4 Admin > Data Streams. | `G-XXXXXXXXXX` |
| `client_id` | string | **Required** | GA4 client_id from the candidate's browser session. Must be captured during the web session and passed through Paradox as an attribute (e.g., `utm_data_hub`). Without this, GA4 cannot stitch the server event to the web session. | `141853617.1775570691` |
| `timestamp_micros` | string | **Required** | Event timestamp in Unix epoch microseconds. Derived from Paradox `candidate.updated_at`. GA4 accepts events up to 72 hours old. | `1712502136698060` |
| `user_data` | array | **Required** | Array containing user-provided data for enhanced conversions. | |
| `user_data.sha256_first_name` | string | Recommended | SHA-256 hashed first name. Source: `candidate.attributes.first_name`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_last_name` | string | Recommended | SHA-256 hashed last name. Source: `candidate.attributes.last_name`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_user_email` | string | Recommended | SHA-256 hashed email. Source: `candidate.email`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_user_phone_number` | string | Recommended | SHA-256 hashed phone (E.164 format before hashing). Source: `candidate.phone`. | `<hashed_value>` |
| `user_data.sha256_street` | string | Recommended | SHA-256 hashed street address. Source: `candidate.attributes.address`. | `<hashed_value>` |
| `user_data.sha256_city` | string | Recommended | SHA-256 hashed city. Source: `candidate.attributes.city`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_region` | string | Recommended | SHA-256 hashed state/region. Source: `candidate.attributes.state`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_postal_code` | string | Recommended | SHA-256 hashed postal code. Source: `candidate.attributes.zip_code`. | `<hashed_value>` |
| `user_data.sha256_country` | string | Recommended | SHA-256 hashed country. Default to "us" for CKF. Lowercase before hashing. | `<hashed_value>` |
| `events[].name` | string | **Required** | GA4 event name. Always `job_application_complete`. | `job_application_complete` |
| `events[].params.detailed_event` | string | **Required** | Descriptive event label for reporting. | `Job Application Complete` |
| `events[].params.job_req_id` | string | **Required** | Paradox job requisition ID. Source: `candidate.job_req_id`. | `PDX_CK_9420A59C-6944-46AB-92CE-FE29D876B626` |
| `events[].params.job_title` | string | Recommended | Position title. Source: `candidate.job_title`. | `Personal Care Aide` |
| `events[].params.office_id` | string | Recommended | Franchise/office identifier. Source: `candidate.group_id`. | `44398` |
| `events[].params.candidate_journey` | string | Optional | Paradox candidate journey name. Source: `candidate.candidate_journey`. | `Default Candidate Journey` |
| `events[].params.utm_source` | string | Recommended | Traffic source captured during the candidate's session. Source: `candidate.attributes.utm_source`. | `Indeed Apply (Organic)` |
| `events[].params.utm_medium` | string | Recommended | Traffic medium captured during the candidate's session. Source: `candidate.attributes.utm_medium`. | `paid` |
| `events[].params.engagement_time_msec` | number | **Required** | Must be > 0 for the event to be processed by GA4. Use `1` for server-side events. | `1` |

## Important Notes

### client_id Requirement
The `client_id` is the single most critical field for this integration. It links the server-side Paradox event back to the candidate's original web session in GA4. The DataHub must:

1. Capture the GA4 `client_id` (from the `_ga` cookie) during the candidate's initial web interaction
2. Pass it to Paradox as a candidate attribute (e.g., `utm_data_hub`)
3. Include it in the MP payload when constructing the `further-ga4` request

Without a valid `client_id`, the event will still appear in GA4 but will be orphaned (not tied to any user session or acquisition source).

### user_data Structure
The `user_data` object must be at the **top level** of the payload (same level as `client_id`), NOT inside `events[].params`. This is required for Google Ads Enhanced Conversions to work.

### Hashing Requirements
All fields in `user_data` must be:
- Trimmed of leading/trailing whitespace
- Lowercased
- Hashed with SHA-256 (including city, region, postal_code, country — consistent with client-side dataLayer specs)
- Phone numbers normalized to E.164 format before hashing (e.g., `+12029487446`)
