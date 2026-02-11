#### Form Submission Succeeded

Fire whenever a user successfully completes a form.

This event is fired when form input is successfully received and processed. This is in contrast to `form_error` which occurs when a submission is attempted but an error occurs and the form input is not received and processed.

**Javascript Code**

```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null, user_data: null });  // Clear the previous event_data object.
dataLayer.push({
 "detailed_event": "Form Submission Succeeded",
 "event": "form_complete",
  "event_data": {
    "category": "<category>",
    "identifier": "<identifier>",
    "name": "<name>"
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
});

```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|category|string|recommended|A human-readable identifier whose purpose will vary by event, but generally is used to group things (forms, links, videos) into loose associations based upon shared characteristics. If running low on custom dimensions, you may combine multiple categories together in this field, separated by greater than (>) or slash (/). See https://schema.org/category.|Job Application|||||||
|identifier|string|recommended|The form machine-readable name. This should be a unique value specific to this form, if one exists. If one does not exist, this can also be populated with the same value as the <name>.|form-12345|||||||
|name|string|required|The form human-readable name. This should be something that an analyst without a deep knowledge of the technical implementation of the site can easily identify the form with. It should be lowercase snake_case.|caregiver_1078_application|||||||
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
