# Phase 1: Server-Side Vendor Validation Matrix
**Derived from:** `CKF | Server-Side Validation | Phase 1.csv`

The following scenarios must be validated in the Server-Side debugger prior to executing the Phase 1 "Safe Launch" publish sequence.

| Event / Trigger | Vendor Tag Name | Expected Result (Server Debugger) | Status |
| :--- | :--- | :--- | :--- |
| `generate_lead` | **3PT \| META Capi Master** | 1. Status: `200 OK`. <br>2. Check Body: `user_data.em` must be SHA256 hashed. <br>3. Routing: Pixel ID matches Corporate parameter. | 🟢 Ready |
| `generate_lead` | **3PT \| StackAdapt \| Conversion** | 1. Request URL is `api.stackadapt.com`. <br>2. Verify `conv_id` matches Lead Conversion ID. | 🟢 Ready |
| `find_a_location` | **3PT \| StackAdapt \| Conversion** | 1. Verify `conv_id` matches the B2B Referral ID. | 🟢 Ready |
| `generate_lead` | **3PT \| Floodlight \| CKF Main** | 1. Outgoing request to `fls.doubleclick.net`. <br>2. Check `ord=` contains the specific `event_id`. | 🟢 Ready |
| `generate_lead` | **Bing UET - Global Tag** | *(Negative Test)* Ensure this tag fires on Client-Side only. Should NOT be present in Server requests. | 🟢 Ready |