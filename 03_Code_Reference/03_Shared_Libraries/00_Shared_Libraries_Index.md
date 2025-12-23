# Shared Libraries Index

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

This document provides an index of shared libraries used in the Auro System.

---

## Core Libraries (csub/)

### Equipment Classes
- [cc_stk](cc_stk.md) - Stacker Classes
- [cc_std](cc_std.md) - Station Classes
- [cc_plc](cc_plc.md) - PLC Classes
- [cc_agv](cc_agv.md) - AGV Classes
- [cc_rtnx](cc_rtnx.md) - RTNX Classes
- [cc_zone](cc_zone.md) - Zone Classes

### Utility Classes
- [cc_gg](cc_gg.md) - Global Definitions
- [cc_route](cc_route.md) - Routing Classes
- [cc_group](cc_group.md) - Group Classes

---

## Database Libraries (dsub/)

- [dsub](dsub.md) - Database Subroutines
  - ds_sql - SQL interface
  - ds_movs - Move database functions
  - ds_load - Load database functions
  - ds_invt - Inventory database functions
  - ds_locn - Location database functions
  - ds_get_locn - Get location logic

---

## Operating System Libraries (osub/)

- [osub](osub.md) - Operating System Subroutines
  - Generated OSUB routines (z_*.cpp, z_*.vb)
  - Database access wrappers

---

## Interoperability Libraries

- [CInterop](CInterop.md) - C++/VB.NET Interop
  - Marshalling between managed/unmanaged code
  - VB.NET to C++ bridge

- [MHC_Interop](MHC_Interop.md) - MHC Interop
  - Managed code interop layer

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Component Dependency Map](../../01_System_Architecture/01_Component_Dependency_Map.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Library Details | Individual library files | Library Documentation |
| Dependency Map | [Component Dependency Map](../../01_System_Architecture/01_Component_Dependency_Map.md) | Dependencies |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

