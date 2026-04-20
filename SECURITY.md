# Security Policy

## Supported Versions

Only the latest published version (per semver) of each NicTool package receives security fixes.

| Package | Supported |
| ------- | --------- |
| Latest release | ✅ |
| Older releases | ❌ |

## Reporting a Vulnerability

**Please do not open a public GitHub issue for security vulnerabilities.**

Report vulnerabilities privately via GitHub's built-in security advisory mechanism:

👉 https://github.com/nictool/.github/security/advisories/new

You can also report against a specific repository:

- **DNS libraries**:
  - https://github.com/nictool/dns-resource-record/security/advisories/new
  - https://github.com/nictool/dns-nameserver/security/advisories/new
  - https://github.com/nictool/dns-zone/security/advisories/new
  - https://github.com/nictool/server/security/advisories/new
  - https://github.com/nictool/validate/security/advisories/new
- **Legacy 2.x**: https://github.com/nictool/nictool/security/advisories/new

### What to Include

- A clear description of the vulnerability and its potential impact
- Steps to reproduce or a proof-of-concept
- Affected package(s) and version(s)
- Any suggested mitigations, if known

### Response Timeline

1. **Acknowledgement** — We aim to acknowledge reports within **4 days**.
2. **Assessment** — We will confirm the issue, determine severity, and identify affected versions.
3. **Fix & Release** — A patch release will be prepared and coordinated with the reporter.
4. **Disclosure** — A GitHub Security Advisory (and CVE if applicable) will be published after the fix is available.

We follow [coordinated vulnerability disclosure](https://vuls.cert.org/confluence/display/CVD). Reporters are credited in the advisory unless they prefer otherwise.
