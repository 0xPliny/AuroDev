# Standards Overview

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides an overview of coding standards used in the Auro System. The Auro System follows the MEM/MHC coding standards with some Auro-specific additions.

---

## Relationship to MHC-Developers-Guide

**CRITICAL:** This documentation references the MHC-Developers-Guide for generic patterns and standards.

The Auro System follows the coding standards defined in:
- [MHC-Developers-Guide Coding Standards](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards)

This document only covers **Auro-specific additions and variations**.

---

## Language Standards

### C++ Standards
- **Standard:** C++11/C++14 (Visual Studio 2017)
- **Naming:** See [C++ Standards](02_CPP_Standards.md)
- **File Organization:** See [C++ Standards](02_CPP_Standards.md)

### VB.NET Standards
- **Framework:** .NET Framework 4.7.2
- **Naming:** See [VB.NET Standards](01_VB_NET_Standards.md)
- **File Organization:** See [VB.NET Standards](01_VB_NET_Standards.md)

---

## Database Access Standards

### OSUB API Usage
- **Generated Routines:** Use `z_*.vb` or `z_*.cpp` files generated from `database_tables.xml`
- **Pattern:** See [Database Access Patterns](03_Database_Access_Patterns.md)

### Direct SQL Usage
- **When:** Complex queries not covered by OSUB
- **Pattern:** Use parameterized queries, handle errors

---

## Error Handling Standards

### Error Detection
- **Processes:** Monitor error flags in mapped memory
- **UI:** Display errors via error dialogs
- **Logging:** All errors logged to `MHC_ERROR` table

### Error Recovery
- **Pattern:** See [Error Handling Patterns](04_Error_Handling_Patterns.md)
- **Recovery Procedures:** Documented in workflows

---

## Auro-Specific Standards

### Site Integration Processes
- **Naming:** `p_si_*` prefix for site-specific processes
- **Examples:** `p_si_AuroHost`, `p_si_AuroTrack`
- **Location:** `MSVC Programs\simu\p_si_*`

### Host Communication
- **Process:** `p_si_AuroHost` handles Auro-specific host protocol
- **Tables:** `MHC_HOST_IN`, `MHC_HOST_OUT`
- **Pattern:** Follows generic host communication pattern with Auro-specific message formats

---

## Code Organization

### Directory Structure
```
MSVC Programs\
  area\          - Area programs (dispatchers, completers)
  sysc\          - System control programs
  simu\          - Site integration programs (Auro-specific)
  trak\          - Tracking programs
  util\          - Utility programs
  csub\          - Core subroutines (if exists)
  dsub\          - Database subroutines
  osub\          - Operating system subroutines

MSVB Programs\
  dilg\mhcMenu\  - UI dialogs
  osub\          - Database access routines (generated)
  util\          - Utility programs
```

---

## Build Standards

### Build Order
1. Utilities (`p_ut_regst`, `makemsg`)
2. Core libraries (csub, dsub, osub, cryptlib)
3. Background processes
4. VB.NET projects (MHC_Interop → GenScript → mhcMenu)

### Build Configuration
- **Debug:** Development and testing
- **Release:** Production deployment
- **Platform:** x86 (32-bit) or x64 (64-bit)

---

## Testing Standards

### Unit Testing
- **C++:** Use test projects in `tsub\` directory
- **VB.NET:** Test dialogs in development environment

### Integration Testing
- **Process:** Test processes with simulated equipment
- **Configuration:** Use `dcbdef_sim_A.cnf` for simulation

---

## Documentation Standards

### Code Comments
- **Header Comments:** File purpose, author, date
- **Function Comments:** Purpose, parameters, return value
- **Inline Comments:** Complex logic explanations

### Documentation Files
- **Process Documentation:** See [Code Reference](../03_Code_Reference/01_Background_Processes/)
- **Dialog Documentation:** See [Code Reference](../03_Code_Reference/02_UI_Dialogs/)
- **Database Documentation:** See [Database Reference](../04_Database_Reference/)

---

## Related Documents

- [VB.NET Standards](01_VB_NET_Standards.md)
- [C++ Standards](02_CPP_Standards.md)
- [Database Access Patterns](03_Database_Access_Patterns.md)
- [Error Handling Patterns](04_Error_Handling_Patterns.md)
- [MHC-Developers-Guide Standards](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| VB.NET Details | [VB.NET Standards](01_VB_NET_Standards.md) | Naming, Patterns |
| C++ Details | [C++ Standards](02_CPP_Standards.md) | Naming, Patterns |
| Database Access | [Database Access Patterns](03_Database_Access_Patterns.md) | OSUB Usage |
| Error Handling | [Error Handling Patterns](04_Error_Handling_Patterns.md) | Error Patterns |
| Code Examples | [Code Reference](../03_Code_Reference/) | Process/Dialog Code |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

