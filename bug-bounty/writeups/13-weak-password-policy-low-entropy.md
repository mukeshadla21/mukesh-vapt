# Finding #13 — Weak Password Policy Allows Low-Entropy Passwords

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dashten  
**Area:** Authentication / Account Registration  
**Vulnerability:** Weak Password Policy / Low-Entropy Password Acceptance  
**Severity:** Low / Informational

## What I Found

During testing of the user registration functionality, I found that the application accepted an extremely low-entropy password consisting of a single repeated lowercase character.

Although the password was very long, it contained effectively only one character pattern and therefore provided very little password complexity.

## What I Tested

1. Opened the user registration page.
2. Entered valid registration information.
3. Submitted a password consisting entirely of a repeated lowercase character.
4. The registration completed successfully.
5. No warning or password-strength validation was presented.

## Security Impact

A long password is not necessarily a strong password.

Accepting extremely predictable passwords may increase the risk of account compromise, particularly when users choose similarly weak passwords.

No account compromise was attempted or demonstrated.

## Recommended Remediation

- Implement password-strength estimation during registration.
- Reject or warn users about extremely low-entropy passwords.
- Check passwords against known compromised-password databases.
- Provide clear password-strength feedback to users.

### Responsible Disclosure

This entry documents the password-policy observation without publishing user credentials or private evidence.
