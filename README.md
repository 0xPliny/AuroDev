# AuroDev Documentation

**Version:** 1.0  
**Last Updated:** 2024-12-22  
**Repository:** https://github.com/0xPliny/AuroDev

---

## Overview

This repository contains comprehensive documentation for the **Auro System**, a Material Handling Controller (MHC) / Warehouse Control System (WCS) built on the MEM (Murata Equipment Manager) framework.

**This documentation is a PROJECT-SPECIFIC COMPANION to the [MHC-Developers-Guide](https://github.com/0xPliny/MHC-Developers-Guide).**

---

## Documentation Structure

```
AuroDev/
├── 00_Project_Overview/          # System overview and quick start
├── 01_System_Architecture/       # Architecture diagrams and component maps
├── 02_Coding_Standards/          # Coding standards and patterns
├── 03_Code_Reference/            # Detailed code documentation
│   ├── 01_Background_Processes/  # All background processes
│   ├── 02_UI_Dialogs/            # All UI dialogs
│   └── 03_Shared_Libraries/      # Shared libraries
├── 04_Database_Reference/       # Database schema documentation
├── 05_Workflows/                 # Critical workflow traces
├── 06_Configuration/             # Configuration file documentation
├── 07_Troubleshooting/           # Troubleshooting guides
└── 08_Project_Specific/          # Auro-specific customizations
```

---

## Quick Start

1. **New Developers:** Start with [Quick Start Guide](00_Project_Overview/02_Quick_Start_Guide.md)
2. **Architecture:** Review [Architecture Overview](01_System_Architecture/00_Architecture_Overview.md)
3. **Workflows:** Study [Workflow Index](05_Workflows/00_Workflow_Index.md)
4. **Code Reference:** Use [Code Reference Index](03_Code_Reference/00_Code_Index.md)

---

## System Overview

The Auro System manages:
- **22 Stacker Cranes** (SR01-SR22)
- **4 PLCs** (PLC01-PLC04)
- **AGV/RTNX Vehicles**
- **Conveyor Systems**
- **Host Integration** (WCS/MES)

---

## Key Documentation

- [Executive Summary](00_Project_Overview/00_Executive_Summary.md)
- [System Architecture](01_System_Architecture/00_Architecture_Overview.md)
- [Database Overview](04_Database_Reference/00_Database_Overview.md)
- [Workflow Index](05_Workflows/00_Workflow_Index.md)
- [Troubleshooting Index](07_Troubleshooting/00_Troubleshooting_Index.md)

---

## Relationship to MHC-Developers-Guide

This documentation references the [MHC-Developers-Guide](https://github.com/0xPliny/MHC-Developers-Guide) for:
- Generic MHC patterns and standards
- Universal coding standards
- General troubleshooting procedures

This documentation covers:
- Auro-specific implementations
- Site-specific customizations
- Project-specific configurations

---

## Source Code Locations

- **Development:** `D:\ICIS\AuroDev\clogan\AuroDev\`
- **Production:** `D:\Auro\`
- **Executables:** `D:\Auro\Exec\`

---

## Documentation Status

✅ **COMPLETE** - All major sections documented

**Last Updated:** December 2024

---

## License

See LICENSE file for details.

