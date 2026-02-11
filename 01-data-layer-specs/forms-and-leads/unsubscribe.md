# Unsubscribe

Fire whenever a user unsubscribes.

## Javascript Code

```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null });  // Clear the previous event_data object.
dataLayer.push({
  event: 'subscribe',
  event_data: {
    category: '<category>',
    identifier: '<identifier>',
    name: '<name>',
    method: '<method>',
    "office_id": '<office_id>'
  }
});
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|category|string|recommended|A human-readible identifier whose purpose will vary by event, but generally is used to group things (forms, links, videos) into loose assocations based upon shared characteristics. If running low on custom dimensions, you may combine multiple categories together in this field, separated by greater than (>) or slash (/). See https://schema.org/category.|newsletter, updates, advocacy|
|identifier|string|recommended|The subscription machine-readable name. This should be a unique value specific to this subscription, if one exists. If one does not exist, this can also be populated with the same value as the `name`.|newsletter_123|
|name|string|required|The subscription human-readable name. This should be something that an analyst without a deep knowledge of the technical implementation of the site can easily identify the subscription with. It should be lowercase snake_case.|whatever_newsletter|
|method|string|recommended|The channel through which the subscription is delivered. This is usually email, but can also be product.|email|
|office_id|string|optional|Set on all events that can be tied back to an office.|/ohio/springfield|
