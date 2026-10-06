# Reconnaissance Methodology

## Objective

Build an accurate attack-surface inventory before vulnerability testing.

## Workflow

```text
Passive Recon
     ↓
Subdomain Enumeration
     ↓
DNS Resolution
     ↓
HTTP Probing
     ↓
Technology Fingerprinting
     ↓
Port / Service Discovery
     ↓
JavaScript Discovery
     ↓
Endpoint Discovery
     ↓
Parameter Discovery
     ↓
Manual Validation
```

## Typical Data Collected

- Domains and subdomains
- Resolved IP addresses
- HTTP status codes
- Titles
- Technologies
- Open ports
- API endpoints
- JavaScript files
- Interesting parameters
- Authentication surfaces

## Quality Control

Recon results should be:

- Deduplicated
- Validated
- Scoped
- Timestamped
- Stored separately from sensitive evidence

Never perform reconnaissance against assets outside the authorized scope.
