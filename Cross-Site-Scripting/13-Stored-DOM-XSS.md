## Lab Title: Stored DOM XSS  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a stored DOM XSS vulnerability where user input is saved by the application and later processed unsafely by client-side JavaScript, ultimately reaching a dangerous sink.  

## Discovery & Analysis  
- **Vulnerable Source:** User-submitted data (e.g., comments, profile fields)  
- **Vulnerable Sink:** DOM methods (`innerHTML`, `document.write`, or jQuery functions)  
- **The Problem:** The application stores user input and later injects it into the DOM without sanitization, allowing persistent script execution.  

## Exploitation  
By submitting a malicious payload into the vulnerable input field, arbitrary JavaScript can be executed whenever the stored content is loaded and processed by the client-side script.  

<img width="1350" height="599" alt="image" src="https://github.com/user-attachments/assets/07a56509-4913-401f-b257-b427c1db60b0" />

### Payload Example  
`<><img src=1 onerror=alert(1)>`

## Verification  
- Submitted the payload into the vulnerable input field  
- Reloaded the page  
- Observed the alert box execution, confirming stored DOM XSS  

<img width="1366" height="732" alt="image" src="https://github.com/user-attachments/assets/c0beceb7-bb4b-4c57-b925-d0aa45b7052e" />

<img width="1366" height="732" alt="image" src="https://github.com/user-attachments/assets/9bce7217-e624-49af-b718-3f9ddb824526" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in every visitor’s browser  
- Persistent execution means all users viewing the page are at risk  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Properly sanitize and validate user input before storing  
- Avoid unsafe DOM methods with user-controlled data  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Use safe DOM APIs (`textContent`, `createElement`)  
- Regularly test for stored DOM XSS vulnerabilities using automated tools and manual review  
