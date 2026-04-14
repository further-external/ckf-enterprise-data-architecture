# Working Lead

Fire when active engagement begins with a lead, such as when the status changes to "Contacted" in Franconnect, or when specific interaction calls occur (e.g., Intro/Pre-Qual Call, CK Oppty Call, FDD Review Call).

## Request info
POST /mp/collect?api_secret="insert_secret_key"&measurement_id=G-3BQ5YPKFRC HTTP/1.1   
HOST: analytics.comfortkeepersfranchise.com
Content-Type: application/json

##Payload

```js
{
  "client_id": "<client_id>",
  "user_id": "<hashed_email_as_user_id>", //Planned for phase 2
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
      "name": "working_lead",
      "params": {
        "franconnect_lead_id": "<hashed_franconnect_lead_id>",
        "working_timestamp": "<working_timestamp_micros>",
        "call_type": "<call_type>",
        "utm_source": "<stored_utm_source>", //For campaig Attribution
        "utm_medium": "<stored_utm_medium>", //For campaig Attribution
        "utm_campaign": "<stored_utm_campaign>", //For campaig Attribution
        "gclid": "<stored_gclid>", //For campaig Attribution
        "detailed_event": "Franconnect Working Lead",
        "engagement_time_msec": 1
      }
    }
  ]
}
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
|---|---|---|---|---|---|---|---|---|---|---|
|`api_secret`|string|required|The API secret for the GA4 property. Found in GA4 Admin: Data Streams > Measurement Protocol API secrets.|`fKhnzB9URSqghrauTtjGMw`|||||||
|`measurement_id`|string|required|The identifier for the GA4 data stream. Found in GA4 Admin: Data Streams.|`G-3BQ5YPKFRC`|||||||
|`client_id`|string|required|Unique identifier for a user/client instance. Essential for linking offline events to online user activity.|`1704286278.1678886400`|||||||
|`timestamp_micros`|string|required|Timestamp of the event in Unix epoch microseconds. GA4 accepts events up to 72 hours old.|`1679145600000000`|||||||
|`events[].name`|string|required|The custom name for your event. Max 40 chars, alphanumeric & underscores, must start with a letter.|`working_lead`|`^[a-zA-Z][a-zA-Z0-9_]*$`||40||||
|`events[].params.franconnect_lead_id`|string|required|(Custom) Hashed unique identifier for the lead in Franconnect. Useful for analysis.|`<hashed_franconnect_lead_id>`|||100||||
|`events[].params.working_timestamp`|string|recommended|(Custom) Timestamp in Unix epoch microseconds when the specific 'working' status/interaction occurred.|`1679145600000`|||100||||
|`events[].params.call_type`|string|required|(Custom) Specifies the type of 'working' interaction, based on Franconnect stage mapping.|`intro_prequal`|||100||||
|`events[].params.detailed_event`|string|optional|(Custom) A descriptive name for easily identifying the event source in GA4.|`Franconnect Working Lead`|||100||||
|`user_data.sha256_first_name`|string|recommended|SHA-256 hashed first name. Lowercase and trim before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_last_name`|string|recommended|SHA-256 hashed last name. Lowercase and trim before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_user_email`|string|recommended|SHA-256 hashed email. Lowercase and trim before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_user_phone_number`|string|recommended|SHA-256 hashed phone (E.164 format before hashing).|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_street`|string|recommended|SHA-256 hashed street address.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_city`|string|recommended|SHA-256 hashed city. Lowercase and trim before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_region`|string|recommended|SHA-256 hashed state/region. Lowercase and trim before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_postal_code`|string|recommended|SHA-256 hashed postal code.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.sha256_country`|string|recommended|SHA-256 hashed country. Default to "us". Lowercase before hashing.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`events[].params.engagement_time_msec`|number|required|Must be > 0 for GA4. Use `1` for server-side events.|`1`|||||||








