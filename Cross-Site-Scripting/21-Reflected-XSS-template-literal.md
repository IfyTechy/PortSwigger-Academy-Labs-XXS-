## Lab Title: Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed inside a JavaScript template literal. Angle brackets (`< >`), single quotes (`'`), double quotes (`"`), backslashes (`\`), and backticks (`` ` ``) are Unicode‑escaped, preventing direct injection. The goal is to bypass these protections and execute arbitrary JavaScript.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL.  
- **Vulnerable Context:** Inside a JavaScript template string (`` `...${input}...` ``).  
- **The Problem:**  
  - Special characters are escaped, preventing direct breaking out.  
  - However, template literals support **expression interpolation** using `${...}`, which is not escaped.  
  - This allows arbitrary JavaScript execution inside the template string.  

## Exploitation  

### Successful Payload  
`${alert(1)}`

<img width="1138" height="586" alt="image" src="https://github.com/user-attachments/assets/b24a24e4-0ec3-42d2-b9c8-415b84b3d6a8" />

- `${...}` is evaluated inside the template literal.  
- `alert(1)` executes immediately when the script runs.  
- Escaping of quotes, brackets, and backslashes does not matter, because the payload leverages the interpolation feature.  

## Verification  
- Sent the payload via Burp Repeater.  
- Copied the generated URL and loaded it in the browser.  
- Observed the alert box execution, confirming reflected XSS.

<img width="1327" height="501" alt="image" src="https://github.com/user-attachments/assets/d39fd2e9-7967-411a-b22d-8c9881ef080f" />

<img width="1350" height="395" alt="image" src="https://github.com/user-attachments/assets/5991254a-e3af-47fb-8634-8ae7336b8e90" />

## Business Impact  
- Attackers can bypass escaping by exploiting template literal interpolation.  
- Arbitrary JavaScript execution enables session hijacking, credential theft, or malicious redirection.  

## Remediation  
- Properly sanitize and encode user input before placing it inside template literals.  
- Avoid directly embedding unsanitized input in JavaScript code.  
- Apply Content Security Policy (CSP) to reduce script injection risks.  

## Recommendation  
- Sanitize and validate all user inputs.  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript).  
- Regularly test for XSS vulnerabilities using automated tools and manual review.  
