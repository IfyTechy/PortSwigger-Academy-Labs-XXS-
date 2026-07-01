## Lab Title: Reflected XSS into HTML context with all tags blocked except custom ones  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a reflected XSS vulnerability where user input is placed into an HTML context. All standard tags are blocked, but custom/non-standard tags are allowed. The goal is to inject a custom tag that automatically executes JavaScript to exfiltrate `document.cookie`.  

## Discovery & Analysis  
- **Vulnerable Source:** Query string parameter in the URL  
- **Vulnerable Context:** HTML body content  
- **The Problem:** The application blocks common tags and attributes, but custom tags slip through. By combining event handlers and focus mechanics, attackers can still trigger script execution.  

## Exploitation  
PortSwigger’s solution uses a custom tag (`<xss>`) with an `onfocus` event handler. To ensure the event fires automatically:  
- `id="x"` → gives the element a reference.  
- `tabindex=1` → makes the element focusable.  
- `#x` in the URL fragment → automatically focuses the element when the page loads.  
 
<img width="1015" height="472" alt="image" src="https://github.com/user-attachments/assets/a2c912a5-227b-4542-81a6-def0f3ad4a94" />

### Payload Example  
`<script>
location = 'https://0a38003d0311f2f680fa036c008000f1.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>

## Verification  
- Delivered the exploit via the exploit server.
- When the victim loads the page, the browser automatically focuses the custom tag.
- The onfocus handler executes, triggering alert(document.cookie).

<img width="1169" height="536" alt="image" src="https://github.com/user-attachments/assets/bc797934-feef-46f7-8cdc-c50ba95b7e28" />

## Business Impact  
- Attackers can bypass naive filtering by using custom tags  
- Persistent risk of session hijacking, credential theft, or malicious redirection.
  
## Remediation  
- Use proper output encoding for HTML context  
- Avoid blacklisting; instead, apply context‑aware escaping  
- Apply Content Security Policy (CSP) to mitigate script injection risks  

## Recommendation  
- Implement robust input validation and output encoding  
- Test filters against known bypass techniques (custom tags, unusual attributes)  
- Regularly audit applications for XSS vulnerabilities  
