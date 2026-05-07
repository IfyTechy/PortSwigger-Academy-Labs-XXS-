## Lab Title: Reflected XSS into HTML context with nothing encoded


- **Category:** Cross-Site Scripting
  
- **Level:** Practitioner
  
- **Platform:** PortSwigger Web Security Academy
  

  ## Objective

  The goal was to exploit a cross-site scripting vulnerability in the search functionality. 

  ## Discovery & Analysis
- **Vulnerable Parameter:** The functional search bar
- **The Problem:** The application does not sanitizee the user input allowing the injected payload to execute

  ## Exploitation 

  I used a specialized XXS payload injected into the funtional search bar in the web application
  
  <img width="1090" height="610" alt="image" src="https://github.com/user-attachments/assets/9f5c0188-0772-4f94-8eec-327540892a85" />

### Payload
`<script>alert(1)</script>`

  ## Verification
  - I pasted the XXS payload on the search bar
  - I clicked search
  - **Result:** I received a popup alert displaying 1 on the screen
<img width="658" height="372" alt="WhatsApp Image 2026-05-07 at 11 28 07" src="https://github.com/user-attachments/assets/28c5dafa-bd0a-4205-ae4e-38b535eb7622" />

    

