# Background Processes Index

**Document Version:** 1.1
**Last Updated:** 2024-12-23
**Author:** CmL
**Confidence Score:** 0.95

---

## Overview

This document provides a complete index of all background processes in the Auro System.

---

## Detailed Documentation

### Dispatchers
- [p_ar_fndwk](p_ar_fndwk.md) - Find Work
- [p_ar_movdp](p_ar_movdp.md) - Move Dispatcher
- [p_ar_stkdp](p_ar_stkdp.md) - Stacker Dispatcher
- [p_ar_agvdp](p_ar_agvdp.md) - AGV Dispatcher
- [p_ar_rtndp](p_ar_rtndp.md) - RTN Dispatcher

### Completers
- [p_ar_srcmp](p_ar_srcmp.md) - Stacker Completer
- [p_ar_agvcp](p_ar_agvcp.md) - AGV Completer
- [p_ar_rtnxcp](p_ar_rtnxcp.md) - RTN Completer

### Communication Services
- [p_cc_stkcmx](p_cc_stkcmx.md) - Stacker Communications
- [p_cc_mudpcm](p_cc_mudpcm.md) - MUDP Communications
- [Kepware_PLC](Kepware_PLC.md) - PLC Communications (VB.NET)

### System Services
- [p_sy_dbini](p_sy_dbini.md) - Database Initialization ✅
- [p_sy_stats](p_sy_stats.md) - Statistics Collection ✅
- [p_sy_sm2db](p_sy_sm2db.md) - Shared Memory to Database ✅
- [p_sy_errdev](p_sy_errdev.md) - Error Device Monitor ✅
- [p_sy_erlog](p_sy_erlog.md) - Error Logger ✅
- [p_sy_emailer](p_sy_emailer.md) - Email Service
- [p_sy_watch](p_sy_watch.md) - Process Watchdog
- [p_sy_gcini](p_sy_gcini.md) - Global Control Initialization
- [p_sy_shmop](p_sy_shmop.md) - Shared Memory Operations
- [p_sy_runner](p_sy_runner.md) - Process Runner
- [p_sy_ThickServer](p_sy_ThickServer.md) - Thick Server

### Auro-Specific Services
- [p_si_AuroHost](p_si_AuroHost.md) - Auro Host Integration
- [p_si_AuroTrack](p_si_AuroTrack.md) - Auro Tracking Integration
- [p_si_stkcmx](p_si_stkcmx.md) - Site Stacker Communications
- [p_si_system](p_si_system.md) - Site System Services
- [p_si_hostsim](p_si_hostsim.md) - Host Simulator

### Tracking
- [p_tk_trkfl](p_tk_trkfl.md) - Tracking Filter
- [p_tk_trkmon](p_tk_trkmon.md) - Tracking Monitor
- [p_tk_tracker](p_tk_tracker.md) - Tracker

### Area Programs
- [p_ar_alloc](p_ar_alloc.md) - Stand Allocator ✅
- [p_ar_arive](p_ar_arive.md) - Load Arrival Handler ✅
- [p_ar_events](p_ar_events.md) - Events Handler ✅
- [p_ar_order](p_ar_order.md) - Order Processing ✅
- [p_ar_release](p_ar_release.md) - Release Operations ✅
- [p_ar_shuffle](p_ar_shuffle.md) - Shuffle Operations ✅
- [p_ar_hostcom](p_ar_hostcom.md) - Host Communications
- [p_ar_modechange](p_ar_modechange.md) - Mode Change
- [p_cc_hostcom](p_cc_hostcom.md) - Host Communications ✅

### Utilities
- [p_ut_makdb](p_ut_makdb.md) - Make Database Routines
- [p_ut_regst](p_ut_regst.md) - Register System
- [p_ut_filhol](p_ut_filhol.md) - File Hold
- [p_ut_locbd](p_ut_locbd.md) - Location Database
- [p_ut_exrsr](p_ut_exrsr.md) - Exercise Stacker
- [p_ut_SR_times](p_ut_SR_times.md) - Stacker Times
- [p_ut_Import_Load](p_ut_Import_Load.md) - Import Load
- [p_ut_Import_Kind](p_ut_Import_Kind.md) - Import Kind
- [p_ut_Import_Entry](p_ut_Import_Entry.md) - Import Entry
- [p_ut_Part_Master](p_ut_Part_Master.md) - Part Master
- [p_ut_Recv_Load](p_ut_Recv_Load.md) - Receive Load
- [p_ut_Recv_Create](p_ut_Recv_Create.md) - Receive Create
- [p_ut_Retrieve_Time](p_ut_Retrieve_Time.md) - Retrieve Time
- [p_ut_Retrieve_Times](p_ut_Retrieve_Times.md) - Retrieve Times
- [p_ut_bench_time](p_ut_bench_time.md) - Benchmark Time
- [p_ut_data_in](p_ut_data_in.md) - Data Input
- [p_ut_DBexport](p_ut_DBexport.md) - Database Export
- [p_ut_DBimport](p_ut_DBimport.md) - Database Import
- [p_ut_AGV_Stand](p_ut_AGV_Stand.md) - AGV Stand
- [p_ut_mail](p_ut_mail.md) - Mail Utility
- [p_ut_plcsim](p_ut_plcsim.md) - PLC Simulator

---

## Process Summary Table

| Process | Category | Purpose | Location |
|---------|----------|---------|----------|
| p_ar_fndwk | Dispatcher | Find queued moves | area\p_ar_fndwk |
| p_ar_movdp | Dispatcher | Station moves | area\p_ar_movdp |
| p_ar_stkdp | Dispatcher | Stacker work assignment | area\p_ar_stkdp |
| p_ar_agvdp | Dispatcher | AGV work assignment | area\p_ar_agvdp |
| p_ar_rtndp | Dispatcher | RTNX work assignment | area\p_ar_rtndp |
| p_ar_srcmp | Completer | Stacker completion | area\p_ar_srcmp |
| p_ar_agvcp | Completer | AGV completion | area\p_ar_agvcp |
| p_ar_rtnxcp | Completer | RTNX completion | area\p_ar_rtnxcp |
| p_cc_stkcmx | Communication | Stacker communications | area\p_cc_stkcmx |
| p_cc_mudpcm | Communication | MUDP communications | area\p_cc_mudpcm |
| Kepware_PLC | Communication | PLC communications | MSVB Programs\Kepware_PLC |
| p_sy_dbini | System | Database initialization | sysc\p_sy_dbini |
| p_sy_stats | System | Statistics collection | sysc\p_sy_stats |
| p_sy_emailer | System | Email notifications | MSVB Programs\p_sy_emailer |
| p_sy_erlog | System | Error logging | sysc\p_sy_erlog |
| p_sy_errdev | System | Error device monitor | sysc\p_sy_errdev |
| p_sy_watch | System | Process watchdog | sysc\p_sy_watch |
| p_sy_sm2db | System | Shared memory to DB | sysc\p_sy_sm2db |
| p_sy_gcini | System | Global control init | sysc\p_sy_gcini |
| p_sy_shmop | System | Shared memory operations | sysc\p_sy_shmop |
| p_sy_ThickServer | System | Thick server | sysc\p_sy_ThickServer |
| p_si_AuroHost | Auro-Specific | Auro host integration | simu\p_si_AuroHost |
| p_si_AuroTrack | Auro-Specific | Auro tracking | simu\p_si_AuroTrack |
| p_si_stkcmx | Auro-Specific | Site stacker comms | simu\p_si_stkcmx |
| p_si_system | Auro-Specific | Site system services | simu\p_si_system |
| p_si_hostsim | Auro-Specific | Host simulator | simu\p_si_hostsim |
| p_tk_trkfl | Tracking | Tracking filter | trak\p_tk_trkfl |
| p_tk_trkmon | Tracking | Tracking monitor | trak\p_tk_trkmon |
| p_tk_tracker | Tracking | Tracker | trak\p_tk_tracker |
| p_ar_arive | Area | Arrival handler | area\p_ar_arive |
| p_ar_order | Area | Order handler | area\p_ar_order |
| p_ar_release | Area | Releaser | area\p_ar_release |
| p_ar_events | Area | Events handler | area\p_ar_events |
| p_ar_hostcom | Area | Host communications | area\p_ar_hostcom |
| p_ar_modechange | Area | Mode change | area\p_ar_modechange |
| p_ar_alloc | Area | Allocator | area\p_ar_alloc |
| p_cc_hostcom | Area | Host communications | area\p_cc_hostcom |

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md)
- [Component Dependency Map](../../01_System_Architecture/01_Component_Dependency_Map.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Process Details | Individual process files | Process Documentation |
| Startup Sequence | [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md) | Startup Order |
| Dependency Map | [Component Dependency Map](../../01_System_Architecture/01_Component_Dependency_Map.md) | Dependencies |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

