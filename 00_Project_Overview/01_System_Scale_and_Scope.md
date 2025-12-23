# System Scale and Scope

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.95

---

## Overview

This document provides detailed metrics and scale information for the Auro System, including component counts, file statistics, and operational parameters.

---

## Equipment Scale

### Stacker Cranes
- **Total Count:** 22 cranes
- **Naming:** SR01 through SR22
- **Control:** Managed by `p_ar_stkdp` (Stacker Dispatcher) and `p_ar_srcmp` (Stacker Completer)
- **Communication:** `p_cc_stkcmx` (Stacker Communications) and `p_si_stkcmx` (Site-specific)
- **Database Table:** `MHC_SM2DB_STACKER`

### PLC Devices
- **Total Count:** 4 PLCs
- **Naming:** PLC01, PLC02, PLC03, PLC04
- **Control:** Managed via Kepware OPC Server (`Kepware_PLC.exe`)
- **Configuration:** `KEPWAREplcdef_A.xml`
- **Database Tables:** `MHC_SM2DB_COMMS`, `MHC_SM2DB_ELT`

### Vehicles
- **AGVs:** Managed by `p_ar_agvdp` and `p_ar_agvcp`
- **RTNX:** Managed by `p_ar_rtndp` and `p_ar_rtnxcp`
- **Configuration:** `vehicle.xml`, `points.xml`
- **Communication:** `p_cc_mudpcm` (MUDP protocol)

### Stations
- **Control:** Managed by station classes (`cc_std`)
- **Database Table:** `MHC_SM2DB_STATION`
- **Configuration:** Station definitions in configuration files

---

## Software Component Scale

### Background Processes

| Category | Count | Processes |
|----------|-------|-----------|
| **Dispatchers** | 5 | `p_ar_fndwk`, `p_ar_movdp`, `p_ar_stkdp`, `p_ar_agvdp`, `p_ar_rtndp` |
| **Completers** | 3 | `p_ar_srcmp`, `p_ar_agvcp`, `p_ar_rtnxcp` |
| **Communication** | 3 | `p_cc_stkcmx`, `p_cc_mudpcm`, `Kepware_PLC` |
| **System Services** | 10+ | `p_sy_dbini`, `p_sy_stats`, `p_sy_emailer`, `p_sy_erlog`, `p_sy_errdev`, `p_sy_watch`, `p_sy_sm2db`, `p_sy_gcini`, `p_sy_shmop`, `p_sy_ThickServer` |
| **Site Integration** | 5+ | `p_si_AuroHost`, `p_si_AuroTrack`, `p_si_stkcmx`, `p_si_system`, `p_si_hostsim` |
| **Tracking** | 3 | `p_tk_trkfl`, `p_tk_trkmon`, `p_tk_tracker` |
| **Utilities** | 20+ | `p_ut_makdb`, `p_ut_regst`, `p_ut_filhol`, `p_ut_locbd`, `p_ut_exrsr`, `p_ut_SR_times`, etc. |
| **Area Programs** | 8+ | `p_ar_arive`, `p_ar_order`, `p_ar_release`, `p_ar_events`, `p_ar_hostcom`, `p_ar_modechange`, `p_ar_alloc`, `p_cc_hostcom` |
| **Total** | **60+** | |

### UI Dialogs

| Category | Count | Examples |
|----------|-------|----------|
| **Operations** | 20+ | `frm_Store`, `frm_Manual_Retrieve`, `frm_MoveInq` |
| **Inquiries** | 30+ | `frm_Inventory_Inquiry`, `frm_LoadProb_Inquiry`, `frm_Order_Inquiry`, `frm_Pallet_Inquiry`, `frm_Location_Inquiry` |
| **Maintenance** | 15+ | `frm_Move_Maintenance`, `frm_Stand_Maintenance`, `frm_Part_Maintenance`, `frm_Host_Maintenance` |
| **System** | 10+ | `frm_SysMon`, `frm_Stats`, `frm_Dash_Board` |
| **Utilities** | 10+ | `frmAGVUtil`, `frmRTNXUtil`, `frm_TCar_Utility`, `frm_Label_Print_Utility` |
| **Security** | 5+ | `frmPassword`, `frmUserGroupM`, `frm_Enable_Dialog` |
| **Total** | **100+** | |

### Database Tables

| Category | Count | Examples |
|----------|-------|----------|
| **Core Tables** | 15+ | `MHC_MOVS`, `MHC_LOAD`, `MHC_LOCATION`, `MHC_INVENTORY` |
| **Equipment Tables** | 10+ | `MHC_SM2DB_STACKER`, `MHC_SM2DB_STATION`, `MHC_SM2DB_COMMS`, `MHC_SM2DB_ELT` |
| **Order Tables** | 5+ | `MHC_ORDER`, `MHC_ORDER_LINE`, `MHC_PALLET` |
| **Error Tables** | 8+ | `MHC_ERROR`, `MHC_ERROR_HISTORY`, `MHC_ERROR_QUEUE`, `MHC_ERROR_DESCRIPTION` |
| **Security Tables** | 6+ | `MHC_SEC_USERS`, `MHC_SEC_GROUP`, `MHC_SEC_USER_GROUP`, `MHC_SEC_MENU_ITEM` |
| **Statistics Tables** | 3+ | `MHC_STATISTICS`, `MHC_STATISTICS_CONTROL` |
| **Log Tables** | 15+ | `MHC_MOVS_LOG`, `MHC_LOAD_LOG`, `MHC_INVENTORY_LOG`, etc. |
| **Views** | 20+ | `VIEW_LOAD_INVT`, `VIEW_LOCN_LOAD`, `VIEW_RACK_INQ`, etc. |
| **Total** | **80+** | Tables + Views |

### Configuration Files

| Type | Count | Examples |
|------|-------|----------|
| **XML Files** | 10+ | `database_tables.xml`, `Stacker.xml`, `vehicle.xml`, `points.xml`, `KEPWAREplcdef_A.xml`, `menu.cnf` |
| **CNF Files** | 15+ | `dcbcnf_A.cnf`, `Dcbdef_A.cnf`, `logdef.cnf`, `timer.cnf`, `wrm_Auro_a.cnf` |
| **MSG Files** | 2+ | `english.msg`, `icis_english.msg` |
| **Total** | **40+** | |

---

## Source Code Statistics

### C++ Source Files
- **Total Files:** 587 C++ files
- **Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\`
- **Categories:**
  - Area programs: `area\p_ar_*`
  - Communication: `ccsub\` (if exists) or `area\p_cc_*`
  - System: `sysc\p_sy_*`
  - Site Integration: `simu\p_si_*`
  - Tracking: `trak\p_tk_*`
  - Utilities: `util\p_ut_*`

### VB.NET Source Files
- **Total Files:** 1,187 VB.NET files
- **Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\`
- **Categories:**
  - UI Dialogs: `frm_*.vb` (100+ forms)
  - Database Access: `osub\z_*.vb` (60+ generated files)
  - Classes: `Classes\*.vb`
  - Controls: `Controls\*.vb`

### Executables
- **Total Count:** 67 executables
- **Location:** `D:\Auro\Exec\`
- **Categories:**
  - Background processes: `p_ar_*.exe`, `p_cc_*.exe`, `p_sy_*.exe`, etc.
  - UI: `mhcMenu.exe`
  - Utilities: `p_ut_*.exe`
  - Viewers: `view_*.exe`

---

## Operational Parameters

### Communication Intervals
- **PLC Polling:** 100-500 ms (configurable)
- **Equipment Polling:** 200-1000 ms (configurable)
- **Dispatcher Polling:** 100-250 ms
- **Completer Polling:** 100-250 ms

### Database Operations
- **Transaction Logging:** Enabled for all log tables
- **Statistics Roll-up:** Half-hourly → Hourly → Daily → Monthly
- **Backup Frequency:** Daily full backups, hourly transaction log backups (recommended)

### Shared Memory
- **Mapped Memory Files:** Multiple `.shm` files in `D:\Auro\shmf\`
- **Persistence:** Flushed to disk periodically
- **Access:** Concurrent read/write by multiple processes

---

## Performance Metrics

### Throughput Capacity
- **Stacker Cranes:** 22 cranes operating concurrently
- **Move Processing:** Handles hundreds of moves per hour
- **Database Transactions:** High-volume transaction processing

### System Resources
- **CPU:** Multi-core processor required (4+ cores recommended)
- **RAM:** 16 GB minimum recommended
- **Storage:** RAID 5 array for database and mapped memory
- **Network:** Ethernet for PLC and equipment communication

---

## Related Documents

- [Executive Summary](00_Executive_Summary.md)
- [Quick Start Guide](02_Quick_Start_Guide.md)
- [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
- [Database Overview](../04_Database_Reference/00_Database_Overview.md)
- [Code Reference Index](../03_Code_Reference/00_Code_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Background Processes | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process List |
| UI Dialogs | [Code Reference](../03_Code_Reference/02_UI_Dialogs/) | Dialog List |
| Database Tables | [Database Reference](../04_Database_Reference/00_Database_Overview.md) | Table List |
| Configuration Files | [Configuration Overview](../06_Configuration/00_Configuration_Overview.md) | File List |
| System Architecture | [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md) | Component Map |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

