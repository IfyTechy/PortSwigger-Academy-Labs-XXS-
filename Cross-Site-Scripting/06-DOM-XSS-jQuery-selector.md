## Lab Title: DOM XSS in jQuery selector sink using a hashchange event  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a DOM-based XSS vulnerability where unsanitized data from the URL fragment (`location.hash`) is passed into a jQuery selector during a `hashchange` event.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.hash`  
- **Vulnerable Sink:** jQuery selector (`$(location.hash)`)  
- **Trigger:** `hashchange` event  
- **The Problem:** The application uses user-controlled input directly in a jQuery selector without sanitization, allowing arbitrary script execution.  

## Exploitation  
By manipulating the fragment identifier in the URL, a malicious payload was injected. Since the application used jQuery selectors with unsanitized input, the payload executed when the `hashchange` event fired.  

### Payload Example  


## Verification  
- Modified the URL fragment with the payload  
- Triggered the `hashchange` event by navigating or modifying the hash  
- Observed the alert box execution, confirming DOM-based XSS  

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Avoid using user input directly in jQuery selectors  
- Validate and sanitize `location.hash` before use  
- Use safe DOM APIs instead of dynamic selectors  

## Recommendation  
- Implement strict input validation and output encoding  
- Apply Content Security Policy (CSP) to mitigate script injection risks  
- Regularly test for DOM XSS vulnerabilities with automated tools and manual review  
