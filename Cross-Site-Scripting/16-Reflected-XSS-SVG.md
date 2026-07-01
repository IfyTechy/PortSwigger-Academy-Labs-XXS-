## Lab Title: Reflected XSS with some SVG markup allowed  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed into an HTML context. Most tags are blocked, but some SVG markup is allowed, enabling injection.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL  
- **Vulnerable Context:** HTML body content  
- **The Problem:** The application blocks common tags, but certain SVG elements and attributes are permitted. SVG is part of the HTML5 spec, so browsers parse it as valid markup.  

## Exploitation  
By crafting a payload that uses allowed SVG markup, arbitrary JavaScript can be executed. 

<img width="1153" height="147" alt="image" src="https://github.com/user-attachments/assets/678bc9ad-99d7-401e-8d1e-be8c54927ccb" />

### Payload Example  
`https://0aa900f403c86e8f8168343d00ee0034.h1-web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E`

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS despite filtering  

<img width="1074" height="574" alt="image" src="https://github.com/user-attachments/assets/bf5d3dfe-2c2a-4056-849e-4aba5a28da00" />

<img width="1150" height="511" alt="image" src="https://github.com/user-attachments/assets/e7ddb797-c0fd-424a-918e-aa5e11c88f1f" />

## Business Impact  
- Attackers can bypass naive filtering by using SVG markup  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Use proper output encoding for HTML context  
- Avoid blacklisting; instead, apply context‑aware escaping  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Implement robust input validation and output encoding  
- Test filters against known bypass techniques (SVG, MathML, unusual attributes)  
- Regularly audit applications for XSS vulnerabilities  
