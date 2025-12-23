# MHC_LOCATION

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The `MHC_LOCATION` table stores all storage locations in the system, including rack locations, stations, and temporary storage positions.

---

## Purpose

Maintains location master data and tracks which load (if any) is currently stored at each location.

---

## Schema

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| Location | varchar(50) | No | | Primary key, location identifier |
| Location_Type | varchar(20) | No | | RACK, STATION, TEMP, etc. |
| Location_Status | varchar(20) | No | | AVAILABLE, OCCUPIED, LOCKED, ERROR |
| Location_Size | varchar(20) | No | | Size classification |
| Load_ID | varchar(50) | Yes | NULL | Current load at location |
| Area | varchar(20) | Yes | NULL | Area containing location |
| Aisle | int | Yes | NULL | Aisle number (for rack locations) |
| Bay | int | Yes | NULL | Bay number |
| Level | int | Yes | NULL | Level/tier number |
| Position | int | Yes | NULL | Position number |
| Add_User | varchar(50) | No | | User who created location |
| Add_Date | datetime | No | GETDATE() | When location was created |

---

## Primary Key

- `Location` (varchar(50))

---

## Foreign Keys

| Column | References | On Delete |
|--------|------------|-----------|
| Load_ID | MHC_LOAD.Load_ID | SET NULL |
| Location_Type | MHC_LOCATION_TYPE.Location_Type | RESTRICT |
| Location_Status | MHC_LOCATION_STATUS.Location_Status | RESTRICT |
| Location_Size | MHC_LOCATION_SIZE.Location_Size | RESTRICT |
| Area | MHC_AREA.Area | RESTRICT |

---

## Related Tables

- **MHC_LOAD** - Load currently at location
- **MHC_INVENTORY** - Inventory records at location
- **MHC_MOVS** - Moves to/from location
- **MHC_LOCATION_LOG** - Location history log

---

## Related Documents

- [Database Overview](../00_Database_Overview.md)
- [MHC_MOVS](MHC_MOVS.md)
- [MHC_LOAD](MHC_LOAD.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Table | [MHC_MOVS](MHC_MOVS.md) | Schema |
| Load Table | [MHC_LOAD](MHC_LOAD.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

