# Decision Trees

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

This document provides decision trees for diagnosing common problems in the Auro System.

---

## Move Stuck Diagnostic

```
Is move status QUEUED?
  Yes → Check p_ar_fndwk is running
  No → Continue

Is move status PENDING?
  Yes → Check dispatcher is running
  No → Continue

Is move status SCHEDULED?
  Yes → Check communication process is running
  No → Continue

Is move status BUSY?
  Yes → Check equipment is responding
  No → Check completer is running
```

---

## Equipment Offline Diagnostic

```
Is equipment showing offline?
  Yes → Check communication process is running
        → Check network connectivity
        → Check equipment power
        → Verify communication settings
  No → Equipment is online
```

---

## Related Documents

- [Troubleshooting Index](00_Troubleshooting_Index.md)
- [Common Errors](01_Common_Errors.md)
- [Recovery Procedures](03_Recovery_Procedures.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Common Errors | [01_Common_Errors.md](01_Common_Errors.md) | Error List |
| Recovery | [03_Recovery_Procedures.md](03_Recovery_Procedures.md) | Recovery Steps |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

