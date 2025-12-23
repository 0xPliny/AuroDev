# p_ar_movdp (Move Dispatcher)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Move Dispatcher (`p_ar_movdp`) handles station-to-station moves and conveyor operations. It processes moves that don't require stacker cranes.

---

## Purpose

Dispatches moves between stations, handles conveyor operations, and manages load transfers at stations.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\p_ar_movdp\p_ar_movdp.cpp`
- **Executable:** `D:\Auro\Exec\p_ar_movdp.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| `cc_std` | Shared Library | Station class for station operations |
| `cc_plc` | Shared Library | PLC class for conveyor control |
| `movs.h` | OSUB Header | Move record access |
| `sttn.h` | OSUB Header | Station record access |
| Mapped Memory | Shared Memory | Station state, PLC state |

---

## Key Functions

### movdp_findAssign
```cpp
void movdp_findAssign()
```
**Purpose:** Finds moves that need location assignment
**Called By:** Main loop
**Calls:** Database queries

### movdp_findLostMovs
```cpp
void movdp_findLostMovs()
```
**Purpose:** Finds cases that need moves (lost move detection)
**Called By:** Main loop
**Calls:** Database queries

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | SELECT, UPDATE | Find pending moves, update status |
| MHC_STATION | SELECT | Station information |
| MHC_INVENTORY | SELECT | Find loads at stations |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Station State | R/W | Read station status, write commands |
| PLC State | R/W | Control conveyors via PLC |

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Outbound Move Workflow](../../05_Workflows/02_Outbound_Move.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Station Table | [MHC_SM2DB_STATION](../../04_Database_Reference/02_Equipment_Tables/MHC_SM2DB_STATION.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

