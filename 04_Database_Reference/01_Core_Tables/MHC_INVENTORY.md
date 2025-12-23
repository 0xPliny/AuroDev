# MHC_INVENTORY

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The `MHC_INVENTORY` table stores inventory records linking loads, items, and locations. It tracks what items are stored where.

---

## Purpose

Maintains inventory records showing which items (by item code) are stored in which loads at which locations.

---

## Schema

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| Inventory_ID | int | No | IDENTITY | Primary key, auto-increment |
| Load_ID | varchar(50) | No | | Foreign key to MHC_LOAD |
| Item_Code | varchar(50) | No | | Item/product code |
| Quantity | decimal(18,2) | No | | Quantity of item |
| Location | varchar(50) | No | | Foreign key to MHC_LOCATION |
| Status | varchar(20) | No | | Inventory status |
| Add_User | varchar(50) | No | | User who created record |
| Add_Date | datetime | No | GETDATE() | When record was created |

---

## Primary Key

- `Inventory_ID` (int, IDENTITY)

---

## Foreign Keys

| Column | References | On Delete |
|--------|------------|-----------|
| Load_ID | MHC_LOAD.Load_ID | CASCADE |
| Location | MHC_LOCATION.Location | RESTRICT |
| Status | MHC_INVENTORY_STATUS.Status | RESTRICT |

---

## Related Tables

- **MHC_LOAD** - Load containing inventory
- **MHC_LOCATION** - Location where inventory is stored
- **MHC_INVENTORY_LOG** - Inventory history log

---

## Related Documents

- [Database Overview](../00_Database_Overview.md)
- [MHC_LOAD](MHC_LOAD.md)
- [MHC_LOCATION](MHC_LOCATION.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Load Table | [MHC_LOAD](MHC_LOAD.md) | Schema |
| Location Table | [MHC_LOCATION](MHC_LOCATION.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

