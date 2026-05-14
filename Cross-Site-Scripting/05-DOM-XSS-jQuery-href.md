## Lab Title: DOM XSS in jQuery anchor href attribute sink using location.search  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a DOM-based XSS vulnerability where unsanitized data from `location.search` is used to set the `href` attribute of an anchor tag via jQuery.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search`  
- **Vulnerable Sink:** jQuery `.attr("href", ...)`  
- **The Problem:** The application directly uses user-controlled input to update the anchor’s `href` attribute without sanitization, allowing script injection.  

## Exploitation  
By manipulating the query string in the URL, a malicious payload was injected. Since jQuery wrote the unsanitized input into the anchor’s `href`, the payload executed when the link was clicked.  

<img width="886" height="361" alt="image" src="https://github.com/user-attachments/assets/c3857a3f-4210-4d90-ba1c-a6cbb168122b" />


### Payload Example  
`javascript:alert(document.cookie)`

## Verification  
- Modified the URL query string with the payload  
- Reloaded the page  
- Clicked the anchor link and observed the alert box execution, confirming DOM-based XSS  

<img width="868" height="450" alt="image" src="https://github.com/user-attachments/assets/2a890045-4c9e-41b8-8233-9175156b4eb3" />

<img width="1086" height="474" alt="image" src="https://github.com/user-attachments/assets/243aaccb-c19c-4e8d-bc79-9bb8434b372f" />


## Business Impact  
- Attackers can redirect users to malicious sites or execute arbitrary JavaScript  
- Potential for phishing, credential theft, or malware delivery  

## Remediation  
- Avoid setting `href` attributes directly from user input  
- Validate and sanitize query parameters before use  
- Use safe APIs and enforce strict URL schemes  

## Recommendation  
- Implement input validation and output encoding  
- Restrict anchor links to safe protocols (`http`, `https`)  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

