# frm_MoveInq

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Move Inquiry dialog (`frm_MoveInq`) displays move records and allows operators to query and view move status.

---

## Purpose

Provides UI for operators to inquire on move records, view move status, and monitor move progress.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\frm_MoveInq.vb`
- **Executable:** `mhcMenu.exe`

---

## Key Functions

### LoadMoves
**Purpose:** Loads move records from database
**Action:** Queries MHC_MOVS table, displays in grid

### FilterMoves
**Purpose:** Filters displayed moves
**Action:** Applies filters based on status, type, etc.

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | SELECT | Query move records |
| MHC_LOAD | SELECT | Load information |
| MHC_LOCATION | SELECT | Location information |

---

## User Actions

1. Query moves by status, type, date range
2. View move details
3. Monitor move progress
4. Drill down to related records

---

## Related Dialogs

- [frm_Move_Maintenance](frm_Move_Maintenance.md) - Move maintenance
- [frm_Store](frm_Store.md) - Store operation
- [frm_Manual_Retrieve](frm_Manual_Retrieve.md) - Manual retrieve

---

## Related Documents

- [Dialog Index](00_Dialog_Index.md)
- [Move Table](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Move Maintenance | [frm_Move_Maintenance](frm_Move_Maintenance.md) | Maintenance Dialog |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

