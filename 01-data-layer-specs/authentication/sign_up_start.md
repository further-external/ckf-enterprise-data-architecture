# Sign Up Start

Fire whenever a user views the first step of the account creation form.

## Javascript Code

```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null });  // Clear the previous event_data object.
dataLayer.push({
  event: 'sign_up_start'
  event_data: {
    method: '<method>',
    franchise_id: '<franchise_id>'
  }
});
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|method|string|recommended|The method by which a user created a new account.|local, social_login|
|franchise_id|integer|optional|Set on all events that can be tied back to a franchise.|143, 576, 1134|
