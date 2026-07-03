## Lab Title: Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a stored XSS vulnerability where user input is saved and later reflected inside an `onclick` event handler attribute. Angle brackets (`< >`) and double quotes (`"`) are HTML‑encoded, while single quotes (`'`) and backslashes (`\`) are escaped. The goal is to bypass these protections and execute arbitrary JavaScript.  

## Discovery & Analysis  
- **Vulnerable Source:** User‑submitted input in the "Website" field of the comment form.  
- **Vulnerable Context:** Reflected inside an `onclick` attribute of the rendered HTML.  
- **The Problem:**  
  - `< >` and `"` are encoded, preventing direct tag injection.  
  - `'` and `\` are escaped, preventing simple breaking out.  
  - However, by injecting a crafted JavaScript URL, the escaping can be bypassed.  

## Exploitation  

### Attempt 1 (failed)  
#### Payload:  
`test'payload`

<img width="967" height="566" alt="image" src="https://github.com/user-attachments/assets/0b8af7d8-f412-4306-bebd-a3ced89b9cf4" />

- Result: Single quote was escaped (`\'`), so the attribute remained intact.  

### Attempt 2 (successful)  
Payload:  
`http://foo?&apos;-alert(1)-&apos;`

<img width="768" height="522" alt="image" src="https://github.com/user-attachments/assets/7e090763-b9bc-4c3e-aa95-e312357c74d2" />

- The `&apos;` entity is decoded into a single quote in the attribute context.  
- This breaks out of the string safely.  
- `alert(1)` executes when the victim clicks the rendered link.  
- The trailing `-` ensures the syntax remains valid.  

## Verification  
- Submitted the payload into the "Website" field.  
- Reloaded the page and clicked the name above the comment.  
- Observed the alert box execution, confirming stored XSS.  

<img width="1164" height="504" alt="image" src="https://github.com/user-attachments/assets/a66b69a2-4cc3-42d8-9249-c4a140b27000" />

## Business Impact  
- Attackers can persistently inject JavaScript into the application.  
- Every user who clicks the vulnerable element triggers the malicious payload.  
- Potential for session hijacking, credential theft, or malicious redirection.  

## Remediation  
- Properly escape and encode user input before placing it in event handler attributes.  
- Avoid inline event handlers (`onclick`, `onload`, etc.) with user‑controlled data.  
- Apply Content Security Policy (CSP) to reduce script injection risks.  

## Recommendation  
- Sanitize and validate all user inputs.  
- Encode outputs correctly depending on context (HTML, attribute, JavaScript).  
- Use safe event binding methods (e.g., `addEventListener`) instead of inline handlers.  
- Regularly test for stored XSS vulnerabilities using automated tools and manual review.  

