# C++ Standards

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes C++ coding standards specific to the Auro System. For generic C++ standards, see [MHC-Developers-Guide C++ Standards](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards).

---

## Auro-Specific C++ Patterns

### Process Naming
- **Area Programs:** `p_ar_*` (dispatchers, completers)
- **Communication:** `p_cc_*` (equipment communication)
- **System:** `p_sy_*` (system services)
- **Site Integration:** `p_si_*` (Auro-specific)
- **Tracking:** `p_tk_*` (tracking)
- **Utilities:** `p_ut_*` (utilities)

### Equipment Class Usage
- **Stacker:** `cc_stk` class
- **Station:** `cc_std` class
- **PLC:** `cc_plc` class
- **AGV:** `cc_agv` class
- **RTNX:** `cc_rtnx` class

---

## Related Documents

- [Standards Overview](00_Standards_Overview.md)
- [Database Access Patterns](03_Database_Access_Patterns.md)
- [MHC-Developers-Guide C++](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Background Processes | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process Documentation |
| Shared Libraries | [Code Reference](../03_Code_Reference/03_Shared_Libraries/) | Library Documentation |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

