# Elements Table

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

This document describes the `MHC_ELEMENTS` table used for system configuration parameters.

---

## Purpose

The `MHC_ELEMENTS` table stores system-wide configuration parameters that can be modified without code changes.

---

## Schema

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| Element_ID | int | No | IDENTITY | Primary key |
| Element_Name | varchar(100) | No | | Element name |
| Element_Value | varchar(500) | Yes | NULL | Element value |
| Element_Type | varchar(20) | Yes | NULL | Element type |
| Description | varchar(500) | Yes | NULL | Description |

---

## Common Elements

- Database connection strings
- Communication timeouts
- Retry counts
- System flags
- Path configurations

---

## Related Documents

- [Configuration Overview](00_Configuration_Overview.md)
- [Database Overview](../04_Database_Reference/00_Database_Overview.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Tables | [Database Overview](../04_Database_Reference/00_Database_Overview.md) | Table List |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

