#### B2B Referral Form Submission Succeeded

Fire whenever a user successfully completes the B2B referral form on `/b2b-referral/`.

This event is fired when the referral form input is successfully received and processed. This is a specialized version of `form_complete` that captures additional B2B referral context (organization, title, referral type, and location of care).

**Javascript Code**

```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null, user_data: null });  // Clear the previous event_data object.
dataLayer.push({
  "detailed_event": "Form Submission Succeeded",
  "event": "referral_form_complete",
  "event_data": {
    "category": "B2B Referral",
    "identifier": "<identifier>",
    "name": "b2b_referral",
    "organization": "<organization>",
    "title": "<title>",
    "referral_type": "<referral_type>",
    "location_of_care_zip": "<location_of_care_zip>"
  },
  "user_data": [{
    "sha256_first_name": "<hashed_user_first_name>",
    "sha256_last_name": "<hashed_user_last_name>",
    "sha256_user_email": "<hashed_user_email>",
    "sha256_user_phone_number": "<hashed_user_phone_number>",
    "sha256_postal_code": "<hashed_postal_code>",
    "sha256_country": "<hashed_country>"
  }]
});

```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|category|string|required|The form category. Always "B2B Referral" for this event.|B2B Referral|||||||
|identifier|string|recommended|The form machine-readable name. This should be a unique value specific to this form, if one exists. If one does not exist, this can also be populated with the same value as the `name`.|b2b-referral-form|||||||
|name|string|required|The form human-readable name. Should be lowercase snake_case.|b2b_referral|||||||
|organization|string|required|The referrer's organization name. Captured from the "Your Organization" field.|Springfield Regional Medical Center|||||||
|title|string|required|The referrer's job title. Captured from the "Your Title" field.|Discharge Coordinator|||||||
|referral_type|string|required|The type of referrer, captured from the "Who Are you?" dropdown.|Hospital Discharge Planner|||||||
|location_of_care_zip|string|required|The zip code where in-home care is needed. Captured from the "Location of Care (Zip Code)" field. This is the care recipient's location, not the referrer's address.|45501||5|10||||
|user_data|array|required|An array containing user-provided data for enhanced conversions. Each object in the array represents a user.||||||||
|user_data.sha256_first_name|string|recommended|SHA-256 hashed value of the referrer's first name. Parsed from the "Your Name" field. Lowercase and trim before hashing.||||||||
|user_data.sha256_last_name|string|recommended|SHA-256 hashed value of the referrer's last name. Parsed from the "Your Name" field. Lowercase and trim before hashing.||||||||
|user_data.sha256_user_email|string|recommended|SHA-256 hashed value of the referrer's email address. Lowercase and trim before hashing.||||||||
|user_data.sha256_user_phone_number|string|recommended|SHA-256 hashed value of the referrer's phone number (normalize to E.164 format before hashing).||||||||
|user_data.sha256_postal_code|string|recommended|SHA-256 hashed value of the referrer's postal code. Use `location_of_care_zip` value. Lowercase and trim before hashing.||||||||
|user_data.sha256_country|string|recommended|SHA-256 hashed value of the referrer's country. Default to "us". Lowercase and trim before hashing.|US|||||||

## Implementation Notes

- **Name parsing**: The form has a single "Your Name" field. Split on the first space to derive `first_name` and `last_name`. If only one word is provided, use it as `first_name` and leave `last_name` empty.
- **Case normalization**: Lowercase all `user_data` values before SHA-256 hashing. Trim leading/trailing whitespace.
- **Phone format**: Normalize phone number to E.164 format (e.g., `+18555045893`) before hashing.
- **Referral type values**: Populate `referral_type` with the exact value selected from the "Who Are you?" dropdown. Use lowercase snake_case if transforming (e.g., `hospital_discharge_planner`).
- **Unhashed event_data**: Fields in `event_data` (organization, title, referral_type, location_of_care_zip) are sent as plain text, not hashed. Only `user_data` fields are hashed.
