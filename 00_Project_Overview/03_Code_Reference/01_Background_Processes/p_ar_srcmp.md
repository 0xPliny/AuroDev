# p_ar_srcmp (Stacker Completer)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Stacker Completer (`p_ar_srcmp`) monitors stacker completion messages and updates move records, inventory, and location tables when moves complete.

---

## Purpose

Processes completion messages from stacker cranes, updates database records to reflect completed moves, and updates statistics.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\p_ar_srcmp\p_ar_srcmp.cpp`
- **Executable:** `D:\Auro\Exec\p_ar_srcmp.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| `cc_stk` | Shared Library | Stacker class for accessing completion status |
| `movs.h` | OSUB Header | Move record access |
| `invt.h` | OSUB Header | Inventory record access |
| `locn.h` | OSUB Header | Location record access |
| Mapped Memory | Shared Memory | Read completion messages |

---

## Key Functions

### ProcessCompletion
**Purpose:** Processes stacker completion message
**Action:** Updates MHC_MOVS, MHC_INVENTORY, MHC_LOCATION, statistics

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | UPDATE | Update status to COMPLETE |
| MHC_INVENTORY | UPDATE | Update location |
| MHC_LOCATION | UPDATE | Update load_ID and status |
| MHC_STATISTICS | UPDATE | Increment counters |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Stacker State | R | Read completion status |
| Message Queue | R | Read completion messages |

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md)
- [Outbound Move Workflow](../../05_Workflows/02_Outbound_Move.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](p_ar_stkdp.md) | Dispatcher |
| Stacker Communications | [p_cc_stkcmx](p_cc_stkcmx.md) | Communication |
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

