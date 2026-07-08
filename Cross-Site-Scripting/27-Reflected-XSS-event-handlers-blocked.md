## Lab Title: Reflected XSS with event handlers and href attributes blocked  

- **Category:** Cross-Site Scripting  
- **Level:** Expert  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates how attackers can exploit reflected XSS even when event handlers (such as `onclick`) and anchor `href` attributes are blocked. By leveraging whitelisted tags and SVG animation, it is possible to inject a clickable vector that executes JavaScript when triggered.  

## Vulnerability Context  
- **Entry Point:** Search query parameter.  
- **Reflection Point:** HTML rendered in the page.  
- **Impact:** Arbitrary JavaScript execution when the victim clicks the injected element.  
- **Security Weakness:** Incomplete sanitization — event attributes and anchor `href` are blocked, but SVG animation attributes are allowed.  

## Exploit Technique  
The attack relies on SVG’s ability to animate attributes:  
1. Inject an `<svg>` element containing an `<a>` tag.  
2. Use `<animate>` to modify the `href` attribute dynamically.  
3. Set the `values` attribute to a JavaScript payload (`javascript:alert(1)`).  
4. Add a `<text>` element labeled “Click me” to entice the victim to click.  
5. When clicked, the anchor executes the injected JavaScript.  

### Example Exploit URL  
`https://YOUR-LAB-ID.web-security-academy.net/?search=%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E`

<img width="1164" height="569" alt="image" src="https://github.com/user-attachments/assets/2bb54392-07ac-418e-a156-00ce3ea15862" />

- The `<animate>` element bypasses restrictions on static `href` attributes.  
- The victim sees “Click me” and clicks the link.  
- The payload executes, triggering the alert.  

## Verification  
- Injected the payload via the search parameter.  
- Reloaded the page with the crafted URL.  
- Clicked the “Click me” text.  
- Observed the alert box execution, confirming reflected XSS.  

<img width="1170" height="530" alt="image" src="https://github.com/user-attachments/assets/4a9ebb27-f906-407f-9a04-1b2b4792bcac" />

## Security Impact  
- **Bypass of Attribute Blocking:** Attackers can exploit allowed SVG features to reintroduce dangerous attributes.  
- **Arbitrary Code Execution:** Victims unknowingly execute attacker‑supplied JavaScript.  
- **High Severity:** Demonstrates that blocking event handlers and `href` attributes alone is insufficient.  

## Recommended Fixes  
- Disallow unsafe SVG elements and attributes (`animate`, `href`, etc.).  
- Use a strict HTML sanitization library that accounts for SVG quirks.  
- Apply Content Security Policy (CSP) to restrict script execution.  
- Avoid reflecting unsanitized user input into the DOM.  

## Lessons Learned  
This lab highlights how attackers can exploit overlooked features like SVG animation to bypass sanitization rules. Simply blocking obvious vectors (event handlers, `href`) is not enough — comprehensive sanitization and defense‑in‑depth are required to prevent XSS.

