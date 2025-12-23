# dsub (Database Subroutines)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

The `dsub` library provides database access functions and business logic for database operations.

---

## Purpose

Provides SQL interface and business logic functions for database operations, including move, load, inventory, and location management.

---

## Location

- **Headers:** `dsub\ds_*.h`
- **Source:** `dsub\ds_*.cpp`

---

## Key Functions

### ds_sql
- SQL execution interface
- Parameterized query support
- Transaction management

### ds_movs
- Move database functions
- CRUD operations for MHC_MOVS

### ds_load
- Load database functions
- CRUD operations for MHC_LOAD

### ds_invt
- Inventory database functions
- CRUD operations for MHC_INVENTORY

### ds_locn
- Location database functions
- CRUD operations for MHC_LOCATION

### ds_get_locn
- Get location logic
- Location selection algorithms
- Zone and size considerations

---

## Related Documents

- [Shared Libraries Index](00_Shared_Libraries_Index.md)
- [Database Access Patterns](../../02_Coding_Standards/03_Database_Access_Patterns.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Access | [Database Access Patterns](../../02_Coding_Standards/03_Database_Access_Patterns.md) | Patterns |
| Database Tables | [Database Overview](../../04_Database_Reference/00_Database_Overview.md) | Table List |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

