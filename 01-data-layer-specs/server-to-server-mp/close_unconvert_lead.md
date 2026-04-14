# Close Unconverted Lead

Fire when a lead did not ultimately become a franchisee, despite progressing through some or all of the sales pipeline, corresponding to the "Dead Lead" status update in Franconnect.

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
      "name": "close_unconvert_lead",
      "params": {
        "franconnect_lead_id": "<hashed_franconnect_lead_id>",
        "unconversion_timestamp": "<unconversion_timestamp_micros>",
        "unconversion_reason": "<unconversion_reason>",
        "utm_source": "<stored_utm_source>", //For campaign Attribution
        "utm_medium": "<stored_utm_medium>", //For campaign Attribution
        "utm_campaign": "<stored_utm_campaign>", //For campaign Attribution
        "gclid": "<stored_gclid>", //For campaign Attribution
        "detailed_event": "Franconnect Close Unconvert Lead",
        "engagement_time_msec": 1
      }
    }
  ]
}
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
|---|---|---|---|---|---|---|---|---|---|---|
|`api_secret`|string|required|The API secret for the GA4 property. Found in GA4 Admin: Data Streams > Measurement Protocol API secrets. Must be kept confidential.|`fKhnzB9URSqghrauTtjGMw`|||||||
|`measurement_id`|string|required|The identifier for the GA4 data stream. Found in GA4 Admin: Data Streams.|`G-3BQ5YPKFRC`|||||||
|`client_id`|string|required|Unique identifier for a user/client instance. Essential for linking offline events to online user activity.|`1704286278.1678886400`|||||||
|`timestamp_micros`|string|required|Timestamp of the event in Unix epoch microseconds. GA4 accepts events up to 72 hours old.|`1679318400000000`|||||||
|`events[].name`|string|required|The custom name for your event. Max 40 chars, alphanumeric & underscores, must start with a letter.|`close_unconvert_lead`|`^[a-zA-Z][a-zA-Z0-9_]*$`||40||||
|`events[].params.franconnect_lead_id`|string|required|(Custom) Hashed unique identifier for the lead in Franconnect. Useful for analysis.|`<hashed_franconnect_lead_id>`|||100||||
|`events[].params.unconversion_timestamp`|string|recommended|(Custom) Timestamp in Unix epoch microseconds when the lead was closed/lost.|`1679318400000`|||100||||
|`events[].params.unconversion_reason`|string|optional|(Custom) Reason provided for why the lead did not convert (if available from Franconnect).|`Lost to competitor`|||100||||
|`events[].params.detailed_event`|string|optional|(Custom) A descriptive name for easily identifying the event source in GA4.|`Franconnect Close Unconvert Lead`|||100||||
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







