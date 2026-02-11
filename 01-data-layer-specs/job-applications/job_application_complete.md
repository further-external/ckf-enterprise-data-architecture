# Job Application Complete

Fire whenever a user completes a job application. 

## Javascript Code

```js
window.dataLayer = window.dataLayer || []; 
window.dataLayer.push({event_data: null, user_data: null}) 
dataLayer.push({
  "event": "job_application_complete",
  "event_data": {
    "identifier": "<identifier>",
    "method": "<method>",
    "office_id": '<office_id>'
  },
  "user_data": [{
    "sha256_first_name": "<hashed_user_first_name>",
    "sha256__last_name": "<hashed_user_last_name>",
    "sha256_user_email": "<hashed_user_email>",
    "sha256_user_phone_number": "<hashed_user_phone_number>",
    "sha256_street": "<hashed_street>",
    "city": "<city>",
    "region": "<region>",
    "postal_code": "<postal_code>",
    "country": "country>"
  }]
})

```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|identifier|string|recommended|A unique machine-readable identifier whose purpose will vary by event, but generally is used to differentiate one "thing" (form, link, video) from another. See https://schema.org/identifier.|ckfi:56f9dd7d-80e6-445c-b638-4e1759789077|||||||
|method|string|recommended|The method by which a user is applying|webform, phone, chat|||||||
|office_id|string|optional|Set on all events that can be tied back to an office.|/ohio/springfield|||||||
|user_data|array|required|An array containing user-provided data for enhanced conversions. Each object in the array represents a user.||||||||
|user_data.sha256_first_name|string|recommended|SHA-256 hashed value of the user's first name.||||||||
|user_data.sha256__last_name|string|recommended|SHA-256 hashed value of the user's last name.||||||||
|user_data.sha256_user_email|string|recommended|SHA-256 hashed value of the user's email address.||||||||
|user_data.sha256_user_phone_number|string|recommended|SHA-256 hashed value of the user's phone number (should be in E.164 format before hashing).||||||||
|user_data.sha256_street|string|recommended|SHA-256 hashed value of the user's street address.||||||||
|user_data.sha256_city|string|recommended|SHA-256 hashed value of the user's city.||||||||
|user_data.sha256_region|string|recommended|SHA-256 hashed value of the user's region (state or province).||||||||
|user_data.sha256_postal_code|string|recommended|SHA-256 hashed value of the user's postal code.||||||||
|user_data.sha256_country|string|recommended|SHA-256 hashed value of the user's country.|US|||||||
