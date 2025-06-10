# CH5 Bypassing Client-Side Controls

## Overview

Web applications often use client-side controls to enforce restrictions, assuming users cannot tamper with them. This is a flawed assumption — attackers can fully control client-side behaviors.

## Categories of Client-Side Controls

### 1. **Data Transmitted via the Client**

These mechanisms assume data can't be altered in transit. Examples:

* **Hidden Form Fields**: Easily modified via an intercepting proxy (e.g., Burp Suite).
* **HTTP Cookies**: Can be manipulated to exploit logic (e.g., discounts).
* **URL Parameters**: Visible and editable directly or via proxy.
* **Referer Headers**: Used for access control, but easily spoofed.

### 2. **Opaque Data**

Data that appears encrypted or obfuscated:

* May contain pricing, tokens, or session-related values.
* Can be replayed, swapped between products, or fuzzed with malformed inputs.
* Example: ASP.NET `__VIEWSTATE` — modifiable if MAC protection is disabled.

### 3. **HTML Form-Based Controls**

* **Max Length**: Input limits like `maxlength=1` can be bypassed by intercepting requests.
* **JavaScript Validation**: Easily neutralized by disabling JS or editing the intercepted request.
* **Disabled Fields**: Not submitted by browsers, but can be added manually in requests.

### 4. **Browser Extension-Based Controls**

Used in advanced clients (Java applets, Flash, Silverlight) to:

* Capture input
* Validate logic
* Transmit serialized or obfuscated data

#### Techniques:

* **Intercepting Serialized Data**: Use tools like Burp extensions (e.g., DSer, AMF parsers).
* **Decompiling Bytecode**: Extract applet (.jar), Flash (.swf), Silverlight (.xap) and analyze logic.
* **Recompiling/Modifying Behavior**: Modify source, recompile, and inject altered behavior via proxy or standalone execution.
* **Obfuscation Handling**: Use deobfuscators or rename variables manually via IDE refactoring.

---

## 🔍 Hacking Steps Summary

* Identify client-side validations and hidden controls.
* Use intercepting proxy to alter:

  * Form fields (visible/hidden)
  * Cookies
  * URL parameters
  * Headers (e.g., Referer)
* Detect and manipulate opaque values.
* Decompile browser extension logic where used.
* Look for inconsistencies between client and server validation.
* Always test whether the server replicates client-side logic.

---

## ⚠️ Common Mistakes to Avoid

* Assuming any client-side control is secure.
* Trusting opaque or "disabled" fields.
* Relying on headers like Referer for access control.
* Assuming serialization makes data tamper-proof.
