## Lab Title: Reflected XSS protected by CSP, with CSP bypass  

- **Category:** Cross-Site Scripting / CSP Bypass  
- **Level:** Expert  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how attackers can exploit reflected XSS even when a Content Security Policy (CSP) is enforced. By abusing the `report-uri` directive and injecting custom CSP rules, it is possible to bypass restrictions and execute arbitrary JavaScript.  

## Vulnerability Context  
- **Entry Point:** Search query parameter.  
- **Reflection Point:** Inside the page’s HTML.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** CSP is present but improperly implemented, allowing attacker‑controlled injection into the policy via the `token` parameter.  

## Exploit Technique  
The attack proceeds in stages:  
1. **Initial attempt blocked:** Injecting `<img src=1 onerror=alert(1)>` shows reflection but CSP prevents execution.  
2. **Observation:** The response contains a CSP header with a `report-uri` directive that includes a `token` parameter.  
3. **Injection:** Because the attacker controls the `token` parameter, they can inject new CSP directives.  
4. **Bypass:** Inject `script-src-elem 'unsafe-inline'` to override existing rules and allow inline scripts.  
5. **Execution:** With inline scripts permitted, a simple `<script>alert(1)</script>` payload executes.

---

**Figure 1: Verifying the reflection of an injected HTML image tag contextually breaking out of the application's search filter rendering.**

<img width="1156" height="539" alt="image" src="https://github.com/user-attachments/assets/d800571a-c71b-4bdf-bc67-645bb4b2baac" />

---

### Example Exploit URL  
`https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cscript%3Ealert(1)%3C%2Fscript%3E&token=;script-src-elem%20%27unsafe-inline%27`

---

**Figure 2: Injected a reflected XSS payload featuring policy-allowed tokens directly via the URL search parameter to target a CSP bypass.**

<img width="1163" height="338" alt="image" src="https://github.com/user-attachments/assets/e749340e-0867-484c-bd8d-7047bda6c03f" />

- The injected directive modifies CSP to allow inline scripts.  
- The reflected `<script>` payload executes, triggering the alert.  
- This bypass works specifically in Chrome due to its handling of CSP directives.

---

## Verification  
- Injected the payload via the search parameter.  
- Reloaded the page with the crafted URL.  
- Observed the alert box execution, confirming CSP bypass and reflected XSS.  

**Figure 3: Successful execution of the injected XSS payload triggering an alert pop-up box with a value of 1.**

<img width="1149" height="469" alt="image" src="https://github.com/user-attachments/assets/7e11275a-15b5-4576-81fd-5ce899981891" />

---

**Figure 4: Verifying successful execution of the CSP bypass payload, confirming full resolution of the lab.**

<img width="1165" height="523" alt="image" src="https://github.com/user-attachments/assets/2bc83f1f-a280-4ebf-8b10-d9e487377f19" />


## Security Impact  
- **CSP Bypass:** Attackers can override CSP rules by injecting new directives.  
- **Arbitrary Code Execution:** Victims unknowingly execute attacker‑supplied JavaScript.  
- **High Severity:** Demonstrates that CSP must be carefully implemented; improper use of dynamic parameters can undermine its protections.  

## Recommended Fixes  
- Do not include attacker‑controlled parameters in CSP directives.  
- Apply strict CSP rules without dynamic injection.  
- Encode and sanitize all user input before rendering.  
- Use defense‑in‑depth measures such as input validation, output encoding, and secure frameworks.  

## Lessons Learned  
This lab highlights how CSP can be bypassed if improperly implemented. Allowing user input to influence CSP directives creates a direct path for attackers to weaken or disable protections. CSP must be applied consistently and securely, without relying on dynamic parameters.

