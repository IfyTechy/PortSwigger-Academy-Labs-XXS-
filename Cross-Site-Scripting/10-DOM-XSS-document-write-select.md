## Lab Title: DOM XSS in document.write sink using source location.search inside a select element  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a DOM-based XSS vulnerability where unsanitized data from `location.search` is written into the page using `document.write` inside a `<select>` element.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search`  
- **Vulnerable Sink:** `document.write`  
- **Vulnerable Context:** `<select>` element options  
- **The Problem:** The application directly injects user-controlled input into the DOM without sanitization, allowing arbitrary script execution.  

## Exploitation  
By manipulating the query string in the URL, a malicious payload was injected. Since the application used `document.write` with unsanitized input inside a `<select>` element, the payload executed in the browser.  

<img width="858" height="279" alt="image" src="https://github.com/user-attachments/assets/922c6192-3bee-4688-be6d-1ddbfff27629" />


### Payload Example  
`<script>alert('hacked')</script>`

## Verification  
- Modified the URL query string with the payload  
- Reloaded the page  
- Observed the alert box execution, confirming DOM-based XSS  

<img width="1363" height="631" alt="image" src="https://github.com/user-attachments/assets/377d7510-9d33-4082-b81f-845933a762fc" />

<img width="1355" height="651" alt="image" src="https://github.com/user-attachments/assets/54918e2f-0528-47fa-beca-ebeacc4fb4c4" />


## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Avoid using `document.write` with user input  
- Use safe DOM manipulation methods (`textContent`, `createElement`)  
- Sanitize and encode data before rendering  

## Recommendation  
- Implement strict input validation and output encoding  
- Apply Content Security Policy (CSP) to mitigate script injection risks  
- Regularly test for DOM XSS vulnerabilities with automated tools and manual review  
