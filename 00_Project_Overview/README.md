# AuroDev Documentation

**Version:** 1.1  
**Last Updated:** 2024-12-23  
**Repository:** https://github.com/0xPliny/AuroDev

---

## Overview

This directory contains **all documentation** for the **Auro System**, a Material Handling Controller (MHC) / Warehouse Control System (WCS) built on the MEM (Murata Equipment Manager) framework.

**This documentation is a PROJECT-SPECIFIC COMPANION to the MHC-Developers-Guide.**

---

## Documentation Structure

```
00_Project_Overview/
├── README.md (this file)
├── 00_Executive_Summary.md          # System overview and capabilities
├── 01_System_Scale_and_Scope.md     # System scale and scope details
├── 00_Quick_Start_Guide.md          # Quick start for new developers
├── 01_System_Blueprint_Layer_2.md   # System blueprint
│
├── 01_System_Architecture/          # Architecture documentation
│   ├── 00_Architecture_Overview.md
│   ├── 01_Component_Dependency_Map.md
│   ├── 02_Data_Flow_Diagrams.md
│   ├── 03_Database_ERD.md
│   ├── 04_Process_Startup_Sequence.md
│   └── 05_IPC_and_Shared_Memory.md
│
├── 02_Coding_Standards/             # Coding standards and patterns
│   ├── 00_Standards_Overview.md
│   ├── 01_VB_NET_Standards.md
│   ├── 02_CPP_Standards.md
│   ├── 03_Database_Access_Patterns.md
│   └── 04_Error_Handling_Patterns.md
│
├── 03_Code_Reference/               # Detailed code documentation
│   ├── 00_Code_Index.md
│   ├── 01_Background_Processes/     # All background processes
│   ├── 02_UI_Dialogs/               # All UI dialogs
│   └── 03_Shared_Libraries/         # Shared libraries
│       ├── 00_Shared_Libraries_Index.md
│       ├── ICISDefines.md           # VB.NET constants (COMPLETE)
│       ├── global_prm.md            # C++ constants (COMPLETE)
│       ├── osub.md                  # OSUB database access
│       ├── ccsub.md                 # CCSUB equipment control
│       ├── csub.md                  # CSUB utility functions
│       └── [other library docs]
│
├── 04_Database_Reference/           # Database schema documentation
│   ├── 00_Database_Overview.md
│   └── 01_Core_Tables/
│
├── 05_Workflows/                    # Critical workflow traces
│   ├── 00_Workflow_Index.md
│   ├── 01_Inbound_Move.md
│   ├── 02_Outbound_Move.md
│   ├── 03_Equipment_Fault_Handling.md
│   ├── 04_System_Startup_Shutdown.md
│   └── 05_Manual_Intervention.md
│
├── 06_Configuration/                # Configuration file documentation
│   ├── 00_Configuration_Overview.md
│   ├── 01_CNF_Files.md
│   ├── 02_XML_Files.md
│   ├── 03_Registry_Settings.md
│   └── 04_Elements_Table.md
│
├── 07_Troubleshooting/              # Troubleshooting guides
│   ├── 00_Troubleshooting_Index.md
│   ├── 01_Common_Errors.md
│   ├── 02_Decision_Trees.md
│   └── 03_Recovery_Procedures.md
│
└── 08_Project_Specific/              # Auro-specific customizations
    ├── 00_Auro_Specific_Overview.md
    ├── 01_Custom_Features.md
    ├── 02_Site_Configuration.md
    └── 03_Known_Issues.md
```

---

## Quick Start

### For New Developers

1. **Start Here:** [00_Executive_Summary.md](00_Executive_Summary.md) - System overview
2. **Quick Start:** [00_Quick_Start_Guide.md](00_Quick_Start_Guide.md) - Get up and running
3. **Architecture:** [01_System_Architecture/00_Architecture_Overview.md](01_System_Architecture/00_Architecture_Overview.md)
4. **Code Reference:** [03_Code_Reference/00_Code_Index.md](03_Code_Reference/00_Code_Index.md)

### For Experienced Developers

- **Constants Reference:** [03_Code_Reference/03_Shared_Libraries/](03_Code_Reference/03_Shared_Libraries/)
  - [ICISDefines.md](03_Code_Reference/03_Shared_Libraries/ICISDefines.md) - All VB.NET constants
  - [global_prm.md](03_Code_Reference/03_Shared_Libraries/global_prm.md) - All C++ constants
- **Workflows:** [05_Workflows/00_Workflow_Index.md](05_Workflows/00_Workflow_Index.md)
- **Troubleshooting:** [07_Troubleshooting/00_Troubleshooting_Index.md](07_Troubleshooting/00_Troubleshooting_Index.md)

---

## System Overview

The Auro System manages:

* **22 Stacker Cranes** (SR01-SR22)
* **4 PLCs** (PLC01-PLC04)
* **AGV/RTNX Vehicles**
* **Conveyor Systems**
* **Host Integration** (WCS/MES)

---

## Key Documentation Sections

### 📋 [Executive Summary](00_Executive_Summary.md)
High-level system overview, capabilities, and architecture summary.

### 🏗️ [System Architecture](01_System_Architecture/)
Component dependencies, data flows, database ERD, startup sequences, IPC.

### 💻 [Code Reference](03_Code_Reference/)
- **Background Processes:** All `p_*` processes
- **UI Dialogs:** All `frm_*` dialogs
- **Shared Libraries:** OSUB, CCSUB, CSUB, constants

### 🗄️ [Database Reference](04_Database_Reference/)
Core tables: MHC_MOVS, MHC_LOAD, MHC_LOCATION, MHC_INVENTORY.

### 🔄 [Workflows](05_Workflows/)
Inbound/outbound moves, fault handling, startup/shutdown, manual intervention.

### ⚙️ [Configuration](06_Configuration/)
CNF files, XML files, registry settings, elements table.

### 🔧 [Troubleshooting](07_Troubleshooting/)
Common errors, decision trees, recovery procedures.

### 🎯 [Project Specific](08_Project_Specific/)
Auro-specific customizations, site configuration, known issues.

---

## Relationship to MHC-Developers-Guide

This documentation references the [MHC-Developers-Guide](https://github.com/0xPliny/MHC-Developers-Guide) for:

* Generic MHC patterns and standards
* Universal coding standards
* General troubleshooting procedures

This documentation covers:

* Auro-specific implementations
* Site-specific customizations
* Project-specific configurations

---

## Source Code Locations

* **Development:** `D:\ICIS\AuroDev\clogan\AuroDev\`
* **Production:** `D:\Auro\`
* **Executables:** `D:\Auro\Exec\`
* **Configuration:** `D:\Auro\File\`

---

## Documentation Status

✅ **COMPLETE** - All major sections documented

**Recent Updates:**
- ✅ Comprehensive ICISDefines.vb documentation (670+ lines)
- ✅ Comprehensive global_prm.h documentation (900+ lines)
- ✅ OSUB, CCSUB, CSUB library documentation
- ✅ All documentation consolidated into single location

**Last Updated:** December 23, 2024

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| System Overview | [Executive Summary](00_Executive_Summary.md) | Overview |
| Architecture | [Architecture Overview](01_System_Architecture/00_Architecture_Overview.md) | Architecture |
| Constants (VB.NET) | [ICISDefines.md](03_Code_Reference/03_Shared_Libraries/ICISDefines.md) | All Sections |
| Constants (C++) | [global_prm.md](03_Code_Reference/03_Shared_Libraries/global_prm.md) | All Sections |
| Database Access | [OSUB](03_Code_Reference/03_Shared_Libraries/osub.md) | Database Patterns |
| Equipment Control | [CCSUB](03_Code_Reference/03_Shared_Libraries/ccsub.md) | Control Classes |
| Workflows | [Workflow Index](05_Workflows/00_Workflow_Index.md) | All Workflows |

---

## License

See LICENSE file in repository root for details.

---

*This documentation is maintained as part of the AuroDev project.*

