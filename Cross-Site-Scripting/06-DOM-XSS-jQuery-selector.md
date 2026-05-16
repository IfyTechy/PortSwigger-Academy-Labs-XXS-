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


<img width="1024" height="524" alt="image" src="https://github.com/user-attachments/assets/737777bc-6d8d-4354-8852-e29c06249b38" />


### Payload Example  
`<iframe src="https://0a410049040a3a4080140d270019006e.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"> </iframe>`

## Verification  
- Modified the URL fragment with the payload  
- Triggered the `hashchange` event by navigating or modifying the hash  
- Observed the alert box execution, confirming DOM-based XSS  

<img width="1044" height="587" alt="image" src="https://github.com/user-attachments/assets/070264e2-d0f5-4c6f-8679-1b1468d0cdd6" />

<img width="1140" height="522" alt="image" src="https://github.com/user-attachments/assets/4b7a3e80-7bc5-4b8e-8ec8-d25f3bbade4b" />

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
