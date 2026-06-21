## Lab Title: Reflected XSS into HTML context with most tags and attributes blocked  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed into an HTML context. Although most tags and attributes are blocked, certain bypasses remain possible.  

## Discovery & Analysis  
- **Vulnerable Parameter:** Query string parameter in the URL  
- **Vulnerable Context:** HTML body content  
- **The Problem:** The application blocks common tags and attributes, but incomplete filtering allows injection through overlooked vectors.  

## Exploitation  
By crafting a payload that uses an allowed tag or attribute, arbitrary JavaScript can still be executed.  

### Payload Example  


## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS despite filtering  

## Business Impact  
- Attackers can bypass naive filtering and execute arbitrary JavaScript  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Use proper output encoding for HTML context  
- Avoid blacklisting; instead, apply context‑aware escaping  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Implement robust input validation and output encoding  
- Test filters against known bypass techniques (SVG, MathML, unusual attributes)  
- Regularly audit applications for XSS vulnerabilities  
