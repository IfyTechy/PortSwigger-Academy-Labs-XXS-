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

   ## Business Impact
- **Account compromise** - Successful exploitation of this XSS vulnerability could allow attackers to execute malicious JavaScript in victims' browsers. This may lead to session hijacking, credential theft, unauthorized account access, phishing attacks, website defacement, and theft of sensitive information. The vulnerability may also result in reputational damage, financial loss, and regulatory compliance violations.
- **Data Theft** - sensitive information may be exposed reveling personal user data, payment information and internal business data. This can lead to privacy violations, regulatory penalties and loss of customer trust
- **Reputation Damage** - If users discover that a website is unsafe customer may stop using the platform, negative publicity may spread online and brand reputation may decline for businesses, where trust is critical
- **Financial Loss** - XXS attacks can cause fraudulent transactions, incident response costs, security remediation expenses, legal costs and customer compensation. large incidents may become expensive very quickly
- **Session Hijacking** - Attackers can impersonate legitimate users by stealing active session cookies. This may allow attackers to access private dashboards, perform actions as the victim, modify sensitive information
- **Malware Distribution** - An attacker may inject malicious scripts that Redirect users to phishing websites, Install malware and Deliver ransomware payloads. This affects both users and the organization’s credibility.

   ## Remediation
    

