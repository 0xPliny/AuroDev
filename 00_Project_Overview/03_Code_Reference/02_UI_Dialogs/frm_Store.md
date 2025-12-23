# frm_Store

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Store dialog (`frm_Store`) allows operators to manually create store operations for loads.

---

## Purpose

Provides UI for operators to enter load information and create store move records in the system.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\frm_Store.vb`
- **Executable:** `mhcMenu.exe`

---

## Key Functions

### btnStore_Click
**Purpose:** Creates store move record
**Action:** Validates input, creates MHC_MOVS record with Status=QUEUED

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | INSERT | Create move record |
| MHC_LOAD | SELECT, INSERT | Verify/create load record |
| MHC_LOCATION | SELECT | Validate location |

---

## User Actions

1. Enter Load ID
2. Select or enter destination location
3. Click Store button
4. System creates move record

---

## Related Dialogs

- [frm_Manual_Retrieve](frm_Manual_Retrieve.md) - Manual retrieve
- [frm_MoveInq](frm_MoveInq.md) - Move inquiry
- [frm_Location_Inquiry](frm_Location_Inquiry.md) - Location inquiry

---

## Related Documents

- [Dialog Index](00_Dialog_Index.md)
- [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Inbound Move | [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md) | Workflow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

