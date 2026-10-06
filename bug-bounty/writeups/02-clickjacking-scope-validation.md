# Security Research Lesson #2 — Clickjacking & Scope Validation

## Topic

**Clickjacking / UI Redressing — Scope and Disclosure Validation**

## Research Context

During security research against a managed bug bounty program, I identified a potential clickjacking issue involving a security-sensitive web workflow.

The issue was submitted through the program's official disclosure channel.

The submission was subsequently classified as **Out of Scope** under the program's published scope rules.

## Key Technical Learning

When assessing clickjacking, it is not enough to demonstrate that a page can be loaded inside an iframe.

A useful assessment should determine:

- Whether anti-clickjacking controls are missing or ineffective.
- Whether the framed page contains a meaningful user action.
- Whether an attacker can realistically influence a victim's interaction.
- Whether the affected functionality has security or business impact.
- Whether the finding satisfies the program's vulnerability taxonomy and scope.

### Relevant Controls

Common anti-clickjacking protections include:

- `Content-Security-Policy: frame-ancestors`
- `X-Frame-Options`

## Key Bug Bounty Lesson

**Always validate program scope before investing significant effort in a finding.**

Before testing, review:

1. In-scope assets.
2. Out-of-scope vulnerability classes.
3. Special exclusions.
4. Vulnerability Rating Taxonomy (VRT) guidance.
5. Disclosure restrictions.
6. Whether public disclosure is permitted.

In this case, the program's scope rules excluded the reported clickjacking scenario, resulting in an **Out of Scope** classification.

## What I Learned

### 1. Framing alone is not sufficient

A missing frame-protection header is a useful security signal, but the actual risk depends heavily on the functionality exposed through the framed interface.

### 2. Demonstrate realistic impact

A stronger clickjacking assessment should establish whether a victim can realistically be induced to perform a meaningful action.

### 3. Understand the bounty taxonomy

The same technical weakness can receive different treatment depending on the affected functionality and the program's published VRT/scope rules.

### 4. Respect disclosure restrictions

A private bug bounty submission should never be converted into a public technical write-up when the program prohibits disclosure.

## Responsible Disclosure

No target name, URL, submission identifier, screenshot, credentials, or proof-of-concept from the confidential submission is published here.

This page intentionally documents only the **general security lesson and methodology**.

---

### Research Principle

> **Find the vulnerability. Validate the impact. Check the scope. Respect the disclosure policy.**
