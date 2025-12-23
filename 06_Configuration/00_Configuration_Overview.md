# Configuration Overview

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides an overview of all configuration files used in the Auro System, including their purposes and locations.

---

## Configuration File Categories

### XML Configuration Files
- `database_tables.xml` - Database schema definitions
- `menu.cnf` - UI menu and security configuration
- `Stacker.xml` - Stacker crane definitions
- `vehicle.xml` - AGV/RTNX vehicle definitions
- `points.xml` - Pathing point definitions
- `KEPWAREplcdef_A.xml` - PLC tag mappings
- `zones.xml` - Zone definitions
- `stands.xml` - Stand definitions
- `areas.xml` - Area definitions

### CNF Configuration Files
- `dcbcnf_A.cnf` - Discrete Control Block configuration
- `Dcbdef_A.cnf` - DCB definitions
- `logdef.cnf` - Log definitions
- `timer.cnf` - Timer configuration
- `wrm_Auro_a.cnf` - Warmstart sequence
- `ttydef_Auro_A.cnf` - TTY definitions
- `watchdef.cnf` - Watchdog definitions

### Message Files
- `english.msg` - English message definitions
- `icis_english.msg` - ICIS English messages

---

## Configuration File Locations

- **Primary Location:** `D:\Auro\File\`
- **Backup Location:** `D:\ICIS\AuroDev\File\` (if exists)

---

## Related Documents

- [CNF Files](01_CNF_Files.md)
- [XML Files](02_XML_Files.md)
- [Registry Settings](03_Registry_Settings.md)
- [Elements Table](04_Elements_Table.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| CNF Files | [CNF Files](01_CNF_Files.md) | CNF Documentation |
| XML Files | [XML Files](02_XML_Files.md) | XML Documentation |
| Registry | [Registry Settings](03_Registry_Settings.md) | Registry Keys |
| Elements | [Elements Table](04_Elements_Table.md) | Elements Documentation |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

