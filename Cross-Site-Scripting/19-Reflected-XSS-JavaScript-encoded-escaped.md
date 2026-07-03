## Lab Title: Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed inside a JavaScript string. Angle brackets (`< >`) and double quotes (`"`) are HTML‑encoded, and single quotes (`'`) are escaped. The goal is to bypass these protections and execute arbitrary JavaScript.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL  
- **Vulnerable Context:** JavaScript string literal inside a `<script>` block  
- **The Problem:**  
  - Angle brackets and double quotes are HTML‑encoded, preventing direct tag injection.  
  - Single quotes are escaped (`\'`), preventing simple string‑breaking payloads.  
  - Backslashes (`\`) are **not escaped**, leaving a bypass vector.  

## Exploitation  

### Attempt 1 (failed)  
Payload:  
`test'payload`

<img width="1054" height="554" alt="image" src="https://github.com/user-attachments/assets/cc1ad5d8-7028-4949-bfdf-45efb4cd40ba" />

- Result: The single quote was escaped (`\'`), so the string remained intact.  

### Attempt 2 (failed)  
Payload:  
`test\payload`

<img width="885" height="538" alt="image" src="https://github.com/user-attachments/assets/487ee0d3-eeed-419b-a2c1-c8af8bc415e2" />

- Result: The backslash was not escaped, but this alone didn’t break out of the string.  

### Attempt 3 (successful)  
Payload:  
`\'-alert(1)//`

<img width="965" height="555" alt="image" src="https://github.com/user-attachments/assets/7c95fbf5-baab-49d4-8a78-7656ef9aaff0" />

- The leading backslash neutralizes the escaping mechanism.  
- The `'-` breaks out of the string safely.  
- `alert(1)` executes immediately.  
- The trailing `//` comments out the remainder of the line to avoid syntax errors.  

## Verification  
- Sent the payload via Burp Repeater.  
- Copied the generated URL and loaded it in the browser.  
- Observed the alert box execution, confirming reflected XSS.  

<img width="1157" height="507" alt="image" src="https://github.com/user-attachments/assets/4070b3e5-c81b-48fe-9c47-560502b90377" />

<img width="1146" height="547" alt="image" src="https://github.com/user-attachments/assets/ef88f78c-e9fb-4a4c-a6a3-f31cace412d1" />

## Business Impact  
- Attackers can bypass naive escaping by exploiting inconsistencies (escaped quotes vs. unescaped backslashes).  
- Arbitrary JavaScript execution enables session hijacking, credential theft, or malicious redirection.  

## Remediation  
- Properly escape and encode user input before placing it in JavaScript contexts.  
- Avoid dynamic string concatenation with unsanitized input.  
- Apply Content Security Policy (CSP) to reduce script injection risks.  

## Recommendation  
- Sanitize and validate all user inputs.  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript).  
- Regularly test for XSS vulnerabilities using automated tools and manual review.  

