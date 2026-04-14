# Hire Candidate Complete

Fire when a candidate's hiring process is complete and they have been officially hired. This is the final event in the Paradox recruitment funnel.

**Source**: Paradox DataHub (`further-ga4` processor)
**Paradox Token**: TBD (expected pattern: `hire:hire-complete` or similar)
**Paradox Label**: TBD (expected: `Hire: Hire Complete`)

> **Note**: No sample payload has been received for this event yet. This spec is based on the established pattern from the other Paradox events and the `candidate.hired_date` field observed in the capture/offer payloads. Update this spec once the actual Paradox webhook payload is confirmed.

## Request Info

```
POST /mp/collect?api_secret=<API_SECRET>&measurement_id=<MEASUREMENT_ID> HTTP/1.1
HOST: analytics.comfortkeepers.jobs
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
      "name": "hire_candidate_complete",
      "params": {
        "detailed_event": "Hire Candidate Complete",
        "job_req_id": "<paradox_job_req_id>",
        "job_title": "<job_title>",
        "office_id": "<group_id>",
        "candidate_journey": "<candidate_journey>",
        "hired_date": "<hired_date>",
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
| `client_id` | string | **Required** | GA4 client_id from the candidate's browser session. Must be captured during the web session and persisted through the entire Paradox candidate lifecycle. | `141853617.1775570691` |
| `timestamp_micros` | string | **Required** | Event timestamp in Unix epoch microseconds. Derived from Paradox `candidate.updated_at`. See timestamp considerations below. | `1712502581939103` |
| `user_data` | array | **Required** | Array containing user-provided data for enhanced conversions. | |
| `user_data.sha256_first_name` | string | Recommended | SHA-256 hashed first name. Source: `candidate.attributes.first_name`. | `<hashed_value>` |
| `user_data.sha256_last_name` | string | Recommended | SHA-256 hashed last name. Source: `candidate.attributes.last_name`. | `<hashed_value>` |
| `user_data.sha256_user_email` | string | Recommended | SHA-256 hashed email. Source: `candidate.email`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_user_phone_number` | string | Recommended | SHA-256 hashed phone (E.164 format before hashing). Source: `candidate.phone`. | `<hashed_value>` |
| `user_data.sha256_street` | string | Recommended | SHA-256 hashed street address. Source: `candidate.attributes.address`. | `<hashed_value>` |
| `user_data.sha256_city` | string | Recommended | SHA-256 hashed city. Source: `candidate.attributes.city`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_region` | string | Recommended | SHA-256 hashed state/region. Source: `candidate.attributes.state`. Lowercase and trim before hashing. | `<hashed_value>` |
| `user_data.sha256_postal_code` | string | Recommended | SHA-256 hashed postal code. Source: `candidate.attributes.zip_code`. | `<hashed_value>` |
| `user_data.sha256_country` | string | Recommended | SHA-256 hashed country. Default to "us" for CKF. Lowercase before hashing. | `<hashed_value>` |
| `events[].name` | string | **Required** | GA4 event name. Always `hire_candidate_complete`. | `hire_candidate_complete` |
| `events[].params.detailed_event` | string | **Required** | Descriptive event label for reporting. | `Hire Candidate Complete` |
| `events[].params.job_req_id` | string | **Required** | Paradox job requisition ID. Source: `candidate.job_req_id`. | `PDX_CK_9420A59C-6944-46AB-92CE-FE29D876B626` |
| `events[].params.job_title` | string | Recommended | Position title. Source: `candidate.job_title`. | `Personal Care Aide` |
| `events[].params.office_id` | string | Recommended | Franchise/office identifier. Source: `candidate.group_id`. | `44398` |
| `events[].params.candidate_journey` | string | Optional | Paradox candidate journey name. Source: `candidate.candidate_journey`. | `Default Candidate Journey` |
| `events[].params.hired_date` | string | Recommended | Date the candidate was officially hired. Source: `candidate.hired_date`. | `2026-04-10` |
| `events[].params.utm_source` | string | Recommended | Traffic source. Source: `candidate.attributes.utm_source`. | `Indeed Apply (Sponsored)` |
| `events[].params.utm_medium` | string | Recommended | Traffic medium. Source: `candidate.attributes.utm_medium`. | `paid` |
| `events[].params.engagement_time_msec` | number | **Required** | Must be > 0 for GA4. Use `1` for server-side events. | `1` |

## Important Notes

### Timestamp Considerations
The hire event will almost certainly fire **well beyond** the 72-hour GA4 MP window from the candidate's original web session. The `timestamp_micros` should be set to the current time when the hire event fires (not the original `candidate.created_at`). Store the actual `hired_date` as an event parameter for accurate reporting.

### client_id Requirement
Same as other Paradox events. The `client_id` from the original browser session must persist through the full candidate lifecycle. Given the longer time between application and hire, this field may be missing more frequently than for earlier-stage events.

### user_data Structure
The `user_data` object must be at the **top level** of the payload (same level as `client_id`), NOT inside `events[].params`.

### Hashing Requirements
All fields in `user_data` must be trimmed, lowercased, and hashed with SHA-256 (including city, region, postal_code, country — consistent with client-side dataLayer specs). Phone numbers must be normalized to E.164 format before hashing.

### Pending Confirmation
This spec requires validation against the actual Paradox webhook payload for the hire event. Key items to confirm:
- Exact Paradox token and label
- Available candidate attributes at the hire stage
- Whether `hired_date` is populated
