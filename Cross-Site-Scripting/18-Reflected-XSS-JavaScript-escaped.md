## Lab Title: Reflected XSS into a JavaScript string with single quote and backslash escaped  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed inside a JavaScript string. Single quotes and backslashes are escaped, preventing simple string‑breaking payloads. The goal is to bypass this escaping and execute arbitrary JavaScript.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL  
- **Vulnerable Context:** JavaScript string literal inside a `<script>` block  
- **The Problem:** Escaping single quotes (`'`) and backslashes (`\`) prevents breaking out of the string directly. However, the input is still reflected inside the script block, so context‑breaking attacks are possible.  

## Exploitation  

### Attempt 1 (failed)  
#### Payload:  
`test'payload`

<img width="937" height="562" alt="image" src="https://github.com/user-attachments/assets/cdc30f34-3bb4-409d-b681-4d140475e43d" />


- Result: The single quote was backslash‑escaped (`\'`), so the string remained intact. No execution occurred.  

### Attempt 2 (successful)  
#### Payload:  
`</script><script>alert(1)</script>`

<img width="964" height="563" alt="image" src="https://github.com/user-attachments/assets/d3124873-408b-43f4-814c-423dc1bbe8e2" />


- This closes the existing `<script>` block and opens a new one containing the payload.  
- Escaping rules inside the original string no longer matter, because the context has been broken.  
- When the page loads, the injected script executes and triggers the alert.  

## Verification  
- Sent the payload via Burp Repeater.  
- Copied the generated URL and loaded it in the browser.  
- Observed the alert box execution, confirming reflected XSS.

<img width="1146" height="550" alt="image" src="https://github.com/user-attachments/assets/afd3a132-75c4-4b0f-aad0-5e6672923e9b" />
   
<img width="1163" height="541" alt="image" src="https://github.com/user-attachments/assets/7b097257-e793-43d1-bdca-83d751ac5dfc" />

## Business Impact  
- Attackers can bypass naive escaping by breaking out of the script block entirely.  
- Arbitrary JavaScript execution enables session hijacking, credential theft, or malicious redirection.  

## Remediation  
- Properly escape and encode user input before placing it in JavaScript contexts.  
- Avoid dynamic string concatenation with unsanitized input.  
- Apply Content Security Policy (CSP) to reduce script injection risks.  

## Recommendation  
- Sanitize and validate all user inputs.  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript).  
- Regularly test for XSS vulnerabilities using automated tools and manual review.  

