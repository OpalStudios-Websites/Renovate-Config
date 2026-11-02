# Renovate Config - Central Agency Fleet Policy

Central configuration repository governing automated dependency updates, supply-chain security, and CVE patching across all client repositories within our GitHub Organisation.

---

## Architecture Overview

Instead of managing update policies per project, every client repository inherits its patching rules from this single repository via `renovate.json`
To link any new client repository to this central rulebook, add `renovate.json` to the root of the client project:

```json
{
  "$schema": "[https://docs.renovatebot.com/renovate-schema.json](https://docs.renovatebot.com/renovate-schema.json)",
  "extends": ["github>YOUR_ORG/renovate-config"]
}
```

---

## Fleet Update Policy

Every client repository inheriting this configuration (`"extends": ["github>YOUR_ORG/renovate-config"]`) operates under the following standardized update schedule:

| Update Type | Schedule | Minimum Release Age | Description |
| :--- | :--- | :--- | :--- |
| **Security CVEs** | Immediate (at any time) | **0 Days** | Bypasses quarantine. Opens an isolated security PR instantly and triggers headless E2E testing. |
| **Minor & Patch** | Mondays before 06:00 UK | **3 Days** | Bundled into a single weekly PR (`all non-major dependencies`) after surviving a 3-day quarantine against poisoned packages. |
| **Major Releases** | Mondays before 06:00 UK | **3 Days** | Opened as individual PRs for manual evaluation before upgrading. |
| **Lockfile Maintenance** | 1st of month before 06:00 | **3 Days** | Automatically refreshes sub-dependencies to keep trees clean. |
