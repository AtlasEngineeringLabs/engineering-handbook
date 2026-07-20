# Engineering Workstation Guide

**Document ID:** AEL-HB-001

**Version:** 0.1.0

**Status:** Draft

**Owner:** Leon Pitkin

**Reviewed By:** Pending

**Last Updated:** 2026-07-20

---

# Purpose

This document defines the standard engineering workstation used throughout Atlas Engineering Labs.

The workstation provides a reproducible, secure and professional development environment for cloud infrastructure engineering, platform engineering and software development.

---

# Hardware Specification

| Component | Specification |
|----------|---------------|
| Device | MacBook Pro M2 |
| Memory | 32 GB |
| Storage | 1 TB SSD |
| Free Space | ~600 GB |
| Operating System | macOS 15.7.3 |

---

# Directory Structure

```text
~/AtlasEngineeringLabs/
│
├── handbook/
├── repositories/
├── labs/
├── architecture/
├── diagrams/
├── notes/
├── scripts/
├── downloads/
├── tools/
└── archive/
```

---

# Core Engineering Tools

| Tool | Purpose | Status |
|------|---------|--------|
| Homebrew | Package Manager | ✅ |
| Git | Source Control | ✅ |
| GitHub CLI | GitHub Integration | ✅ |
| Cursor | Primary IDE | ✅ |
| Docker Desktop | Container Platform | ✅ |
| SSH | GitHub Authentication | ✅ |

---

# Engineering Principles

- Infrastructure should be reproducible.
- Automation should replace manual configuration wherever practical.
- All engineering work should be version controlled.
- Security should be considered from the beginning.
- Documentation is treated as part of the deliverable.
- Keep solutions as simple as possible while meeting the requirements.

---

# Future Toolchain

The following tools will be installed during Sprint 2:

- Terraform
- AWS CLI
- Google Cloud CLI
- Python
- Go
- kubectl
- kind
- Helm
- k9s

---

# Verification

## Git

```bash
git --version
```

## Cursor

```bash
cursor --version
```

## Docker

```bash
docker --version
```

## GitHub CLI

```bash
gh --version
```
---
# Revision History

| Version | Date | Author | Description |
|---------|------|--------|-------------|
| 0.1.0 | 2026-07-20 | Leon Pitkin | Initial document created |


# References

- Atlas Engineering Standards
- Technology Standards
- Programme Charter
