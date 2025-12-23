# p_sy_dbini (Database Initialization)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Database Initialization process (`p_sy_dbini`) initializes the database and loads configuration data during system warmstart.

---

## Purpose

Connects to database, populates tables from configuration files, creates security tables from menu.cnf, and initializes mapped memory with database data.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\sysc\p_sy_dbini\p_sy_dbini.cpp`
- **Executable:** `D:\Auro\Exec\p_sy_dbini.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| Database | Database | SQL Server connection |
| menu.cnf | Configuration File | Security menu definitions |
| database_tables.xml | Configuration File | Database schema |

---

## Key Functions

### InitializeDatabase
**Purpose:** Connects to database and verifies schema
**Action:** Establishes connection, checks tables exist

### ProcessMenuConfig
**Purpose:** Processes menu.cnf to create security tables
**Action:** Creates MHC_SEC_* tables, generates SecurityKeyConstants

### LoadConfiguration
**Purpose:** Loads configuration data into database
**Action:** Populates lookup tables, initializes system data

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_SEC_* | CREATE, INSERT | Create security tables |
| MHC_ELEMENTS | INSERT | Load configuration elements |
| All Tables | SELECT | Verify schema |

---

## Configuration Files Used

- `menu.cnf` - Menu and security definitions
- `database_tables.xml` - Database schema
- `english.msg` - Message definitions

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md)
- [Configuration Overview](../../06_Configuration/00_Configuration_Overview.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Startup Sequence | [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md) | Initialization |
| Menu Config | [XML Files](../../06_Configuration/02_XML_Files.md) | menu.cnf |
| Database Schema | [Database Overview](../../04_Database_Reference/00_Database_Overview.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

