## Lab Title: DOM XSS in document.write sink using source location.search  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
The goal was to exploit a DOM-based XSS vulnerability where unsanitized data from `location.search` is written directly into the page using `document.write`.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search`  
- **Vulnerable Sink:** `document.write`  
- **The Problem:** The application directly writes user-controlled input into the DOM without sanitization, allowing arbitrary script execution.  

## Exploitation  
By manipulating the query string in the URL, a malicious payload was injected. Since the application used `document.write` with unsanitized input, the payload executed in the browser.  

<img width="778" height="351" alt="WhatsApp Image 2026-05-14 at 00 53 32" src="https://github.com/user-attachments/assets/e57f9f9d-283d-4128-a474-3c5f7bdd8d7b" />


### Payload 
`"><svg onload=alert(1)>`

## Verification  
- Modified the URL query string with the payload  
- Reloaded the page  
- Observed the alert box execution, confirming DOM-based XSS  

<img width="905" height="437" alt="WhatsApp Image 2026-05-14 at 00 51 29" src="https://github.com/user-attachments/assets/b9996408-3732-4f52-900b-97ec3aecc04f" />

<img width="1077" height="329" alt="WhatsApp Image 2026-05-14 at 00 54 25" src="https://github.com/user-attachments/assets/a407ad6c-65e8-4c83-a6bb-e4c8f37a8ce0" />


## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Avoid using `document.write` with user input  
- Use safe DOM manipulation methods (e.g., `textContent`, `innerText`)  
- Sanitize and encode data before rendering  

## Recommendation  
- Implement strict input validation and output encoding  
- Apply Content Security Policy (CSP) to mitigate script injection risks  
- Regularly test for DOM XSS vulnerabilities with automated tools and manual review  

