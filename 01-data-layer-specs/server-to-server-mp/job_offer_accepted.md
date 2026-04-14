# Job Offer Accepted

Fire when a candidate accepts a job offer through the Paradox platform. This corresponds to the `offer:offer-accepted` event in Paradox DataHub.

**Source**: Paradox DataHub (`further-ga4` processor, `viv-candidate` processor)
**Paradox Token**: `offer:offer-accepted`
**Paradox Label**: `Offer: Offer Accepted`

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
      "name": "job_offer_accepted",
      "params": {
        "detailed_event": "Job Offer Accepted",
        "job_req_id": "<paradox_job_req_id>",
        "job_title": "<job_title>",
        "office_id": "<group_id>",
        "candidate_journey": "<candidate_journey>",
        "pay_rate": "<pay_rate>",
        "offer_created_date": "<offer_created_date>",
        "offer_accepted_date": "<offer_accepted_date>",
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
| `client_id` | string | **Required** | GA4 client_id from the candidate's browser session. Must be captured during the web session and passed through Paradox as an attribute (e.g., `utm_data_hub`). | `141853617.1775570691` |
| `timestamp_micros` | string | **Required** | Event timestamp in Unix epoch microseconds. Derived from Paradox `candidate.updated_at`. GA4 accepts events up to 72 hours old. Note: offer accepted events may fire **days** after the original web session, so the 72-hour window may be exceeded. Use the `offer_accepted_date` for reporting. | `1712502581939103` |
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
| `events[].name` | string | **Required** | GA4 event name. Always `job_offer_accepted`. | `job_offer_accepted` |
| `events[].params.detailed_event` | string | **Required** | Descriptive event label for reporting. | `Job Offer Accepted` |
| `events[].params.job_req_id` | string | **Required** | Paradox job requisition ID. Source: `candidate.job_req_id`. | `PDX_CK_9420A59C-6944-46AB-92CE-FE29D876B626` |
| `events[].params.job_title` | string | Recommended | Position title. Source: `candidate.job_title`. | `Personal Care Aide` |
| `events[].params.office_id` | string | Recommended | Franchise/office identifier. Source: `candidate.group_id`. | `44398` |
| `events[].params.candidate_journey` | string | Optional | Paradox candidate journey name. Source: `candidate.candidate_journey`. | `Default Candidate Journey` |
| `events[].params.pay_rate` | string | Optional | Offered pay rate. Source: `candidate.attributes.pay_rate`. | `$18.00 / per hour` |
| `events[].params.offer_created_date` | string | Optional | Date the offer was created. Source: `candidate.attributes.offer_created_date`. | `2026-04-07` |
| `events[].params.offer_accepted_date` | string | Optional | Date the offer was accepted. Source: `candidate.attributes.offer_accepted_date`. | `2026-04-07` |
| `events[].params.utm_source` | string | Recommended | Traffic source. Source: `candidate.attributes.utm_source`. | `Indeed Apply (Sponsored)` |
| `events[].params.utm_medium` | string | Recommended | Traffic medium. Source: `candidate.attributes.utm_medium`. | `paid` |
| `events[].params.engagement_time_msec` | number | **Required** | Must be > 0 for GA4. Use `1` for server-side events. | `1` |

## Important Notes

### Full Address Data Available
Unlike earlier funnel events, offer accepted candidates have **full address data** in their attributes (`address`, `city`, `state`, `zip_code`). This enables richer `user_data` for Enhanced Conversions matching.

### Timestamp Considerations
Offer acceptance often happens days after the initial web session. The `timestamp_micros` field should use the `candidate.updated_at` timestamp, but note that GA4 MP rejects events older than 72 hours. If the offer is accepted beyond the 72-hour window, consider using the current time as `timestamp_micros` and storing the actual `offer_accepted_date` as an event parameter.

### client_id Requirement
Same as other events -- the `client_id` must be captured from the candidate's original browser session and persisted through the Paradox candidate lifecycle.

### user_data Structure
The `user_data` object must be at the **top level** of the payload (same level as `client_id`), NOT inside `events[].params`.

### Hashing Requirements
All fields in `user_data` must be trimmed, lowercased, and hashed with SHA-256 (including city, region, postal_code, country — consistent with client-side dataLayer specs). Phone numbers must be normalized to E.164 format before hashing.
