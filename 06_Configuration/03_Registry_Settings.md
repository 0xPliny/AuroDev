# Registry Settings

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes Windows Registry settings used in the Auro System.

---

## Key Registry Settings

### ICIS_BYPASS
- **Path:** `HKEY_CURRENT_USER\Environment\ICIS_BYPASS`
- **Type:** String Value
- **Purpose:** Enables developer bypass mode
- **Value:** Any non-empty string (e.g., "1")
- **Effect:** Bypasses login security in dialogs

### Environment Variables
- **Path:** `HKEY_CURRENT_USER\Environment\`
- **Variables:**
  - `MEM_HOME` - Root directory of MEM installation
  - `ICIS_BYPASS` - Developer bypass flag
  - Additional project-specific variables

---

## Related Documents

- [Configuration Overview](00_Configuration_Overview.md)
- [Quick Start Guide](../00_Quick_Start_Guide.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Setup | [Quick Start Guide](../00_Quick_Start_Guide.md) | Registry Setup |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

