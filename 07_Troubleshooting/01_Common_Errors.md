# Common Errors

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

This document lists common error codes and their solutions in the Auro System.

---

## Error Categories

### Database Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| DB_CONN_FAIL | Database connection failure | Check SQL Server is running, verify connection string |
| DB_DEADLOCK | Transaction deadlock | Retry operation, check for long-running transactions |
| DB_LOCK_TIMEOUT | Table lock timeout | Check for blocking queries, increase timeout |

### Communication Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| COMM_TIMEOUT | Communication timeout | Check network connectivity, verify equipment is online |
| COMM_OFFLINE | Equipment offline | Check equipment power, verify communication settings |
| COMM_PROTOCOL_ERROR | Protocol error | Verify protocol settings, check message format |

### Process Errors

| Error Code | Description | Solution |
|------------|-------------|----------|
| PROC_CRASH | Process crashed | Check logs, restart process, verify dependencies |
| PROC_NOT_START | Process won't start | Check logs, verify prerequisites, check permissions |
| PROC_HANG | Process hanging | Check for deadlocks, restart process |

---

## Related Documents

- [Troubleshooting Index](00_Troubleshooting_Index.md)
- [Decision Trees](02_Decision_Trees.md)
- [Recovery Procedures](03_Recovery_Procedures.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Error Handling | [Error Handling Patterns](../02_Coding_Standards/04_Error_Handling_Patterns.md) | Patterns |
| Error Table | [MHC_ERROR](../04_Database_Reference/01_Core_Tables/MHC_ERROR.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

