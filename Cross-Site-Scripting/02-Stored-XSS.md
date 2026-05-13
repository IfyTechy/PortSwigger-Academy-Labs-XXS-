## Lab Title: Stored XSS in comment section  

- **Category:** Cross-Site Scripting  
- **Level:** Practitioner  
- **Platform:** PortSwigger Web Security Academy  

**Objective**  
The goal was to exploit a stored XSS vulnerability in the comment functionality.  

**Discovery & Analysis**  
- **Vulnerable Parameter:** The comment input field  
- **The Problem:** User input is stored and rendered without sanitization, allowing persistent payload execution
