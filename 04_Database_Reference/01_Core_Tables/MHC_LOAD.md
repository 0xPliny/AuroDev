# MHC_LOAD

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The `MHC_LOAD` table stores load master records. A load represents a physical unit (pallet, case, etc.) that can be stored and retrieved in the system.

---

## Purpose

Maintains master data for all loads in the system, including load identification, type, size, and status.

---

## Schema

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| Load_ID | varchar(50) | No | | Primary key, load identifier |
| Load_Type | varchar(20) | No | | Type of load (PALLET, CASE, etc.) |
| Load_Sub_Type | varchar(20) | Yes | NULL | Sub-type classification |
| Load_Size | varchar(20) | No | | Size classification |
| Load_Status | varchar(20) | No | | Current status |
| Add_User | varchar(50) | No | | User who created load |
| Add_Date | datetime | No | GETDATE() | When load was created |
| Modify_User | varchar(50) | Yes | NULL | User who last modified |
| Modify_Date | datetime | Yes | NULL | Last modification date |

---

## Primary Key

- `Load_ID` (varchar(50))

---

## Foreign Keys

| Column | References | On Delete |
|--------|------------|-----------|
| Load_Type | MHC_LOAD_TYPE.Load_Type | RESTRICT |
| Load_Sub_Type | MHC_LOAD_SUB_TYPE.Load_Sub_Type | RESTRICT |
| Load_Size | MHC_LOAD_SIZE.Load_Size | RESTRICT |
| Load_Status | MHC_LOAD_STATUS.Load_Status | RESTRICT |

---

## Related Tables

- **MHC_MOVS** - Moves involving this load
- **MHC_INVENTORY** - Inventory records for this load
- **MHC_PALLET** - Pallet containing this load
- **MHC_LOAD_LOG** - Load history log

---

## Related Documents

- [Database Overview](../00_Database_Overview.md)
- [MHC_MOVS](MHC_MOVS.md)
- [MHC_INVENTORY](MHC_INVENTORY.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Table | [MHC_MOVS](MHC_MOVS.md) | Schema |
| Inventory Table | [MHC_INVENTORY](MHC_INVENTORY.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

