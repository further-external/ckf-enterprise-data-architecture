# Custom Templates & Variable Logic
### Server-Side Core (`GTM-KDG6S98`)

Because Server-Side GTM (sGTM) operates in a restricted Sandboxed JavaScript environment, complex logic cannot be executed via standard "Custom HTML" tags. Instead, we rely on **Custom Templates**.

This directory serves as a backup and version history for the custom `.tpl` files built for this architecture.

## 🧠 Key Template: The Agency Multiplexer (Lookup Table)
The most critical template in this architecture is the Routing logic that deprecates the need for 50+ client-side agency tags. 

* **How it works:** We utilize a complex Lookup/Regex Table variable template. 
* **Input:** The `agency_id` passed dynamically from the `agency_conversion` GA4 event.
* **Output:** The specific Vendor API Key or Pixel ID associated with that agency.
* **Result:** A single "Master" Meta CAPI tag fires, but routes the data to the correct agency's business manager dynamically. 

By storing these templates as code here, any future engineering team can import them into a new workspace without rebuilding the Sandboxed JS from scratch.