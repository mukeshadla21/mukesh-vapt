# API Security Testing

## Testing Areas

### Authentication
- Token handling
- Session invalidation
- Password reset workflows
- MFA enforcement
- Authentication bypass

### Authorization
- BOLA / IDOR
- Horizontal privilege escalation
- Vertical privilege escalation
- Object-level authorization
- Function-level authorization

### Input Handling
- Parameter tampering
- Injection testing
- Type confusion
- Unexpected HTTP methods
- Mass assignment

### API Configuration
- Excessive data exposure
- Security headers
- CORS configuration
- Rate limiting
- Debug information
- OpenAPI / Swagger exposure

### Business Logic
- Workflow manipulation
- Replay attacks
- Price/quantity manipulation
- State transition bypass
- Missing server-side validation

## Validation Principle

A scanner finding is only a lead. Confirm the behavior manually and establish whether an attacker can cross a meaningful security boundary.

All examples in this repository should use authorized targets or intentionally vulnerable environments.
