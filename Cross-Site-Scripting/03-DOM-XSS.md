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

### Payload 



## Verification  
- Modified the URL query string with the payload  
- Reloaded the page  
- Observed the alert box execution, confirming DOM-based XSS  

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

