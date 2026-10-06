# Bug Bounty Methodology

## 1. Read the Program Policy

Before testing:

- Confirm the asset is in scope.
- Read exclusions.
- Understand rate limits.
- Review prohibited actions.
- Identify the accepted vulnerability classes.

## 2. Map the Attack Surface

Prioritize:

- Authentication
- Authorization
- APIs
- Administrative functionality
- File upload
- URL processing
- Business workflows
- Integrations
- Sensitive data flows

## 3. Discover and Validate

Use a combination of:

- Manual testing
- Proxy-based analysis
- Reconnaissance
- Controlled automation
- Source/JavaScript analysis

Do not submit scanner output without manual validation.

## 4. Demonstrate Impact

A strong report explains:

- What is vulnerable?
- What security control is bypassed?
- Who can exploit it?
- What data or functionality is exposed?
- What realistic impact exists?
- How should it be fixed?

## 5. Report Responsibly

Keep reports:

- Reproducible
- Concise
- Evidence-based
- Within program scope
- Free of unnecessary sensitive information
