## Lab Title: Reflected XSS with AngularJS sandbox escape and CSP  

- **Category:** Cross-Site Scripting / AngularJS Sandbox Escape / CSP Bypass  
- **Level:** Expert  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how AngularJS applications can be exploited even when Content Security Policy (CSP) is enforced. By abusing AngularJS event handling and filter mechanics, attackers can bypass CSP restrictions, escape the AngularJS sandbox, and execute arbitrary JavaScript.  

## Vulnerability Context  
- **Entry Point:** Search query parameter.  
- **Reflection Point:** AngularJS expression evaluation in the page.  
- **Impact:** Arbitrary JavaScript execution in the victim’s browser.  
- **Security Weakness:** Unsafe use of AngularJS expressions combined with CSP that is insufficiently restrictive.  

## Exploit Technique  
The exploit leverages several AngularJS features:  
1. **Event binding:** Use `ng-focus` to trigger execution when the input element gains focus.  
2. **Event object reference:** `$event` provides access to the event object.  
3. **Path traversal:** `$event.composedPath()` (or `$event.path` in Chrome) contains an array of elements that triggered the event, ending with the `window` object.  
4. **Filter abuse:** The `|` operator invokes AngularJS filters. Here, `orderBy` is used with a crafted argument.  
5. **Function assignment:** Instead of calling `alert` directly, assign it to a variable (`z=alert`).  
6. **Execution:** When the filter evaluates the window object, the assigned function executes in the global scope, bypassing AngularJS’s window restrictions and CSP.  

### Example Exploit URL  
`https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x (your-lab-id.web-security-academy.net in Bing)`

<img width="1116" height="530" alt="image" src="https://github.com/user-attachments/assets/b38d9b2a-7247-4275-8940-9c88b42b27b1" />

- The payload injects an `<input>` element with `ng-focus`.  
- When focused, AngularJS evaluates the expression.  
- The `orderBy` filter argument executes `alert(document.cookie)` in the window scope.  
- CSP is bypassed because the payload does not rely on inline scripts or external resources blocked by CSP.  

## Verification  
- Injected the payload via the search parameter.  
- Reloaded the page with the crafted URL.  
- Focused the input element.  
- Observed the alert box execution, confirming CSP bypass and sandbox escape.  

<img width="1169" height="519" alt="image" src="https://github.com/user-attachments/assets/a3591d44-3c12-43bd-b99c-cf9e33bacc92" />

## Security Impact  
- **CSP Bypass:** Attackers can execute scripts despite CSP restrictions.  
- **Sandbox Escape:** AngularJS’s intended protections are broken.  
- **Arbitrary Code Execution:** Full control of client‑side execution.  
- **High Severity:** Demonstrates that CSP alone is not sufficient if unsafe frameworks are used.  

## Recommended Fixes  
- Avoid evaluating user input with AngularJS expressions.  
- Use AngularJS’s `$sanitize` service or trusted libraries to safely render dynamic content.  
- Apply strict CSP that disallows inline event handlers and unsafe dynamic code execution.  
- Regularly update frameworks to patch known sandbox escape techniques.  

## Lessons Learned  
This lab highlights how CSP can be bypassed when combined with unsafe AngularJS usage. Attackers can exploit event objects and filter mechanics to execute arbitrary JavaScript, even in environments thought to be protected. Defense‑in‑depth is essential: preventing XSS at the source is more effective than relying solely on CSP.


