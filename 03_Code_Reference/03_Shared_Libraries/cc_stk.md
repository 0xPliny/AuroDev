# cc_stk (Stacker Classes)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

The `cc_stk` library provides C++ classes for accessing and controlling stacker crane state and operations.

---

## Purpose

Provides object-oriented interface to stacker crane state in mapped memory, including schedule status, function, error flags, and commands.

---

## Location

- **Header:** `csub\cc_stk.h`
- **Source:** `csub\cc_stk.cpp`

---

## Key Classes

### cc_stacker
**Purpose:** Stacker crane class
**Methods:**
- `GetSchedule()` - Get schedule status
- `SetSchedule()` - Set schedule status
- `GetFunction()` - Get current function
- `SetFunction()` - Set function
- `GetError()` - Get error flag
- `GetErrorCode()` - Get error code

---

## Usage

```cpp
#include <cc_stk.h>

cc_stacker stacker;
stacker.Initialize(aisle_number);
if (stacker.GetSchedule() == NOT_BUSY) {
    stacker.SetSchedule(BUSY);
    stacker.SetFunction(STORE);
    stacker.Commit();
}
```

---

## Related Documents

- [Shared Libraries Index](00_Shared_Libraries_Index.md)
- [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](../01_Background_Processes/p_ar_stkdp.md) | Process |
| Stacker Communications | [p_cc_stkcmx](../01_Background_Processes/p_cc_stkcmx.md) | Process |
| IPC | [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md) | Memory Structure |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

