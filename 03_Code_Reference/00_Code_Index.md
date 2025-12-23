# Code Reference Index

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides a complete index of all source code files in the Auro System, organized by component type.

---

## Source Code Locations

### C++ Source
- **Path:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\`
- **Categories:**
  - `area\` - Area programs (dispatchers, completers)
  - `sysc\` - System control programs
  - `simu\` - Site integration programs (Auro-specific)
  - `trak\` - Tracking programs
  - `util\` - Utility programs

### VB.NET Source
- **Path:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\`
- **Categories:**
  - UI Dialogs (`frm_*.vb`)
  - Database Access (`osub\z_*.vb`)
  - Classes (`Classes\*.vb`)
  - Controls (`Controls\*.vb`)

---

## Background Processes

### Dispatchers
- [p_ar_fndwk](01_Background_Processes/p_ar_fndwk.md) - Find Work
- [p_ar_movdp](01_Background_Processes/p_ar_movdp.md) - Move Dispatcher
- [p_ar_stkdp](01_Background_Processes/p_ar_stkdp.md) - Stacker Dispatcher
- [p_ar_agvdp](01_Background_Processes/p_ar_agvdp.md) - AGV Dispatcher
- [p_ar_rtndp](01_Background_Processes/p_ar_rtndp.md) - RTN Dispatcher

### Completers
- [p_ar_srcmp](01_Background_Processes/p_ar_srcmp.md) - Stacker Completer
- [p_ar_agvcp](01_Background_Processes/p_ar_agvcp.md) - AGV Completer
- [p_ar_rtnxcp](01_Background_Processes/p_ar_rtnxcp.md) - RTN Completer

### Communication Services
- [p_cc_stkcmx](01_Background_Processes/p_cc_stkcmx.md) - Stacker Communications
- [p_cc_mudpcm](01_Background_Processes/p_cc_mudpcm.md) - MUDP Communications
- [Kepware_PLC](01_Background_Processes/Kepware_PLC.md) - PLC Communications

### System Services
- [p_sy_dbini](01_Background_Processes/p_sy_dbini.md) - Database Initialization
- [p_sy_stats](01_Background_Processes/p_sy_stats.md) - Statistics Collection
- [p_sy_emailer](01_Background_Processes/p_sy_emailer.md) - Email Service
- [p_sy_erlog](01_Background_Processes/p_sy_erlog.md) - Error Logging
- [p_sy_errdev](01_Background_Processes/p_sy_errdev.md) - Error Device Monitor
- [p_sy_watch](01_Background_Processes/p_sy_watch.md) - Process Watchdog
- [p_sy_sm2db](01_Background_Processes/p_sy_sm2db.md) - Shared Memory to Database
- [p_sy_gcini](01_Background_Processes/p_sy_gcini.md) - Global Control Initialization
- [p_sy_shmop](01_Background_Processes/p_sy_shmop.md) - Shared Memory Operations
- [p_sy_ThickServer](01_Background_Processes/p_sy_ThickServer.md) - Thick Server

### Auro-Specific Services
- [p_si_AuroHost](01_Background_Processes/p_si_AuroHost.md) - Auro Host Integration
- [p_si_AuroTrack](01_Background_Processes/p_si_AuroTrack.md) - Auro Tracking Integration
- [p_si_stkcmx](01_Background_Processes/p_si_stkcmx.md) - Site Stacker Communications
- [p_si_system](01_Background_Processes/p_si_system.md) - Site System Services
- [p_si_hostsim](01_Background_Processes/p_si_hostsim.md) - Host Simulator

### Tracking
- [p_tk_trkfl](01_Background_Processes/p_tk_trkfl.md) - Tracking Filter
- [p_tk_trkmon](01_Background_Processes/p_tk_trkmon.md) - Tracking Monitor
- [p_tk_tracker](01_Background_Processes/p_tk_tracker.md) - Tracker

### Area Programs
- [p_ar_arive](01_Background_Processes/p_ar_arive.md) - Arrival Handler
- [p_ar_order](01_Background_Processes/p_ar_order.md) - Order Handler
- [p_ar_release](01_Background_Processes/p_ar_release.md) - Releaser
- [p_ar_events](01_Background_Processes/p_ar_events.md) - Events Handler
- [p_ar_hostcom](01_Background_Processes/p_ar_hostcom.md) - Host Communications
- [p_ar_modechange](01_Background_Processes/p_ar_modechange.md) - Mode Change
- [p_ar_alloc](01_Background_Processes/p_ar_alloc.md) - Allocator
- [p_cc_hostcom](01_Background_Processes/p_cc_hostcom.md) - Host Communications

### Utilities
- [p_ut_makdb](01_Background_Processes/p_ut_makdb.md) - Make Database Routines
- [p_ut_regst](01_Background_Processes/p_ut_regst.md) - Register System
- [p_ut_filhol](01_Background_Processes/p_ut_filhol.md) - File Hold
- [p_ut_locbd](01_Background_Processes/p_ut_locbd.md) - Location Database
- [p_ut_exrsr](01_Background_Processes/p_ut_exrsr.md) - Exercise Stacker
- [p_ut_SR_times](01_Background_Processes/p_ut_SR_times.md) - Stacker Times

---

## UI Dialogs

### Operations
- [frm_Store](02_UI_Dialogs/frm_Store.md) - Store Operation
- [frm_Manual_Retrieve](02_UI_Dialogs/frm_Manual_Retrieve.md) - Manual Retrieve
- [frm_MoveInq](02_UI_Dialogs/frm_MoveInq.md) - Move Inquiry

### Inquiries
- [frm_Inventory_Inquiry](02_UI_Dialogs/frm_Inventory_Inquiry.md) - Inventory Inquiry
- [frm_LoadProb_Inquiry](02_UI_Dialogs/frm_LoadProb_Inquiry.md) - Load Problem Inquiry
- [frm_Order_Inquiry](02_UI_Dialogs/frm_Order_Inquiry.md) - Order Inquiry
- [frm_Pallet_Inquiry](02_UI_Dialogs/frm_Pallet_Inquiry.md) - Pallet Inquiry
- [frm_Location_Inquiry](02_UI_Dialogs/frm_Location_Inquiry.md) - Location Inquiry

### Maintenance
- [frm_Move_Maintenance](02_UI_Dialogs/frm_Move_Maintenance.md) - Move Maintenance
- [frm_Stand_Maintenance](02_UI_Dialogs/frm_Stand_Maintenance.md) - Stand Maintenance
- [frm_Part_Maintenance](02_UI_Dialogs/frm_Part_Maintenance.md) - Part Maintenance
- [frm_Host_Maintenance](02_UI_Dialogs/frm_Host_Maintenance.md) - Host Maintenance

### System
- [frm_SysMon](02_UI_Dialogs/frm_SysMon.md) - System Monitor
- [frm_Stats](02_UI_Dialogs/frm_Stats.md) - Statistics
- [frm_Dash_Board](02_UI_Dialogs/frm_Dash_Board.md) - Dashboard

### Utilities
- [frmAGVUtil](02_UI_Dialogs/frmAGVUtil.md) - AGV Utility
- [frmRTNXUtil](02_UI_Dialogs/frmRTNXUtil.md) - RTNX Utility
- [frm_TCar_Utility](02_UI_Dialogs/frm_TCar_Utility.md) - TCar Utility

### Security
- [frmPassword](02_UI_Dialogs/frmPassword.md) - Password/Login
- [frmUserGroupM](02_UI_Dialogs/frmUserGroupM.md) - User Group Maintenance
- [frm_Enable_Dialog](02_UI_Dialogs/frm_Enable_Dialog.md) - Enable Dialog

---

## Shared Libraries

### Core Libraries
- [cc_stk](03_Shared_Libraries/cc_stk.md) - Stacker Classes
- [cc_std](03_Shared_Libraries/cc_std.md) - Station Classes
- [cc_plc](03_Shared_Libraries/cc_plc.md) - PLC Classes
- [cc_agv](03_Shared_Libraries/cc_agv.md) - AGV Classes
- [cc_rtnx](03_Shared_Libraries/cc_rtnx.md) - RTNX Classes

### Database Libraries
- [dsub](03_Shared_Libraries/dsub.md) - Database Subroutines
- [osub](03_Shared_Libraries/osub.md) - Operating System Subroutines

### Interoperability
- [CInterop](03_Shared_Libraries/CInterop.md) - C++/VB.NET Interop
- [MHC_Interop](03_Shared_Libraries/MHC_Interop.md) - MHC Interop

---

## Related Documents

- [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
- [Database Overview](../04_Database_Reference/00_Database_Overview.md)
- [Workflow Index](../05_Workflows/00_Workflow_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Background Processes | [01_Background_Processes/](01_Background_Processes/) | Process List |
| UI Dialogs | [02_UI_Dialogs/](02_UI_Dialogs/) | Dialog List |
| Shared Libraries | [03_Shared_Libraries/](03_Shared_Libraries/) | Library List |
| Architecture | [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md) | Component Map |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

