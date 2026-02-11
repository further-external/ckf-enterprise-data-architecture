# Job Application Progress

Fire whenever a user moves to a new field within the job application, a focusin event to a field within the job application.

## Javascript Code

```js
window.dataLayer = window.dataLayer || []; // Initialize the dataLayer variable to avoid JS errors
window.dataLayer.push({event_data: null}) // conditionally reset portions of DL 
window.dataLayer.push({
  "event": "job_application_progress",
  "event_data": {
    "identifier": "<identifier>",
    "method": "<method>",
    "office_id": '<office_id>',
    "field_name": '<field_name>'
  }
})
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|identifier|string|recommended|A unique machine-readible identifier whose purpose will vary by event, but generally is used to differentiate one "thing" (form, link, video) from another. See https://schema.org/identifier.|ckfi:56f9dd7d-80e6-445c-b638-4e1759789077|
|method|string|recommended|The method by which a user is applying|webform,phone,chat|
|office_id|string|optional|Set on all events that can be tied back to an office.|/ohio/springfield|
|field_name|string|optional|The name of the field being filled out/that is in focus.|143, 576, 1134|
