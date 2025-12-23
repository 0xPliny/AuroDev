# Process Startup Sequence

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the exact order in which Auro System processes must be started during warmstart, including dependencies and timing requirements.

---

## Startup Phases

### Phase 1: System Initialization (RUN ONCE)

#### Step 1: Shared Memory Initialization
**Process:** `p_sy_gcini.exe`
- **Type:** RUN ONCE (not a service)
- **Purpose:** Creates and initializes all mapped memory files (.shm)
- **Dependencies:** None (must run first)
- **Actions:**
  - Creates shared memory files in `D:\Auro\shmf\`
  - Initializes equipment state structures (stackers, stations, PLCs)
  - Sets up process tables
  - Initializes message queues
- **Completion:** Must complete before any other process starts

#### Step 2: Database Initialization
**Process:** `p_sy_dbini.exe`
- **Type:** RUN ONCE (not a service)
- **Purpose:** Initializes database and loads configuration
- **Dependencies:** `p_sy_gcini` must complete
- **Actions:**
  - Connects to SQL Server database
  - Populates database tables from configuration files
  - Processes `menu.cnf` to create security tables
  - Processes `database_tables.xml` to verify schema
  - Creates log tables and triggers
  - Loads initial data into mapped memory
- **Completion:** Must complete before processes that access database

#### Step 3: Error Logging Service
**Process:** `p_sy_erlog.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Background error logging service
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Monitors error queues in mapped memory
  - Writes errors to log files (`D:\Auro\Logs\`)
  - Updates `MHC_ERROR_HISTORY` table

---

### Phase 2: Communication Services

#### Step 4: PLC Communications
**Process:** `Kepware_PLC.exe` (multiple instances)
- **Type:** SERVICE (runs continuously)
- **Instances:**
  - `Kepware_PLC - PLC01_In` (reads input tags)
  - `Kepware_PLC - PLC01_Out` (writes output tags)
  - `Kepware_PLC - PLC02` (MMC-211)
  - `Kepware_PLC - PLC03` (MMC-212)
  - `Kepware_PLC - PLC04` (Premier Tech)
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Connects to Kepware OPC server
  - Reads/writes PLC tags
  - Updates mapped memory with PLC state

#### Step 5: Stacker Communications
**Process:** `p_cc_stkcmx.exe` (Region 1)
- **Type:** SERVICE (runs continuously)
- **Purpose:** Handles communication with 22 stacker cranes
- **Dependencies:** `p_sy_gcini` must complete
- **Actions:**
  - Establishes communication with stackers (SR01-SR22)
  - Manages MUDP/MASPA/CRC protocol
  - Reads/writes message queues
  - Updates mapped memory with stacker state

#### Step 6: Host Communications
**Process:** `p_cc_hostcom.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Handles inbound and outbound host messages
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Processes `MHC_HOST_IN` table
  - Processes `MHC_HOST_OUT` table
  - Communicates with host system (WCS/MES)

**Pause:** Wait 2000ms after starting communication services to allow initialization

---

### Phase 3: Tracking and Arrival Processing

#### Step 7: Tracking Filter
**Process:** `p_tk_trkfl.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Processes barcode scan data
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Reads barcode scan data
  - Filters and validates scans
  - Updates database with scan information

#### Step 8: Arrival Handler
**Process:** `p_ar_arive.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Processes load arrival events
- **Dependencies:** `p_tk_trkfl` should be running
- **Actions:**
  - Monitors for load arrivals
  - Creates move records for inbound loads
  - Updates `MHC_MOVS` table

---

### Phase 4: Dispatchers

#### Step 9: Find Work
**Process:** `p_ar_fndwk.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Finds queued moves and updates to pending
- **Dependencies:** `p_sy_dbini` must complete, communication services running
- **Actions:**
  - Polls `MHC_MOVS` for QUEUED moves
  - Updates moves to PENDING status
  - Wakes up dispatchers

**Pause:** Wait 5000ms after starting Find Work

#### Step 10: Move Dispatcher
**Process:** `p_ar_movdp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Dispatches station-to-station moves
- **Dependencies:** `p_ar_fndwk` should be running
- **Actions:**
  - Processes PENDING station moves
  - Assigns moves to stations/conveyors
  - Updates `MHC_MOVS` status

#### Step 11: Stacker Dispatcher
**Process:** `p_ar_stkdp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Dispatches work to 22 stacker cranes
- **Dependencies:** `p_ar_fndwk` should be running, `p_cc_stkcmx` must be running
- **Actions:**
  - Processes PENDING store/retrieve moves
  - Assigns moves to available stackers
  - Writes commands to message queues
  - Updates `MHC_MOVS` status

#### Step 12: AGV Dispatcher
**Process:** `p_ar_agvdp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Dispatches work to AGVs
- **Dependencies:** `p_ar_fndwk` should be running, `p_cc_mudpcm` must be running
- **Actions:**
  - Processes PENDING AGV moves
  - Assigns moves to available AGVs
  - Updates `MHC_MOVS` status

#### Step 13: RTN Dispatcher
**Process:** `p_ar_rtndp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Dispatches work to RTNX vehicles
- **Dependencies:** `p_ar_fndwk` should be running, `p_cc_mudpcm` must be running
- **Actions:**
  - Processes PENDING RTNX moves
  - Assigns moves to available RTNX vehicles
  - Updates `MHC_MOVS` status

---

### Phase 5: Completers

#### Step 14: Stacker Completer
**Process:** `p_ar_srcmp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Monitors stacker completions
- **Dependencies:** `p_cc_stkcmx` must be running
- **Actions:**
  - Monitors completion messages from stackers
  - Updates `MHC_MOVS`, `MHC_INVENTORY`, `MHC_LOCATION`
  - Updates statistics

#### Step 15: AGV Completer
**Process:** `p_ar_agvcp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Monitors AGV completions
- **Dependencies:** `p_cc_mudpcm` must be running
- **Actions:**
  - Monitors completion messages from AGVs
  - Updates `MHC_MOVS` and inventory

#### Step 16: RTN Completer
**Process:** `p_ar_rtnxcp.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Monitors RTNX completions
- **Dependencies:** `p_cc_mudpcm` must be running
- **Actions:**
  - Monitors completion messages from RTNX vehicles
  - Updates `MHC_MOVS` and inventory

---

### Phase 6: Supporting Services

#### Step 17: Statistics Service
**Process:** `p_sy_stats.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Collects and aggregates statistics
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Reads statistics from mapped memory
  - Writes to `MHC_STATISTICS` table
  - Performs roll-up aggregations

#### Step 18: Email Service
**Process:** `p_sy_emailer.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Sends email notifications
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Monitors error subscriptions
  - Sends email notifications for errors
  - Processes `MHC_EML_EMAIL_QUEUE`

#### Step 19: Error Device Monitor
**Process:** `p_sy_errdev.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Monitors device errors
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Monitors error flags in mapped memory
  - Creates `MHC_ERROR` records
  - Triggers error notifications

#### Step 20: Process Watchdog
**Process:** `p_sy_watch.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Monitors process health
- **Dependencies:** All processes should be running
- **Actions:**
  - Monitors process heartbeats
  - Detects crashed processes
  - Logs process status

---

### Phase 7: Auro-Specific Services

#### Step 21: Auro Host Integration
**Process:** `p_si_AuroHost.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Auro-specific host integration
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Processes Auro-specific host messages
  - Manages Auro host communication protocol

#### Step 22: Auro Tracking Integration
**Process:** `p_si_AuroTrack.exe`
- **Type:** SERVICE (runs continuously)
- **Purpose:** Auro-specific tracking integration
- **Dependencies:** `p_sy_dbini` must complete
- **Actions:**
  - Processes Auro-specific tracking data
  - Integrates with Auro tracking systems

---

### Phase 8: User Interface

#### Step 23: Main Menu Application
**Process:** `mhcMenu.exe`
- **Type:** APPLICATION (user-launched)
- **Purpose:** Operator interface
- **Dependencies:** All services should be running
- **Actions:**
  - Provides UI for operations, inquiries, maintenance
  - Displays real-time system status
  - Allows operator intervention

---

## Startup Script Example

```batch
REM Phase 1: System Initialization
echo Starting system initialization...
p_sy_gcini.exe
if %ERRORLEVEL% NEQ 0 goto ERROR

p_sy_dbini.exe
if %ERRORLEVEL% NEQ 0 goto ERROR

start p_sy_erlog.exe

REM Phase 2: Communication Services
echo Starting communication services...
start Kepware_PLC.exe -PLC01_In
start Kepware_PLC.exe -PLC01_Out
start Kepware_PLC.exe -PLC02
start Kepware_PLC.exe -PLC03
start Kepware_PLC.exe -PLC04
start p_cc_stkcmx.exe
start p_cc_hostcom.exe

timeout /t 2

REM Phase 3: Tracking
start p_tk_trkfl.exe
start p_ar_arive.exe

REM Phase 4: Dispatchers
start p_ar_fndwk.exe
timeout /t 5
start p_ar_movdp.exe
start p_ar_stkdp.exe
start p_ar_agvdp.exe
start p_ar_rtndp.exe

REM Phase 5: Completers
start p_ar_srcmp.exe
start p_ar_agvcp.exe
start p_ar_rtnxcp.exe

REM Phase 6: Supporting Services
start p_sy_stats.exe
start p_sy_emailer.exe
start p_sy_errdev.exe
start p_sy_watch.exe

REM Phase 7: Auro-Specific
start p_si_AuroHost.exe
start p_si_AuroTrack.exe

echo System startup complete!
goto END

:ERROR
echo Startup failed!
exit /b 1

:END
```

---

## Shutdown Sequence

1. Stop accepting new work (set system flag)
2. Complete current moves
3. Stop dispatchers
4. Stop completers
5. Stop communication services
6. Stop supporting services
7. Flush mapped memory to disk
8. Close database connections

---

## Related Documents

- [Architecture Overview](00_Architecture_Overview.md)
- [Component Dependency Map](01_Component_Dependency_Map.md)
- [System Startup/Shutdown Workflow](../05_Workflows/04_System_Startup_Shutdown.md)
- [Background Processes](../03_Code_Reference/01_Background_Processes/)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Process Details | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process Documentation |
| Startup Workflow | [System Startup/Shutdown](../05_Workflows/04_System_Startup_Shutdown.md) | Detailed Steps |
| Configuration | [Configuration Overview](../06_Configuration/00_Configuration_Overview.md) | Startup Config |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Startup Issues |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

