## Lab Title: Reflected XSS in canonical link tag  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed inside the `href` attribute of a canonical `<link>` tag.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL  
- **Vulnerable Context:** `<link rel="canonical" href="...">`  
- **The Problem:** The application reflects user input directly into the `href` attribute without sanitization.  

## Exploitation  
By injecting a payload that breaks out of the `href` attribute, arbitrary JavaScript can be executed.  

<img width="1143" height="272" alt="image" src="https://github.com/user-attachments/assets/d9780ccd-6846-4cb7-9f99-e5541b20e5e5" />

### Payload Example  
`https://0a69003504d904978067670700230056.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)`

This payload closes the `href` attribute and injects a `onclick attribute` into the existing `<link>` tag.  

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS  

<img width="1130" height="496" alt="image" src="https://github.com/user-attachments/assets/f4730cf2-e63e-4000-8123-1d31088a3b96" />

<img width="1159" height="405" alt="image" src="https://github.com/user-attachments/assets/58669666-3f6e-47a4-a739-250fe3601e48" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Properly encode user input before placing it in attributes  
- Use frameworks or libraries that automatically escape attribute values  
- Apply Content Security Policy (CSP) to reduce script injection risks  

## Recommendation  
- Sanitize and validate all user inputs  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript)  
- Regularly test for XSS vulnerabilities using automated tools and manual review  
