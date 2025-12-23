# Executive Summary

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.95

---

## Overview

The Auro System is a comprehensive Material Handling Controller (MHC) / Warehouse Control System (WCS) built on the MEM (Murata Equipment Manager) framework. This document provides a high-level overview of the system's purpose, capabilities, architecture, and operational scale.

---

## System Purpose

The Auro System manages automated material handling equipment within a warehouse environment, including:

- **22 Stacker Cranes** (SR01-SR22) for automated storage and retrieval
- **4 PLCs** (PLC01-PLC04) for conveyor and station control
- **AGV/RTNX Vehicles** for horizontal material transport
- **Conveyor Systems** for load movement between stations
- **Host Integration** with WCS/MES systems for order management

The system provides centralized control, real-time load tracking, automated dispatching, error handling, and comprehensive reporting capabilities.

---

## Key Capabilities

### 1. Load Movement Management
- Automated store and retrieve operations
- Multi-cycle load transfers (MUDP protocol)
- Route optimization and scheduling
- Station-to-station moves

### 2. Equipment Control
- Stacker crane control (Quad, Double Deep, Space Storage configurations)
- AGV/RTNX vehicle management and routing
- PLC integration via Kepware OPC server
- Real-time equipment status monitoring

### 3. Inventory Tracking
- Real-time load location tracking
- Inventory reconciliation
- Load problem detection and resolution
- Location availability management

### 4. Host Integration
- Inbound/outbound message handling (Auro-specific: `p_si_AuroHost`, `p_si_AuroTrack`)
- Order management (`MHC_ORDER`, `MHC_ORDER_LINE`)
- Pallet tracking (`MHC_PALLET`)
- Material request processing

### 5. Error Handling & Recovery
- Automated error detection and logging
- Recovery procedures for equipment faults
- Error history tracking (`MHC_ERROR_HISTORY`)
- Operator notification system

### 6. Statistics & Reporting
- Performance metrics collection
- Statistical roll-up (half-hourly, hourly, daily, monthly)
- Email notifications (`p_sy_emailer`)
- Dashboard displays (`frm_Stats`, `frm_SysMon`)

---

## System Scale

| Component Type | Count | Description |
|----------------|-------|-------------|
| **Stacker Cranes** | 22 | SR01-SR22 |
| **PLC Devices** | 4 | PLC01-PLC04 |
| **Background Processes** | 30+ | Dispatchers, completers, communications, utilities |
| **UI Dialogs** | 100+ | Operations, inquiries, maintenance, utilities |
| **Database Tables** | 60+ | Core, equipment, orders, errors, security, statistics |
| **Configuration Files** | 40+ | XML, INI, CNF files |
| **Communication Protocols** | 4+ | MUDP, MASPA, CRC, OPC (Kepware) |

---

## Technology Stack

- **Languages:** C++, VB.NET
- **Database:** Microsoft SQL Server Standard Edition
- **UI Framework:** Windows Forms (VB.NET)
- **Communication:** Kepware OPC Server, Custom protocols (MUDP, MASPA, CRC)
- **IPC:** Mapped Memory (Shared Memory Files)
- **Build System:** Visual Studio 2017

---

## Primary Codebases

### Development Environment
- **Path:** `D:\ICIS\AuroDev\clogan\AuroDev\`
- **C++ Source:** `MSVC Programs\`
- **VB.NET Source:** `MSVB Programs\dilg\mhcMenu\`
- **Database Routines:** `MSVB Programs\osub\`

### Production/Runtime Environment
- **Path:** `D:\Auro\`
- **Executables:** `Exec\` (67 executables)
- **Configuration:** `File\`
- **Logs:** `Logs\`
- **Mapped Memory:** `shmf\`

---

## Critical System Processes

### Dispatchers
- `p_ar_fndwk` - Find Work (scans for queued moves)
- `p_ar_movdp` - Move Dispatcher (station moves)
- `p_ar_stkdp` - Stacker Dispatcher (22 cranes)
- `p_ar_agvdp` - AGV Dispatcher
- `p_ar_rtndp` - RTN Dispatcher

### Completers
- `p_ar_srcmp` - Stacker Completer
- `p_ar_agvcp` - AGV Completer
- `p_ar_rtnxcp` - RTN Completer

### Communication Services
- `p_cc_stkcmx` - Stacker Communications (MUDP/MASPA/CRC)
- `p_cc_mudpcm` - MUDP Communications (AGV/RTNX)
- `Kepware_PLC` - PLC Communications (OPC)

### System Services
- `p_sy_dbini` - Database Initialization
- `p_sy_stats` - Statistics Collection
- `p_sy_emailer` - Email Notifications
- `p_sy_erlog` - Error Logging
- `p_sy_watch` - Process Monitoring

### Auro-Specific Processes
- `p_si_AuroHost` - Auro Host Integration
- `p_si_AuroTrack` - Auro Tracking Integration
- `p_si_stkcmx` - Site-specific Stacker Communications
- `p_si_system` - Site-specific System Services

---

## Documentation Structure

This documentation follows the MHC-Developers-Guide structure:

- **00_Project_Overview/** - System overview and quick start
- **01_System_Architecture/** - Architecture diagrams and component maps
- **02_Coding_Standards/** - Coding standards and patterns
- **03_Code_Reference/** - Detailed code documentation
- **04_Database_Reference/** - Database schema documentation
- **05_Workflows/** - Critical workflow traces
- **06_Configuration/** - Configuration file documentation
- **07_Troubleshooting/** - Troubleshooting guides
- **08_Project_Specific/** - Auro-specific customizations

---

## Related Documents

- [System Scale and Scope](01_System_Scale_and_Scope.md)
- [Quick Start Guide](02_Quick_Start_Guide.md)
- [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
- [MHC-Developers-Guide](https://github.com/0xPliny/MHC-Developers-Guide) - Generic MHC patterns and standards

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| System Architecture | [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md) | Overview |
| Background Processes | [Code Reference](../03_Code_Reference/00_Code_Index.md) | Background Processes |
| Database Schema | [Database Overview](../04_Database_Reference/00_Database_Overview.md) | Schema |
| Workflows | [Workflow Index](../05_Workflows/00_Workflow_Index.md) | Workflows |
| Auro Customizations | [Auro Specific Overview](../08_Project_Specific/00_Auro_Specific_Overview.md) | Custom Features |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

