## Lab Title: Reflected XSS into a JavaScript string with angle brackets HTML-encoded  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed inside a JavaScript string. Although angle brackets are HTML‑encoded, other characters remain unsanitized, allowing injection.  

## Discovery & Analysis  
- **Vulnerable Parameter:** Query string parameter in the URL  
- **Vulnerable Context:** JavaScript string literal  
- **The Problem:** Angle brackets are encoded, but quotes and escape sequences are not properly sanitized.  

## Exploitation  
By injecting a payload that breaks out of the JavaScript string, arbitrary JavaScript can be executed.  

<img width="781" height="241" alt="image" src="https://github.com/user-attachments/assets/74e2d694-4673-49fa-aa5a-6295346bed54" />

### Payload Example  
`'-alert(1)-'`

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS  

<img width="655" height="369" alt="image" src="https://github.com/user-attachments/assets/6653371e-888f-4aec-b89b-62908684f884" />

<img width="1105" height="373" alt="image" src="https://github.com/user-attachments/assets/53c4f16f-d17f-4fd2-a019-665dc1d4b429" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Properly escape and encode user input before placing it in JavaScript contexts  
- Use safe APIs and avoid dynamic string concatenation with user input  
- Apply Content Security Policy (CSP) to reduce script injection risks  

## Recommendation  
- Sanitize and validate all user inputs  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Regularly test for XSS vulnerabilities using automated tools and manual review  
