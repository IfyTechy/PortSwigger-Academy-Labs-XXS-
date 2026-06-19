## Lab Title: Reflected DOM XSS  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected DOM XSS vulnerability where user input is reflected by the server and then processed unsafely by client-side JavaScript, ultimately reaching a dangerous sink.  

## Discovery & Analysis  
- **Vulnerable Source:** Reflected request parameter  
- **Vulnerable Sink:** Client-side script (e.g., `document.write`, `innerHTML`)  
- **The Problem:** The server echoes user input, and the client script injects it into the DOM without sanitization.  

## Exploitation  
By injecting a payload into the reflected parameter, arbitrary JavaScript can be executed when the page processes the input.  

<img width="1366" height="732" alt="image" src="https://github.com/user-attachments/assets/f87fe6f8-8268-455a-96d7-90b60227f21f" />

### Payload Example  
`\"-alert(1)}//`

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected DOM XSS

<img width="1366" height="732" alt="image" src="https://github.com/user-attachments/assets/2c186b0c-0c88-4470-b548-329db148dbb6" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Avoid unsafe DOM methods (`document.write`, `innerHTML`) with user input  
- Sanitize and encode reflected data before rendering  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Validate and sanitize all user inputs  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Regularly test for DOM XSS vulnerabilities using automated tools and manual review  
