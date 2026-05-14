## Lab Title: DOM XSS in innerHTML sink using source location.search  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a DOM-based XSS vulnerability where unsanitized data from `location.search` is written into the page using `innerHTML`.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search`  
- **Vulnerable Sink:** `innerHTML`  
- **The Problem:** The application directly injects user-controlled input into the DOM without sanitization, allowing arbitrary script execution.  

## Exploitation  
By manipulating the query string in the URL, a malicious payload was injected. Since the application used `innerHTML` with unsanitized input, the payload executed in the browser.  

<img width="725" height="455" alt="image" src="https://github.com/user-attachments/assets/963f1f9e-409b-4b03-a468-613086c4b442" />

### Payload Example  
`<img src=1 onerror=alert(1)>`

## Verification  
- Modified the URL query string with the payload  
- Reloaded the page  
- Observed the alert box execution, confirming DOM-based XSS  

<img width="844" height="512" alt="image" src="https://github.com/user-attachments/assets/614ab837-0e6b-47fc-b76c-524ac8ad294a" />

<img width="1098" height="383" alt="image" src="https://github.com/user-attachments/assets/352219d0-1ce7-42f8-8f6b-e34bd6613e3e" />


## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Avoid using `innerHTML` with user input  
- Use safe DOM manipulation methods (`textContent`, `innerText`)  
- Sanitize and encode data before rendering  

## Recommendation  
- Implement strict input validation and output encoding  
- Apply Content Security Policy (CSP) to mitigate script injection risks  
- Regularly test for DOM XSS vulnerabilities with automated tools and manual review  

