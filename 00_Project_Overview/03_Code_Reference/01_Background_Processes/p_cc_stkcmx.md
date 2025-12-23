# p_cc_stkcmx (Stacker Communications)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Stacker Communications process (`p_cc_stkcmx`) handles low-level communication with the 22 stacker cranes using MUDP, MASPA, or CRC protocols.

---

## Purpose

Manages communication protocol with stacker cranes, sends commands, receives status updates, and updates mapped memory with real-time stacker state.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\p_cc_stkcmx\` or `ccsub\`
- **Executable:** `D:\Auro\Exec\p_cc_stkcmx.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| `cc_stk` | Shared Library | Stacker class for state management |
| Communication Protocol | Protocol Library | MUDP/MASPA/CRC protocol implementation |
| Mapped Memory | Shared Memory | Message queues, stacker state |

---

## Key Functions

### SendCommand
**Purpose:** Sends command to stacker crane
**Protocol:** MUDP, MASPA, or CRC
**Action:** Formats message, sends via communication link

### ReceiveStatus
**Purpose:** Receives status updates from stacker
**Action:** Parses message, updates mapped memory

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_SM2DB_STACKER | UPDATE | Update stacker state (via shared memory) |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Stacker State | R/W | Read/write stacker state |
| Message Queue | R/W | Read commands, write status |

---

## Communication Protocols

- **MUDP:** Multi-User Data Protocol
- **MASPA:** Material Handling System Protocol A
- **CRC:** Custom protocol for stacker cranes

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](p_ar_stkdp.md) | Dispatcher |
| Stacker Completer | [p_ar_srcmp](p_ar_srcmp.md) | Completer |
| IPC | [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md) | Memory Structure |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

