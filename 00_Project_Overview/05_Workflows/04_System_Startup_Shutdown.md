# System Startup/Shutdown

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the system startup and shutdown procedures for the Auro System.

---

## Startup Sequence

See [Process Startup Sequence](../01_System_Architecture/04_Process_Startup_Sequence.md) for detailed startup order.

### Summary
1. System Initialization (p_sy_gcini, p_sy_dbini)
2. Communication Services (Kepware_PLC, p_cc_stkcmx)
3. Tracking and Arrival (p_tk_trkfl, p_ar_arive)
4. Dispatchers (p_ar_fndwk, p_ar_stkdp, etc.)
5. Completers (p_ar_srcmp, p_ar_agvcp, etc.)
6. Supporting Services (p_sy_stats, p_sy_emailer, etc.)
7. Auro-Specific Services (p_si_AuroHost, p_si_AuroTrack)
8. User Interface (mhcMenu.exe)

---

## Shutdown Sequence

### Step 1: Stop Accepting New Work
- Set system flag to prevent new move creation
- Complete current moves in progress

### Step 2: Stop Dispatchers
- Stop `p_ar_fndwk`
- Stop `p_ar_stkdp`, `p_ar_movdp`, `p_ar_agvdp`, `p_ar_rtndp`

### Step 3: Stop Completers
- Stop `p_ar_srcmp`, `p_ar_agvcp`, `p_ar_rtnxcp`

### Step 4: Stop Communication Services
- Stop `p_cc_stkcmx`, `p_cc_mudpcm`
- Stop `Kepware_PLC` instances

### Step 5: Stop Supporting Services
- Stop `p_sy_stats`, `p_sy_emailer`, `p_sy_erlog`
- Stop `p_sy_errdev`, `p_sy_watch`

### Step 6: Flush Mapped Memory
- Run `p_sy_shmop` to flush shared memory to disk
- Ensure all data is persisted

### Step 7: Close Database Connections
- Close all database connections
- Ensure transactions are committed

---

## Related Documents

- [Process Startup Sequence](../01_System_Architecture/04_Process_Startup_Sequence.md)
- [Workflow Index](00_Workflow_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Startup Details | [Process Startup Sequence](../01_System_Architecture/04_Process_Startup_Sequence.md) | Detailed Steps |
| IPC | [IPC and Shared Memory](../01_System_Architecture/05_IPC_and_Shared_Memory.md) | Memory Flush |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

