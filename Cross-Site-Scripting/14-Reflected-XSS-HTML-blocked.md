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

<img width="1366" height="590" alt="image" src="https://github.com/user-attachments/assets/68a4eff4-828a-42a1-9d25-d578656ba948" />

<img width="1359" height="581" alt="image" src="https://github.com/user-attachments/assets/2b712c3e-b6a6-4ac4-bd03-8d25e2d7dc75" />


### Payload Example  
`<iframe src="https://0a1e00b90337116e811111f700c80095.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>"`

<img width="1344" height="545" alt="image" src="https://github.com/user-attachments/assets/7d115427-80aa-4096-ad9d-17b939b36b00" />

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming reflected XSS despite filtering  

<img width="1364" height="510" alt="image" src="https://github.com/user-attachments/assets/bbc93a5e-eaf5-468e-b5c2-fe86bcc4537b" />

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
