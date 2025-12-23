# p_ar_fndwk (Find Work)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Find Work process (`p_ar_fndwk`) scans the database for queued moves and updates them to pending status, waking up dispatchers to process the work.

---

## Purpose

Finds QUEUED moves in the database, validates they can be processed, updates them to PENDING status, and wakes up appropriate dispatchers.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\p_ar_fndwk\p_ar_fndwk.cpp`
- **Executable:** `D:\Auro\Exec\p_ar_fndwk.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| `movs.h` | OSUB Header | Move record access |
| `load.h` | OSUB Header | Load record access |
| `locn.h` | OSUB Header | Location record access |
| Database | Database | Query MHC_MOVS table |

---

## Key Functions

### FindQueuedMoves
**Purpose:** Scans MHC_MOVS for QUEUED moves
**Action:** Updates status to PENDING, sets scheduled_date
**Wake:** Wakes up dispatchers (p_ar_stkdp, p_ar_movdp, etc.)

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | SELECT, UPDATE | Find QUEUED moves, update to PENDING |
| MHC_LOAD | SELECT | Validate load exists |
| MHC_LOCATION | SELECT | Validate location exists |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Process Table | R/W | Wake dispatcher processes |

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md)
- [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Dispatcher | [p_ar_stkdp](p_ar_stkdp.md) | Dispatcher |
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Inbound Move | [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md) | Workflow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

