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

Exploitation was achieved by injecting a malicious XSS payload into the search functionality, which failed to sanitize user input, allowing arbitrary script execution in the browser.
  
  Fig 1. <img width="1090" height="610" alt="image" src="https://github.com/user-attachments/assets/9f5c0188-0772-4f94-8eec-327540892a85" />

### Payload
`<script>alert(1)</script>`

  ## Verification
  - Inserted the XSS payload into the search input field
  - Submitted the request via the search function
  - Result: The application executed unsanitized user input in the HTML context, confirming the presence of a reflected Cross-Site Scripting (XSS) vulnerability.
    
Fig 2. <img width="658" height="372" alt="WhatsApp Image 2026-05-07 at 11 28 07" src="https://github.com/user-attachments/assets/28c5dafa-bd0a-4205-ae4e-38b535eb7622" />

Fig 3. <img width="941" height="393" alt="image" src="https://github.com/user-attachments/assets/95db3796-f199-402f-910f-988fe0e5b7da" />


   ## Business Impact
- **Account compromise** - Successful exploitation of this XSS vulnerability could allow attackers to execute malicious JavaScript in victims' browsers. This may lead to session hijacking, credential theft, unauthorized account access, phishing attacks, website defacement, and theft of sensitive information. The vulnerability may also result in reputational damage, financial loss, and regulatory compliance violations.
- **Data Theft** - sensitive information may be exposed reveling personal user data, payment information and internal business data. This can lead to privacy violations, regulatory penalties and loss of customer trust
- **Reputation Damage** - If users discover that a website is unsafe customer may stop using the platform, negative publicity may spread online and brand reputation may decline for businesses, where trust is critical
- **Financial Loss** - XXS attacks can cause fraudulent transactions, incident response costs, security remediation expenses, legal costs and customer compensation. large incidents may become expensive very quickly
- **Session Hijacking** - Attackers can impersonate legitimate users by stealing active session cookies. This may allow attackers to access private dashboards, perform actions as the victim, modify sensitive information
- **Malware Distribution** - An attacker may inject malicious scripts that Redirect users to phishing websites, Install malware and Deliver ransomware payloads. This affects both users and the organization’s credibility.
- **Defacement of Web Applications** - Attackers can modify webpage content to display fake messages, spread propaganda, damage company image. This is especially damaging for public-facing websites.
- **Compliance and Legal Issues** - Organizations handling sensitive data may violate regulations such as PCI DSS, GDPR & HIPAA. This can result in fines, audits & legal investigations.

   ## Remediation
To mitigate Cross-Site Scripting (XSS) vulnerabilities, the application should implement proper input validation and output encoding mechanisms.

Recommended remediation measures include:

- Validate and sanitize all user-supplied input before processing or storing it.
- Apply context-aware output encoding for HTML, JavaScript, URLs, and attributes.
- Implement Content Security Policy (CSP) headers to restrict the execution of malicious scripts.
- Avoid directly rendering untrusted user input in web pages.
- Use secure frameworks and templating engines that automatically escape output.
- Mark session cookies as HttpOnly and Secure to reduce the risk of session theft.
- Regularly perform security testing and code reviews to identify XSS vulnerabilities.
- Keep web frameworks, libraries, and dependencies updated with security patches.
    

