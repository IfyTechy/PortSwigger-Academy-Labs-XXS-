## Lab Title: Reflected XSS in a JavaScript URL with some characters blocked  

- **Category:** Cross-Site Scripting  
- **Level:** Expert  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how attackers can exploit reflected XSS even when certain characters are blocked in a JavaScript URL context. By combining exception handling, arrow functions, and creative use of comments, it is possible to bypass restrictions and execute arbitrary JavaScript.  

## Vulnerability Context  
- **Entry Point:** Query string parameter in the blog post URL.  
- **Reflection Point:** Inside a JavaScript URL context.  
- **Impact:** Arbitrary JavaScript execution when the victim clicks a link.  
- **Security Weakness:** Incomplete character blocking — spaces and certain operators are restricted, but advanced constructs can bypass these filters.  

## Exploit Technique  
The exploit relies on several advanced JavaScript features:  
1. **Arrow functions:** Used to create a block scope where `throw` can be used.  
2. **Exception handling:** Assign `alert` to the `onerror` handler, then trigger it via `throw`.  
3. **Comment injection:** Use `/**/` to bypass restrictions on spaces.  
4. **String conversion:** Assign the function to `window.toString` and force a string conversion to trigger execution.  
5. **Payload construction:** Ensure the alert message contains `1337` as required by the lab.  

### Example Exploit URL  
`https://YOUR-LAB-ID.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27`

<img width="1165" height="480" alt="image" src="https://github.com/user-attachments/assets/4b9d48a9-76fe-489d-b796-588205418321" />

- The payload uses `throw/**/onerror=alert,1337` to trigger the alert.  
- Arrow functions allow `throw` to be used inside an expression.  
- `toString` is overwritten to call the malicious function when `window` is coerced into a string.  
- The alert displays `1337`, satisfying the lab requirement.  

## Verification  
- Injected the payload via the search parameter.  
- Reloaded the page with the crafted URL.  
- Clicked “Back to blog” at the bottom of the page.  
- Observed the alert box execution with `1337`, confirming reflected XSS.  

<img width="1166" height="505" alt="image" src="https://github.com/user-attachments/assets/efa65398-e6a2-4726-8044-73c3fada3eac" />

## Security Impact  
- **Bypass of Character Blocking:** Attackers can exploit advanced JavaScript constructs to evade filters.  
- **Arbitrary Code Execution:** Victims unknowingly execute attacker‑supplied JavaScript.  
- **High Severity:** Demonstrates that simple character blocking is insufficient to prevent XSS.  

## Recommended Fixes  
- Avoid reflecting unsanitized user input into JavaScript contexts.  
- Use strict input validation and context‑aware output encoding.  
- Apply Content Security Policy (CSP) to restrict script execution.  
- Employ defense‑in‑depth measures rather than relying on character blocking.  

## Lessons Learned  
This lab highlights how attackers can bypass naive character blocking in JavaScript contexts. Advanced features like arrow functions, prototype manipulation, and exception handling can be abused to achieve XSS. Comprehensive sanitization and secure design are essential to prevent such attacks.
