## Lab Title: Stored XSS into anchor href attribute with double quotes HTML-encoded  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a stored XSS vulnerability where user input is reflected into an anchor tag’s `href` attribute. Although double quotes are HTML‑encoded, other characters remain unsanitized, allowing injection.  

## Discovery & Analysis  
- **Vulnerable Parameter:** Comment or input field storing user data  
- **Vulnerable Context:** Anchor `href` attribute  
- **The Problem:** Double quotes are encoded, but angle brackets and JavaScript schemes are not properly sanitized.  

## Exploitation  
By injecting a payload that uses a JavaScript scheme in the `href` attribute, arbitrary JavaScript can be executed when the link is clicked.  

<img width="800" height="353" alt="image" src="https://github.com/user-attachments/assets/cff4f443-bbf2-4b42-8d14-561d0a0245ef" />

### Payload Example  
`javascript:alert(1)`

## Verification  
- Inserted the payload into the vulnerable input field  
- Submitted the comment  
- Reloaded the page and clicked the anchor link  
- Observed the alert box execution, confirming stored XSS  

<img width="780" height="351" alt="image" src="https://github.com/user-attachments/assets/ef33c4cf-073e-4886-b730-c54f44febc26" />

<img width="1080" height="413" alt="image" src="https://github.com/user-attachments/assets/984d8286-670b-4c21-8448-af9e8ed3daa2" />

## Business Impact  
- Attackers can redirect users to malicious sites or execute arbitrary JavaScript  
- Persistent execution means every user viewing the page is at risk  

## Remediation  
- Properly sanitize and validate user input before storing  
- Restrict anchor links to safe protocols (`http`, `https`)  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Use safe DOM APIs and libraries that escape attribute values  
- Regularly test for stored XSS vulnerabilities using automated tools and manual review  
