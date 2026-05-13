## Lab Title: DOM XSS in document.write sink using source location.search  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

## Objective  
The goal was to exploit a DOM-based XSS vulnerability where unsanitized data from `location.search` is written directly into the page using `document.write`.  

## Discovery & Analysis  
- **Vulnerable Source:** `location.search`  
- **Vulnerable Sink:** `document.write`  
- **The Problem:** The application directly writes user-controlled input into the DOM without sanitization, allowing arbitrary script execution.  

## Exploitation  
By manipulating the query string in the URL, a malicious payload was injected. Since the application used `document.write` with unsanitized input, the payload executed in the browser.  

### Payload 
