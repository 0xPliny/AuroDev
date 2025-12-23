# Inbound Move (Store Operation)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the complete workflow for an inbound move (store operation) from host request through storage completion.

---

## Workflow Steps

### Step 1: Host Request
- **Process:** `p_si_AuroHost` (Auro Host Integration)
- **Action:** Receives store request from host system (WCS/MES)
- **Message Format:** Load ID, Item Code, Quantity, Destination Zone
- **Database:** Inserts into `MHC_HOST_IN` table

### Step 2: Create Move Record
- **Process:** `p_si_AuroHost`
- **Action:** Creates `MHC_MOVS` record
- **Status:** QUEUED
- **Fields:** Load_ID, Move_Type='STORE', Current_Location, Next_Location=NULL

### Step 3: Find Work
- **Process:** `p_ar_fndwk` (Find Work)
- **Action:** Polls `MHC_MOVS` for QUEUED moves
- **Update:** Changes status to PENDING
- **Wake:** Wakes up `p_ar_stkdp`

### Step 4: Stacker Dispatcher
- **Process:** `p_ar_stkdp` (Stacker Dispatcher)
- **Action:** 
  - Finds PENDING store moves
  - Selects available stacker (SR01-SR22)
  - Selects target location (via `ds_get_locn`)
  - Updates `MHC_MOVS`: Status=SCHEDULED, Next_Location, Schedule=SR##
  - Writes command to message queue

### Step 5: Stacker Communication
- **Process:** `p_cc_stkcmx` (Stacker Communications)
- **Action:**
  - Reads command from queue
  - Formats MUDP/MASPA/CRC message
  - Sends STORE command to stacker
  - Updates mapped memory: Schedule=BUSY, Function=STORE

### Step 6: Equipment Execution
- **Equipment:** Stacker Crane (SR##)
- **Action:**
  - Moves to pickup location
  - Picks up load
  - Moves to storage location
  - Stores load
  - Sends completion message

### Step 7: Completion Processing
- **Process:** `p_ar_srcmp` (Stacker Completer)
- **Action:**
  - Receives completion message
  - Updates `MHC_MOVS`: Status=COMPLETE, Complete_Date
  - Updates `MHC_INVENTORY`: Location=Next_Location
  - Updates `MHC_LOCATION`: Load_ID, Status=OCCUPIED
  - Updates mapped memory: Schedule=NOT_BUSY
  - Updates statistics: CountStores++

### Step 8: Host Notification
- **Process:** `p_ar_hostcom` (Host Communications)
- **Action:**
  - Monitors completed moves
  - Creates completion message
  - Inserts into `MHC_HOST_OUT` table
  - Sends to host system

---

## Related Documents

- [Workflow Index](00_Workflow_Index.md)
- [Data Flow Diagrams](../01_System_Architecture/02_Data_Flow_Diagrams.md)
- [p_ar_stkdp](../03_Code_Reference/01_Background_Processes/p_ar_stkdp.md)
- [p_ar_srcmp](../03_Code_Reference/01_Background_Processes/p_ar_srcmp.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](../03_Code_Reference/01_Background_Processes/p_ar_stkdp.md) | Process |
| Stacker Completer | [p_ar_srcmp](../03_Code_Reference/01_Background_Processes/p_ar_srcmp.md) | Process |
| Move Table | [MHC_MOVS](../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Data Flow | [Data Flow Diagrams](../01_System_Architecture/02_Data_Flow_Diagrams.md) | Inbound Flow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

