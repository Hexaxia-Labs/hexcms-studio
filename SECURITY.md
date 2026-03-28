# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 0.1.x   | Yes       |

## Reporting a Vulnerability

If you discover a security vulnerability, please report it responsibly.

**Do not open a public issue.**

Instead, email **security@hexaxia.com** with:

- A description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

We aim to acknowledge reports within 48 hours and provide a fix or mitigation plan within 7 days.

## Scope

HexCMS Studio runs locally on your machine. Security concerns primarily involve:

- Local filesystem access (read/write to configured repositories)
- Git credential handling (delegated to system Git)
- Markdown rendering (XSS prevention via DOMPurify)

## Acknowledgments

We appreciate responsible disclosure and will credit reporters in release notes (unless you prefer anonymity).
