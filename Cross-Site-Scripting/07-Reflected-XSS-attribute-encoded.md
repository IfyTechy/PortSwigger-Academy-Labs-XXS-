## Lab Title: Reflected XSS into attribute with angle brackets HTML-encoded  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is reflected inside an HTML attribute. Although angle brackets are HTML‑encoded, other characters remain unsanitized, allowing injection.  

## Discovery & Analysis  
- **Vulnerable Parameter:** Query string parameter in the URL  
- **Vulnerable Context:** HTML attribute value  
- **The Problem:** Angle brackets are encoded, but quotes and event handlers are not properly sanitized.  

## Exploitation  
By injecting a payload that breaks out of the attribute value, arbitrary JavaScript can be executed.  

<img width="810" height="348" alt="image" src="https://github.com/user-attachments/assets/0968546c-e568-463d-8059-17f6d502ebb0" />

### Payload Example  
`"onmouseover="alert(1)`

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS  

<img width="681" height="458" alt="image" src="https://github.com/user-attachments/assets/feb8d76c-1f06-4c63-96ba-23d6bacf4919" />

<img width="1083" height="375" alt="image" src="https://github.com/user-attachments/assets/49dc30ef-5d97-4986-95d1-96565cb90e33" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Properly encode all user input before placing it in attributes  
- Use frameworks or libraries that automatically escape attribute values  
- Apply Content Security Policy (CSP) to reduce script injection risks  

## Recommendation  
- Sanitize and validate all user inputs  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Regularly test for XSS vulnerabilities using automated tools and manual review  

