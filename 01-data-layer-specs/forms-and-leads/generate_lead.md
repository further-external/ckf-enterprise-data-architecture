# Generate Lead

Fire when a new lead is created in Franconnect. This is the first event in the lead lifecycle.

## Request info
POST /mp/collect?api_secret="insert_secret_key"&measurement_id=G-3BQ5YPKFRC HTTP/1.1   
HOST: www.google-analytics.com
Content-Type: application/json

##Payload

```js
{
  "client_id": "<client_id>",
  "user_id": "<hashed_email_as_user_id>", //Planned for phase 2
  "timestamp_micros": "<timestamp_micros>",
  "user_data": {
    "address": {
      "sha256_first_name": "<hashed_user_first_name>",
      "sha256_last_name": "<hashed_user_last_name>",
      "sha256_street": "<hashed_street>",
      "city": "<normalized_city>",
      "region": "<normalized_region>",
      "postal_code": "<normalized_postal_code>",
      "country": "<normalized_country_code>"
    },
    "sha256_email_address": ["<hashed_user_email>"],
    "sha256_phone_number": ["<hashed_user_phone_number>"]
  },
  "events": [
    {
      "name": "generate_lead",
      "params": {
        "franconnect_lead_id": "<hashed_franconnect_lead_id>",
        "lead_source": "Franconnect",
        "utm_source": "<captured_utm_source>", //For campaign Attribution
        "utm_medium": "<captured_utm_medium>", //For campaign Attribution
        "utm_campaign": "<captured_utm_campaign>", //For campaign Attribution
        "gclid": "<captured_gclid>", //For campaign Attribution
        "is_international": "<true_or_false>",
        "detailed_event": "Franconnect Generate Lead"
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
|`timestamp_micros`|string|required|Timestamp of the event in Unix epoch microseconds. GA4 accepts events up to 72 hours old.|`1678886400000000`|||||||
|`events[].name`|string|required|The custom name for your event. Max 40 chars, alphanumeric & underscores, must start with a letter.|`generate_lead`|`^[a-zA-Z][a-zA-Z0-9_]*$`||40||||
|`events[].params.franconnect_lead_id`|string|required|(Custom) Hashed unique identifier for the lead in Franconnect. Useful for analysis.|`<hashed_franconnect_lead_id>`|||100||||
|`events[].params.lead_source`|string|recommended|(Custom) The specific source system of the lead.|`Franconnect`|||100||||
|`events[].params.utm_source`|string|recommended|The campaign source, captured from the user's initial online session. Passed through to Franconnect.|`google`|||100||||
|`events[].params.utm_medium`|string|recommended|The campaign medium, captured from the user's initial online session. Passed through to Franconnect.|`cpc`|||100||||
|`events[].params.utm_campaign`|string|recommended|The campaign name, captured from the user's initial online session. Passed through to Franconnect.|`spring_sale`|||100||||
|`events[].params.is_international`|string|optional|(Custom) A flag to identify international leads. Send as a string "true" or "false".|`false`|||100||||
|`events[].params.detailed_event`|string|optional|(Custom) A descriptive name for easily identifying the event source in GA4.|`Franconnect Generate Lead`|||100||||
|`user_data.address.sha256_first_name`|string|recommended|SHA-256 Hashed first name of the user.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.address.sha256_last_name`|string|recommended|SHA-256 Hashed last name of the user.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.address.sha256_street`|string|recommended|SHA-256 Hashed street address of the user.|`<hashed_value>`|`^[a-fA-F0-9]{64}$`|64|64||||
|`user_data.address.city`|string|recommended|**Unhashed** city of the user, normalized (e.g., lowercase, no punctuation).|`new york`|||||||
|`user_data.address.region`|string|recommended|**Unhashed** state/region of the user, normalized (e.g., 'ny' for New York).|`ny`|||||||
|`user_data.address.postal_code`|string|recommended|**Unhashed** postal code of the user.|`10011`|||||||
|`user_data.address.country`|string|recommended|**Unhashed** two-letter ISO country code of the user.|`us`|||||||
|`user_data.sha256_email_address`|array|recommended|Array containing the SHA-256 Hashed email address of the user.|`["<hashed_email>"]`|||||||
|`user_data.sha256_phone_number`|array|recommended|Array containing the SHA-256 Hashed phone number of the user (normalized to E.164 format before hashing).|`["<hashed_phone_number>"]`|||||||








