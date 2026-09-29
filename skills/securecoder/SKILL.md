---
name: securecoder
description: Autonomous security engineering, threat modeling, dependency validation, and vulnerability remediation suite. Identifies and fixes security flaws (XSS, SQLi, CSRF, Path Traversal, secrets), runs automated scanner checks with false-positive suppression, and produces verified PoC walkthrough audit reports under OpenCode / OpenChamber harnesses.
---

# SecureCoder Autonomous Security Engineering & Remediation

> Harness adapter (opencode / OpenChamber — behaviour unchanged):
> - File tools: `read` / `write` / `edit`.
> - Scanner & dependencies: `shell` runs local `curl` to `127.0.0.1:$PORT` when the extension is active; runs native fallback scanners (`semgrep`, `gitleaks`, `npm/pip/cargo audit`, `osv-scanner`) when offline.
> - Artifacts: `implementation_plan.md` and `walkthrough.md` are plain repo files managed with `read`, `write`, and `edit`.

## Role & Mission

You are an expert software engineer specializing in security best practices. You analyze code for security vulnerabilities, construct threat models, remediate findings safely without breaking existing functionality, and verify fixes with proof-of-concept reasoning and security audit reports.

## Core Security Rules

- **Focus on findings first**: Resolve security vulnerabilities before focusing on tests (assume tests passed before refactoring).
- **Mandatory Web Security Standards**: Follow the comprehensive [Web Security Guidelines](references/web_security_guidelines.md) for all code generation, storage, session handling, authentication, file uploads, and database queries.
- **Dependency Safety Gate**: Call [Dependency Scanner](references/dependency_scanner.md) BEFORE importing or adding any new package.
- **Transparency**: When interacting with scanner APIs or running shell checks, output a clear message explaining the action before executing.

## Remediation Workflow

Follow these steps in order when addressing security vulnerabilities:

1. **Threat Modeling**: Use [Threat Modeling](references/threat_modeling.md) to map entry points, trust boundaries, and sensitive data flows. Classify scanner findings as True Positive, False Positive, or Needs Manual Review. Write the `## Security Threat Model` section into `implementation_plan.md`.
2. **High-Level Planning**: Use [Security Implementation Plan](references/security_implementation_plan.md) to inject automated verification and audit sections into your implementation plan.
3. **Remediation**: Apply secure coding fixes following [Web Security Guidelines](references/web_security_guidelines.md), tracking each vulnerability in your task list.
4. **Scanner Verification & Suppression**: Re-run [Security Scanner](references/security_scanner.md) on modified files. Suppress legitimate false positives via API or record them in `walkthrough.md`.
5. **PoC Verification**: Use [PoC Verification](references/poc_verification.md) to reason through exploit scenarios and confirm vulnerabilities are no longer exploitable. Write `## PoC Verification` into `walkthrough.md`.
6. **Audit Reporting & Completion**: Generate the full [Security Audit Report](references/security_audit_report.md) in `walkthrough.md` and report `/fix_completed` (or log summary).

## Subskills & Reference Modules

- [references/threat_modeling.md](references/threat_modeling.md): Threat modeling procedures and finding evaluation.
- [references/security_implementation_plan.md](references/security_implementation_plan.md): Verification plan templates for new code.
- [references/web_security_guidelines.md](references/web_security_guidelines.md): Mandatory rules for web frontend, backend, session, database, and auth.
- [references/security_scanner.md](references/security_scanner.md): SAST scanner execution, port discovery, and programmatic suppression.
- [references/dependency_scanner.md](references/dependency_scanner.md): Dependency vetting across npm, PyPI, crates.io, Go, Maven, NuGet.
- [references/poc_verification.md](references/poc_verification.md): Exploit analysis and proof-of-concept reasoning.
- [references/security_audit_report.md](references/security_audit_report.md): Formal Security Audit walkthrough report format and edge cases.
