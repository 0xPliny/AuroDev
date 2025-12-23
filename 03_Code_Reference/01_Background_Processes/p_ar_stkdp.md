# p_ar_stkdp (Stacker Dispatcher)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The Stacker Dispatcher (`p_ar_stkdp`) is responsible for assigning store and retrieve work to the 22 stacker cranes (SR01-SR22) in the Auro System. It monitors stacker availability and assigns moves based on scheduling algorithms.

---

## Purpose

Assigns pending store and retrieve moves to available stacker cranes, manages load balancing across cranes, and coordinates multi-cycle operations.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\area\p_ar_stkdp\p_ar_stkdp.cpp`
- **Executable:** `D:\Auro\Exec\p_ar_stkdp.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| `cc_stk` | Shared Library | Stacker class for accessing stacker state |
| `cc_std` | Shared Library | Station class for station operations |
| `ds_get_locn` | Database Function | Location selection logic |
| `movs.h` | OSUB Header | Move record access |
| `locn.h` | OSUB Header | Location record access |
| `load.h` | OSUB Header | Load record access |
| `invt.h` | OSUB Header | Inventory record access |
| Mapped Memory | Shared Memory | Stacker state, message queues |

---

## Key Functions

### FindAMove
```cpp
bool FindAMove(MOVS *movs)
```
**Purpose:** Finds the next move to assign to a stacker
**Called By:** Main dispatch loop
**Calls:** `FindStore()`, `FindRetrieve()`, database queries

### FindStore
```cpp
bool FindStore(MOVS *movs, long group = 0)
```
**Purpose:** Finds pending store moves for assignment
**Called By:** `FindAMove()`
**Calls:** Database queries, `ds_get_locn()`

### FindRetrieve
```cpp
bool FindRetrieve(MOVS *movs)
```
**Purpose:** Finds pending retrieve moves for assignment
**Called By:** `FindAMove()`
**Calls:** Database queries

### FindWork
```cpp
void FindWork()
```
**Purpose:** Main dispatch loop - checks stackers and assigns work
**Called By:** Main loop
**Calls:** `FindAMove()`, stacker state access

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_MOVS | SELECT, UPDATE | Find pending moves, update status and schedule |
| MHC_LOCATION | SELECT | Verify location availability |
| MHC_LOAD | SELECT | Verify load information |
| MHC_INVENTORY | SELECT | Verify inventory records |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Stacker State | R/W | Read schedule status, write commands |
| Message Queue | W | Write commands to stacker communication |
| Process Table | R/W | Update heartbeat |

---

## Error Handling

- Monitors stacker error flags
- Skips stackers in error state
- Logs dispatch failures
- Updates move status on errors

---

## Configuration

- **CNF Files:** `sort_2_Aisle_Est_Time.cnf` (Auro-specific timing configuration)
- **Elements Table:** Various timing and scheduling parameters
- **Startup:** Started after `p_ar_fndwk` in warmstart sequence

---

## Scheduling Logic

- Checks stackers in rotation (round-robin)
- Considers stacker mode (Normal, Store Only, Retrieve Only)
- Balances workload across available stackers
- Handles multi-cycle operations (MUDP protocol)

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md)
- [Outbound Move Workflow](../../05_Workflows/02_Outbound_Move.md)
- [Process Startup Sequence](../../01_System_Architecture/04_Process_Startup_Sequence.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Completer | [p_ar_srcmp](p_ar_srcmp.md) | Completer Process |
| Stacker Communications | [p_cc_stkcmx](p_cc_stkcmx.md) | Communication Process |
| Move Table | [MHC_MOVS](../../04_Database_Reference/01_Core_Tables/MHC_MOVS.md) | Schema |
| Inbound Move | [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md) | Workflow |
| Outbound Move | [Outbound Move Workflow](../../05_Workflows/02_Outbound_Move.md) | Workflow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

