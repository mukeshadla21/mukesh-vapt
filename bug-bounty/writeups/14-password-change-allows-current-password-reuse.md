# Finding #14 — Password Change Allows Reusing the Current Password

## 🐞 Bug Found — Responsible Disclosure

**Program:** Dashten  
**Area:** Authentication / Password Management  
**Vulnerability:** Password Reuse / Weak Password Change Validation  
**Severity:** Low / Informational

## What I Found

During testing of the password change functionality, I found that the application allowed a user to set their new password to exactly the same value as their current password.

The password change request was accepted and the application returned a successful password-update response even though the credential remained unchanged.

## How It Worked

1. Logged in to a valid user account.
2. Opened the account password-change functionality.
3. Entered the current password in the **Current Password** field.
4. Entered the exact same value in the **New Password** field.
5. Entered the same value in the confirmation field.
6. Submitted the request.
7. The application accepted the request and reported that the password was updated successfully.

The submitted password values and session information are intentionally not published.

## Evidence

The captured HTTP exchange demonstrated that the password-change request was accepted with the current password supplied as the new password, and the application returned a successful password-update response.

## Security Impact

Allowing the current password to be reused reduces the effectiveness of the password-change control.

This becomes more relevant when a password change is required after suspected credential exposure, security maintenance, or account recovery. A user can effectively complete a password-change workflow without actually changing the credential.

No account takeover or unauthorized access was demonstrated.

## Recommended Remediation

- Compare the proposed new password against the current password.
- Reject the change when both passwords are identical.
- Return a clear validation message such as: **"The new password must be different from your current password."**
- Consider enforcing password-history rules where appropriate for the application's security requirements.

### Responsible Disclosure

This finding is documented using sanitized evidence. Credentials, session tokens, and other sensitive request data are not published.
