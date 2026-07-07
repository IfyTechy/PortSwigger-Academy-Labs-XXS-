## Lab Title: Reflected XSS with AngularJS sandbox escape without strings  

- **Category:** Cross-Site Scripting / AngularJS Sandbox Escape  
- **Level:** Expert  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how AngularJS applications can be exploited even when common functions like `$eval` are unavailable and string literals are blocked. By abusing prototype manipulation and character code conversion, attackers can escape the AngularJS sandbox and execute arbitrary JavaScript.  

## Vulnerability Context  
- **Entry Point:** Search query parameter.  
- **Reflection Point:** AngularJS expression evaluation in the page.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** Unsafe use of AngularJS expressions without proper sandboxing or sanitization.  

## Exploit Technique  
The exploit leverages several steps:  
1. **String creation without quotes:** Use `toString()` to generate a string object without directly writing string literals.  
2. **Prototype manipulation:** Overwrite the `charAt` function on the `String` prototype via `toString().constructor.prototype.charAt = [].join;`.  
3. **Sandbox escape:** This breaks AngularJS’s sandbox restrictions, allowing forbidden operations.  
4. **Payload generation:** Use `String.fromCharCode()` to build the payload `x=alert(1)` from character codes.  
5. **Execution:** Pass the payload into the `orderBy` filter, which evaluates the expression.  

### Example Exploit URL  
`https://YOUR-LAB-ID.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1`

<img width="1108" height="344" alt="image" src="https://github.com/user-attachments/assets/f10d7f7f-993f-49c5-ac61-d1cc3a166921" />

- The payload constructs `x=alert(1)` without using quotes.  
- The overwritten `charAt` function allows AngularJS to accept the expression.  
- The `orderBy` filter evaluates the payload, triggering the alert.  

## Verification  
- Injected the payload via the search parameter.  
- Reloaded the page with the crafted URL.  
- Observed the alert box execution, confirming sandbox escape and reflected XSS.  

<img width="1158" height="584" alt="image" src="https://github.com/user-attachments/assets/e6ea298e-0e9d-4746-8a86-eda2b188be5b" />

## Security Impact  
- **Sandbox Bypass:** Attackers can break AngularJS’s intended restrictions.  
- **Arbitrary Code Execution:** Full control of client‑side execution.  
- **High Severity:** Demonstrates that even advanced defenses can be bypassed with prototype abuse.  

## Recommended Fixes  
- Avoid evaluating user input with AngularJS expressions.  
- Use AngularJS’s `$sanitize` service or trusted libraries to safely render dynamic content.  
- Apply Content Security Policy (CSP) to restrict script execution.  
- Regularly update frameworks to patch known sandbox escape techniques.  

## Lessons Learned  
This lab highlights the risks of relying solely on AngularJS’s sandbox for security. Even without strings and `$eval`, attackers can exploit prototype manipulation and character code conversion to execute arbitrary JavaScript. Secure coding practices and defense‑in‑depth measures are essential to prevent such advanced XSS attacks.



