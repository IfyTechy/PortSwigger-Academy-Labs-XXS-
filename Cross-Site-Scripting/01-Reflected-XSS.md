## Lab Title: Reflected XSS into HTML context with nothing encoded


- **Category:** Cross-Site Scripting
  
- **Level:** Practitioner
  
- **Platform:** PortSwigger Web Security Academy
  

  ## Objective

  The goal was to exploit a cross-site scripting vulnerability in the search functionality. 

  ## Discovery & Analysis
- Vulnerable Parameter: The functionality search bar
- The Problem: The application does not sanitizee the user input allowing the injected payload to execute

  
