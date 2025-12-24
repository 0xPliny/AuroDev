# Background Process Source File Inventory

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  

---

## Overview

This document provides a comprehensive inventory of all background process source files in the ICIS/AuroDev codebase, tracking which processes have detailed documentation.

---

## Documentation Status Summary

| Category | Total | Documented | Pending |
|----------|-------|------------|---------|
| Area Programs (p_ar_*) | 10 | 4 | 6 |
| Communication Programs (p_cc_*) | 3 | 1 | 2 |
| System Programs (p_sy_*) | 9 | 1 | 8 |
| Simulation Programs (p_si_*) | 25 | 1 | 24 |
| Utility Programs (p_ut_*) | 26 | 0 | 26 |
| **Total** | **73** | **7** | **66** |

---

## Area Programs (p_ar_*)

| File | Description | Lines | Documented |
|------|-------------|-------|------------|
| `p_ar_fndwk.cpp` | Find Work - Scans for pending moves | ~500 | ✅ Yes |
| `p_ar_movdp.cpp` | Move Dispatcher - Station-to-station moves | ~800 | ✅ Yes |
| `p_ar_stkdp.cpp` | Stacker Dispatcher - Assigns work to stackers | ~1500 | ✅ Yes |
| `p_ar_srcmp.cpp` | Stacker Completer - Confirms stacker completions | ~1200 | ✅ Yes |
| `p_ar_alloc.cpp` | Stand Allocator - Allocates work based on orders | ~231 | ❌ Pending |
| `p_ar_arive.cpp` | Load Arrival Handler - Processes conveyor arrivals | ~7045 | ❌ Pending |
| `p_ar_events.cpp` | Events Handler - Periodic system events | ~6095 | ❌ Pending |
| `p_ar_order.cpp` | Order Processing - Manages pallet orders | ~3331 | ❌ Pending |
| `p_ar_release.cpp` | Release Operations - Controls case releases | ~3552 | ❌ Pending |
| `p_ar_shuffle.cpp` | Shuffle Operations - Optimizes load positions | ~2392 | ❌ Pending |

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\`

---

## Communication Programs (p_cc_*)

| File | Description | Lines | Documented |
|------|-------------|-------|------------|
| `p_cc_stkcmx.cpp` | Stacker Communications - Serial/TCP to stackers | ~2000 | ✅ Yes |
| `p_cc_stkcmO.cpp` | Stacker Communications (Oracle) | ~1800 | ❌ Pending |
| `p_cc_hostcom.cpp` | Host Communications - Interface with host system | ~1500 | ❌ Pending |

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\` and `Base\trunk\MSVC Programs\ccom\`

---

## System Programs (p_sy_*)

| File | Description | Lines | Documented |
|------|-------------|-------|------------|
| `p_sy_dbini.cpp` | Database Initialization - Creates/populates DB | ~3000 | ✅ Yes |
| `p_sy_stats.cpp` | Statistics Collection - Gathers performance data | ~1500 | ❌ Pending |
| `p_sy_sm2db.cpp` | Shared Memory to Database - Syncs SM to DB | ~800 | ❌ Pending |
| `p_sy_errdev.cpp` | Error Device - Error handling device | ~600 | ❌ Pending |
| `p_sy_erlog.cpp` | Error Logger - Central error logging | ~500 | ❌ Pending |
| `p_sy_watch.cpp` | Process Watchdog - Monitors process health | ~400 | ❌ Pending |
| `p_sy_runner.cpp` | Process Runner - Launches background processes | ~350 | ❌ Pending |
| `p_sy_shmop.cpp` | Shared Memory Operations - SM utilities | ~450 | ❌ Pending |
| `p_sy_gcini.cpp` | GC Initialization - Garbage collection init | ~300 | ❌ Pending |

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\sysc\` and `Base\trunk\MSVC Programs\sysc\`

---

## Simulation Programs (p_si_*)

| File | Description | Lines | Documented |
|------|-------------|-------|------------|
| `p_si_AuroHost.cpp` | Auro Host Integration | ~2000 | ✅ Yes |
| `p_si_AuroTrack.cpp` | Auro Tracking Integration | ~1500 | ❌ Pending |
| `p_si_AutoMV.cpp` | Auto Move Simulation | ~800 | ❌ Pending |
| `p_si_ablcm.cpp` | Allen-Bradley Communication Simulator | ~600 | ❌ Pending |
| `p_si_abplc.cpp` | Allen-Bradley PLC Simulator | ~700 | ❌ Pending |
| `p_si_agvcm.cpp` | AGV Communication Simulator | ~900 | ❌ Pending |
| `p_si_agvsm.cpp` | AGV Simulation | ~1200 | ❌ Pending |
| `p_si_asccm.cpp` | ASCII Communication Simulator | ~500 | ❌ Pending |
| `p_si_bcpcm.cpp` | Barcode PC Communication Simulator | ~400 | ❌ Pending |
| `p_si_bcrcm.cpp` | Barcode Reader Communication Simulator | ~450 | ❌ Pending |
| `p_si_display.cpp` | Display Simulation | ~300 | ❌ Pending |
| `p_si_mudpcm.cpp` | MUDP Communication Simulator | ~800 | ❌ Pending |
| `p_si_plcstd.cpp` | PLC Standard Simulator | ~600 | ❌ Pending |
| `p_si_robot.cpp` | Robot Simulation | ~500 | ❌ Pending |
| `p_si_rtncm.cpp` | RTN Communication Simulator | ~700 | ❌ Pending |
| `p_si_sgvcm.cpp` | SGV Communication Simulator | ~600 | ❌ Pending |
| `p_si_slpcm.cpp` | SLP Communication Simulator | ~500 | ❌ Pending |
| `p_si_stkcm.cpp` | Stacker Communication Simulator | ~900 | ❌ Pending |
| `p_si_stkcmx.cpp` | Stacker Communication X Simulator | ~1000 | ❌ Pending |
| `p_si_stkcmO.cpp` | Stacker Communication O Simulator | ~950 | ❌ Pending |
| `p_si_system.cpp` | System Simulation | ~800 | ❌ Pending |
| `p_si_track.cpp` | Track Simulation | ~600 | ❌ Pending |
| `p_si_voyager.cpp` | Voyager Simulation | ~700 | ❌ Pending |
| `p_si_weicm.cpp` | Weight Communication Simulator | ~400 | ❌ Pending |

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\simu\`

---

## Utility Programs (p_ut_*)

| File | Description | Lines | Documented |
|------|-------------|-------|------------|
| `p_ut_makdb.cpp` | Make Database Routines - Generates OSUB code | ~3500 | ❌ Pending |
| `p_ut_regst.cpp` | Register System - Registers MEM environment | ~800 | ❌ Pending |
| `p_ut_vbdef.cpp` | VB Definitions - Generates VB constant files | ~600 | ❌ Pending |
| `p_ut_flush.cpp` | Flush Shared Memory - Flushes SM to disk | ~300 | ❌ Pending |
| `p_ut_locbd.cpp` | Location Build - Generates rack locations | ~1200 | ❌ Pending |
| `p_ut_plcsim.cpp` | PLC Simulator | ~900 | ❌ Pending |
| `p_ut_mail.cpp` | Mail Utility - Email notifications | ~500 | ❌ Pending |
| `p_ut_filhol.cpp` | File Hold - File management utility | ~400 | ❌ Pending |
| `p_ut_exrsr.cpp` | Exercise Stacker - Stacker test utility | ~600 | ❌ Pending |
| `p_ut_exrsr_2.cpp` | Exercise Stacker V2 | ~650 | ❌ Pending |
| `p_ut_data_in.cpp` | Data Import - Import data utility | ~800 | ❌ Pending |
| `p_ut_bench_time.cpp` | Benchmark Time - Performance benchmarking | ~400 | ❌ Pending |
| `p_ut_SR_times.cpp` | Stacker Times - Stacker timing analysis | ~500 | ❌ Pending |
| `p_ut_Retrieve_Time.cpp` | Retrieve Time - Retrieval timing | ~450 | ❌ Pending |
| `p_ut_Recv_Load.cpp` | Receive Load - Load receipt utility | ~600 | ❌ Pending |
| `p_ut_Recv_Create.cpp` | Receive Create - Create receipt records | ~550 | ❌ Pending |
| `p_ut_Part_Master.cpp` | Part Master - Part master maintenance | ~700 | ❌ Pending |
| `p_ut_Import_Load.cpp` | Import Load - Load import utility | ~800 | ❌ Pending |
| `p_ut_Import_Kind.cpp` | Import Kind - Kind import utility | ~600 | ❌ Pending |
| `p_ut_Import_Entry.cpp` | Import Entry - Entry import utility | ~650 | ❌ Pending |
| `p_ut_DBimport.cpp` | Database Import - Full DB import | ~900 | ❌ Pending |
| `p_ut_DBexport.cpp` | Database Export - Full DB export | ~850 | ❌ Pending |
| `p_ut_AGV_Stand.cpp` | AGV Stand - AGV stand configuration | ~500 | ❌ Pending |

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\util\` and `Base\trunk\MSVC Programs\util\`

---

## Documentation Priority

### Priority 1: Core Area Programs (Batch 1-2)
1. `p_ar_alloc` - Stand Allocator
2. `p_ar_arive` - Load Arrival Handler
3. `p_ar_events` - Events Handler
4. `p_ar_order` - Order Processing
5. `p_ar_release` - Release Operations
6. `p_ar_shuffle` - Shuffle Operations

### Priority 2: System Programs (Batch 3-4)
7. `p_sy_stats` - Statistics Collection
8. `p_sy_sm2db` - Shared Memory to Database
9. `p_sy_errdev` - Error Device
10. `p_sy_erlog` - Error Logger
11. `p_sy_watch` - Process Watchdog
12. `p_sy_runner` - Process Runner
13. `p_sy_shmop` - Shared Memory Operations
14. `p_sy_gcini` - GC Initialization

### Priority 3: Communication Programs
15. `p_cc_hostcom` - Host Communications
16. `p_cc_stkcmO` - Stacker Communications (Oracle)

### Priority 4: Simulation Programs
17-40. All `p_si_*` programs

### Priority 5: Utility Programs
41-66. All `p_ut_*` programs

---

## Related Documents

- [Process Index](00_Process_Index.md)
- [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md)
- [ICIS Source File Inventory](../03_Shared_Libraries/00_ICIS_Source_File_Inventory.md)

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial creation |


