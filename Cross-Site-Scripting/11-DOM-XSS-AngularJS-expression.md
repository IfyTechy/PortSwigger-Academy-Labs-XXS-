## Lab Title: DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
Exploit a DOM-based XSS vulnerability where user input is evaluated inside an AngularJS expression. Although angle brackets and double quotes are HTML‑encoded, other characters remain unsanitized, allowing injection.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search` or user-controlled input  
- **Vulnerable Sink:** AngularJS expression (`{{ }}` binding)  
- **The Problem:** Encoding of angle brackets and quotes is insufficient; attackers can inject payloads using other characters or bypass filters.  

## Exploitation  
By injecting a crafted payload into the vulnerable parameter, arbitrary JavaScript can be executed through AngularJS expression evaluation.  

<img width="1157" height="579" alt="image" src="https://github.com/user-attachments/assets/dfb3d9df-c667-4e00-8eff-5727eaa586a9" />


### Payload Example  
`{{constructor.constructor('alert(1)')()}}`

## Verification  
- Inserted the payload into the vulnerable parameter  
- Reloaded the page  
- Observed the alert box execution, confirming DOM-based XSS  

<img width="1274" height="596" alt="image" src="https://github.com/user-attachments/assets/c1fa336c-e9df-4256-ae4f-ac5092b1e8eb" />

<img width="1260" height="575" alt="image" src="https://github.com/user-attachments/assets/4821c475-6a7b-46f0-8077-afd08d7e8a7d" />

## Business Impact  
- Attackers can execute arbitrary JavaScript in the victim’s browser  
- Potential for session hijacking, credential theft, or malicious redirection  

## Remediation  
- Disable AngularJS expression evaluation for user input  
- Use AngularJS’s `$sce` (Strict Contextual Escaping) to enforce safe bindings  
- Sanitize and validate all user inputs before rendering  

## Recommendation  
- Avoid binding unsanitized user input directly into AngularJS expressions  
- Apply Content Security Policy (CSP) to mitigate script injection risks  
- Regularly test for DOM XSS vulnerabilities with automated tools and manual review  
