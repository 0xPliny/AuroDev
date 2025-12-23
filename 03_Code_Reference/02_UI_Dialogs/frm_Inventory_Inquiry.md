# frm_Inventory_Inquiry

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Inventory Inquiry dialog (`frm_Inventory_Inquiry`) displays inventory records and allows operators to query inventory by various criteria.

---

## Purpose

Provides UI for operators to inquire on inventory records, view load locations, and search for items.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\frm_Inventory_Inquiry.vb`
- **Executable:** `mhcMenu.exe`

---

## Key Functions

### LoadInventory
**Purpose:** Loads inventory records from database
**Action:** Queries MHC_INVENTORY table, displays in grid

### SearchInventory
**Purpose:** Searches inventory by item code, load ID, location
**Action:** Applies search filters, displays results

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_INVENTORY | SELECT | Query inventory records |
| MHC_LOAD | SELECT | Load information |
| MHC_LOCATION | SELECT | Location information |
| VIEW_LOAD_INVT | SELECT | Combined load/inventory view |

---

## User Actions

1. Search by item code
2. Search by load ID
3. Search by location
4. View inventory details
5. Drill down to related records

---

## Related Dialogs

- [frm_Location_Inquiry](frm_Location_Inquiry.md) - Location inquiry
- [frm_LoadProb_Inquiry](frm_LoadProb_Inquiry.md) - Load problem inquiry

---

## Related Documents

- [Dialog Index](00_Dialog_Index.md)
- [Inventory Table](../../04_Database_Reference/01_Core_Tables/MHC_INVENTORY.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Inventory Table | [MHC_INVENTORY](../../04_Database_Reference/01_Core_Tables/MHC_INVENTORY.md) | Schema |
| Location Table | [MHC_LOCATION](../../04_Database_Reference/01_Core_Tables/MHC_LOCATION.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

