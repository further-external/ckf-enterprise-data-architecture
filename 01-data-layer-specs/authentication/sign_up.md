# Sign Up

Fire whenever a user successfully creates an account.

## Javascript Code

```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null, page_data: null, user_data: null });  
dataLayer.push({
  event: 'sign_up',
  event_data: {
    method: '<method>'
  },
  page_data: {
    franchise_id: '<franchise_id>' // Moved for consistency
  },
  user_data: {
    user_id: '<user_id>',
    email_address: '<email_address>', // CRITICAL FOR sGTM CAPI
    phone_number: '<phone_number>'    // CRITICAL FOR sGTM CAPI
  }
});
```

## Variable Definitions

|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|method|string|recommended|The method by which a user created a new account.|local, social_login|
|franchise_id|integer|optional|Set on all events that can be tied back to a franchise.|143, 576, 1134|
