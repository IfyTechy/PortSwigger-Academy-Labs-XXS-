## Lab Title: Stored XSS in comment section  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective 
The goal was to exploit a stored XSS vulnerability in the comment functionality.  

## Discovery & Analysis 
- **Vulnerable Parameter:** The comment input field  
- **The Problem:** User input is stored and rendered without sanitization, allowing persistent payload execution

 ## Exploitation 
Exploitation was achieved by injecting a malicious XSS payload into the comment input field. Because the application failed to sanitize user input, the payload was stored and executed whenever the page was viewed, resulting in persistent script execution in the browser.

<img width="721" height="585" alt="image" src="https://github.com/user-attachments/assets/8ee98d9f-3974-4a0d-95bd-7426514d9d9a" />


 ### Payload
`<script>alert(1)</script>`

## Verification
- Inserted the XSS payload into the comment input field
- Submitted the comment
- Reloaded the page and observed the payload executing persistently
- Result: The application executed unsanitized user input in the HTML context, confirming the presence of a stored Cross-Site Scripting (XSS) vulnerability.

   <img width="1084" height="540" alt="image" src="https://github.com/user-attachments/assets/2a91d551-7e0c-4f66-9bec-6bf93171ca2c" />

 ## Business Impact
- Attackers could inject malicious scripts that steal session cookies, deface the site, or deliver malware.
- Persistent execution means every user viewing the page is at risk.

## Remediation
- Implement proper input validation and output encoding.
- Use frameworks or libraries that automatically handle HTML escaping.
- Apply Content Security Policy (CSP) to reduce script injection risks.

## Recommendation
- Sanitize all user inputs before storing them.
- Encode outputs when rendering dynamic content.
- Regularly test for XSS vulnerabilities using automated tools and manual penetration testing.

