# Close Converted Lead

Fire when a lead successfully converts into a franchisee, corresponding to the "AWARDED" status update in Franconnect[cite: 49, 57].


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
      "name": "close_convert_lead",
      "params": {
        "value": <franchise_contract_value>,
        "currency": "<currency_code>",
        "franconnect_lead_id": "<hashed_franconnect_lead_id>",
        "conversion_timestamp": "<conversion_timestamp_micros>",
        "utm_source": "<stored_utm_source>", //For campaign Attribution
        "utm_medium": "<stored_utm_medium>", //For campaign Attribution
        "utm_campaign": "<stored_utm_campaign>", //For campaign Attribution
        "gclid": "<stored_gclid>", //For campaign Attribution
        "detailed_event": "Franconnect Close Convert Lead"
      }
    }
  ]
}
```

## Variable Definitions

| Field                                     | Type   | Required      | Description                                                                                                                           | Example                           |
| ----------------------------------------- | ------ | ------------- | ------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| `api_secret`                              | string | **Required** | The API secret for the GA4 property. Found in GA4 Admin: Data Streams > Measurement Protocol API secrets.                               | `fKhnzB9URSqghrauTtjGMw`           |
| `measurement_id`                          | string | **Required** | The identifier for the GA4 data stream. Found in GA4 Admin: Data Streams.                                                              | `G-3BQ5YPKFRC`                    |
| `client_id`                               | string | **Required** | Unique identifier for a user/client instance. Essential for linking offline events to online user activity.                           | `1704286278.1678886400`           |
| `timestamp_micros`                        | string | **Required** | Timestamp of the event in Unix epoch microseconds. GA4 accepts events up to 72 hours old.                                             | `1679232000000000`                |
| `events[].name`                           | string | **Required** | The custom name for your conversion event. Max 40 chars, alphanumeric & underscores, must start with a letter.                        | `close_convert_lead`              |
| `events[].params.value`                   | number | **Required** | **Standard Parameter.** The monetary value of the contract. Using `value` allows GA4 to automatically process it as revenue.          | `49500.00`                        |
| `events[].params.currency`                | string | **Required** | **Standard Parameter.** The 3-letter ISO 4217 currency code for the `value`. Required when `value` is present.                        | `USD`                             |
| `events[].params.franconnect_lead_id`     | string | Recommended   | (Custom) Hashed unique identifier for the lead in Franconnect. Useful for analysis.                                                   | `<hashed_franconnect_lead_id>`    |
| `events[].params.conversion_timestamp`    | string | Recommended   | (Custom) Timestamp in Unix epoch microseconds when the lead was converted/awarded.                                                    | `1679232000000`                   |
| `events[].params.detailed_event`          | string | Optional      | (Custom) A descriptive name for easily identifying the event source in GA4.                                                           | `Franconnect Close Convert Lead`  |
| `user_data.address.sha256_first_name`     | string | Recommended   | SHA-256 Hashed first name of the user.                                                                                                | `<hashed_value>`                  |
| `user_data.address.sha256_last_name`      | string | Recommended   | SHA-256 Hashed last name of the user.                                                                                                 | `<hashed_value>`                  |
| `user_data.address.sha256_street`         | string | Recommended   | SHA-256 Hashed street address of the user.                                                                                            | `<hashed_value>`                  |
| `user_data.address.city`                  | string | Recommended   | **Unhashed** city of the user, normalized (e.g., lowercase, no punctuation).                                                          | `new york`                        |
| `user_data.address.region`                | string | Recommended   | **Unhashed** state/region of the user, normalized (e.g., 'ny' for New York).                                                          | `ny`                              |
| `user_data.address.postal_code`           | string | Recommended   | **Unhashed** postal code of the user.                                                                                                 | `10011`                           |
| `user_data.address.country`               | string | Recommended   | **Unhashed** two-letter ISO country code of the user.                                                                                 | `us`                              |
| `user_data.sha256_email_address`          | array  | Recommended   | Array containing the SHA-256 Hashed email address of the user.                                                                        | `["<hashed_email>"]`              |
| `user_data.sha256_phone_number`           | array  | Recommended   | Array containing the SHA-256 Hashed phone number of the user (normalized to E.164 format before hashing).                             | `["<hashed_phone_number>"]`       |







