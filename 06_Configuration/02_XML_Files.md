# XML Files

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the XML configuration files used in the Auro System.

---

## Key XML Files

### database_tables.xml
- **Purpose:** Database schema definitions
- **Location:** `D:\Auro\File\database_tables.xml`
- **Usage:** Used by `p_ut_makdb.exe` to generate database access routines
- **Structure:** Defines tables, views, synonyms, log tables

### menu.cnf
- **Purpose:** UI menu and security configuration
- **Location:** `D:\Auro\File\menu.cnf`
- **Usage:** Used by `p_sy_dbini.exe` to create security tables
- **Structure:** Defines menus, dialogs, actions, permissions

### Stacker.xml
- **Purpose:** Stacker crane definitions
- **Location:** `D:\Auro\File\Stacker.xml`
- **Usage:** Defines 22 stacker cranes (SR01-SR22)

### vehicle.xml
- **Purpose:** AGV/RTNX vehicle definitions
- **Location:** `D:\Auro\File\vehicle.xml`
- **Usage:** Defines vehicles and movement zones

### points.xml
- **Purpose:** Pathing point definitions
- **Location:** `D:\Auro\File\points.xml`
- **Usage:** Defines pathing points for vehicles

### KEPWAREplcdef_A.xml
- **Purpose:** PLC tag mappings
- **Location:** `D:\Auro\File\KEPWAREplcdef_A.xml`
- **Usage:** Maps PLC tags to MEM points

---

## Related Documents

- [Configuration Overview](00_Configuration_Overview.md)
- [Database Overview](../04_Database_Reference/00_Database_Overview.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Schema | [Database Overview](../04_Database_Reference/00_Database_Overview.md) | Schema |
| Menu Configuration | [Security](../08_Project_Specific/02_Site_Configuration.md) | Menu Setup |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

