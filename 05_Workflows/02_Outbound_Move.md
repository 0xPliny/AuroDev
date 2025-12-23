# Outbound Move (Retrieve Operation)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the complete workflow for an outbound move (retrieve operation) from host request through load delivery.

---

## Workflow Steps

### Step 1: Host Request
- **Process:** `p_si_AuroHost`
- **Action:** Receives retrieve request from host system
- **Message Format:** Load ID, Destination Station
- **Database:** Inserts into `MHC_HOST_IN` table

### Step 2: Find Load Location
- **Process:** `p_si_AuroHost`
- **Action:** Queries `MHC_INVENTORY` to find load location
- **Database:** SELECT Location FROM MHC_INVENTORY WHERE Load_ID = ?

### Step 3: Create Move Record
- **Process:** `p_si_AuroHost`
- **Action:** Creates `MHC_MOVS` record
- **Status:** QUEUED
- **Fields:** Load_ID, Move_Type='RETRIEVE', Current_Location, Next_Location=Station

### Step 4: Find Work
- **Process:** `p_ar_fndwk`
- **Action:** Polls `MHC_MOVS` for QUEUED moves
- **Update:** Changes status to PENDING
- **Wake:** Wakes up `p_ar_stkdp`

### Step 5: Stacker Dispatcher
- **Process:** `p_ar_stkdp`
- **Action:**
  - Finds PENDING retrieve moves
  - Selects available stacker
  - Updates `MHC_MOVS`: Status=SCHEDULED, Schedule=SR##
  - Writes command to message queue

### Step 6: Stacker Communication
- **Process:** `p_cc_stkcmx`
- **Action:**
  - Reads command from queue
  - Sends RETRIEVE command to stacker
  - Updates mapped memory: Schedule=BUSY, Function=RETRIEVE

### Step 7: Equipment Execution
- **Equipment:** Stacker Crane (SR##)
- **Action:**
  - Moves to storage location
  - Retrieves load
  - Moves to drop-off location (station)
  - Drops load
  - Sends completion message

### Step 8: Completion Processing
- **Process:** `p_ar_srcmp`
- **Action:**
  - Receives completion message
  - Updates `MHC_MOVS`: Status=COMPLETE
  - Updates `MHC_INVENTORY`: Location=Station
  - Updates `MHC_LOCATION`: Load_ID=NULL, Status=AVAILABLE
  - Updates mapped memory: Schedule=NOT_BUSY
  - Updates statistics: CountRetrieves++

### Step 9: Station Move (if needed)
- **Process:** `p_ar_movdp` (Move Dispatcher)
- **Action:**
  - Processes station-to-station moves
  - Activates conveyor to move load to destination
  - Updates `MHC_MOVS` status

### Step 10: Host Notification
- **Process:** `p_ar_hostcom`
- **Action:**
  - Monitors completed moves
  - Creates completion message
  - Sends to host system

---

## Related Documents

- [Workflow Index](00_Workflow_Index.md)
- [Data Flow Diagrams](../01_System_Architecture/02_Data_Flow_Diagrams.md)
- [p_ar_stkdp](../03_Code_Reference/01_Background_Processes/p_ar_stkdp.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](../03_Code_Reference/01_Background_Processes/p_ar_stkdp.md) | Process |
| Move Dispatcher | [p_ar_movdp](../03_Code_Reference/01_Background_Processes/p_ar_movdp.md) | Process |
| Data Flow | [Data Flow Diagrams](../01_System_Architecture/02_Data_Flow_Diagrams.md) | Outbound Flow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

