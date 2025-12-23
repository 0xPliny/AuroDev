# ds_stk - Stacker Operations Module

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_stk.cpp` / `ds_stk.h` |
| **Library** | DSUB (Database Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Status** | ✅ Active - Stacker Operations |

---

## Overview

The `ds_stk` module provides business logic functions for stacker crane operations. It handles stacker selection, availability checking, mode management, and statistics tracking.

**Key Responsibilities:**
- Stacker selection for moves
- Availability and mode checking
- Stacker statistics tracking
- Error recovery coordination
- Load assignment to stackers

---

## Key Functions

### ds_stk_select_for_move

**Signature:**
```cpp
long ds_stk_select_for_move(int aisle, int sourceRow, int destRow, STK* stk);
```

**Purpose:** Select best available stacker for a move operation.

| Parameter | Type | Description |
|-----------|------|-------------|
| `aisle` | `int` | Aisle number |
| `sourceRow` | `int` | Source row |
| `destRow` | `int` | Destination row |
| `stk` | `STK*` | Output: Selected stacker |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Stacker selected |
| `GP.EMPTY` | No available stacker |
| `GP.BAD` | Error |

**Logic Flow:**
1. Query available stackers in aisle
2. Filter by mode (can handle move type)
3. Check schedule status (not busy)
4. Select based on proximity
5. Return selected stacker

---

### ds_stk_check_availability

**Signature:**
```cpp
long ds_stk_check_availability(int stackerId, int moveType);
```

**Purpose:** Check if stacker can handle a specific move type.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stackerId` | `int` | Stacker identifier |
| `moveType` | `int` | Type of move (Store/Retrieve) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Stacker available |
| `GP.BAD` | Stacker not available |

**Checks:**
- Stacker enabled
- Communication online
- No errors
- Mode allows move type
- Schedule not busy

---

### ds_stk_update_stats

**Signature:**
```cpp
long ds_stk_update_stats(int stackerId, int moveType, int completionType);
```

**Purpose:** Update stacker statistics after move completion.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stackerId` | `int` | Stacker identifier |
| `moveType` | `int` | Type of move completed |
| `completionType` | `int` | Completion status |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Stats updated |
| `GP.BAD` | Update failed |

---

### ds_stk_set_mode

**Signature:**
```cpp
long ds_stk_set_mode(int stackerId, int mode);
```

**Purpose:** Set stacker operating mode.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stackerId` | `int` | Stacker identifier |
| `mode` | `int` | New mode (GP.SR.MODES) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Mode set |
| `GP.BAD` | Mode change failed |

---

### ds_stk_get_pending_count

**Signature:**
```cpp
long ds_stk_get_pending_count(int stackerId, int* storeCount, int* retrieveCount);
```

**Purpose:** Get count of pending moves for stacker.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stackerId` | `int` | Stacker identifier |
| `storeCount` | `int*` | Output: Pending stores |
| `retrieveCount` | `int*` | Output: Pending retrieves |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Counts retrieved |
| `GP.BAD` | Error |

---

## Common Usage Patterns

### Pattern 1: Select Stacker for Store Move

```cpp
STK stk;

// Select stacker for store from station to aisle
if (ds_stk_select_for_move(aisle, 0, row, &stk) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Selected stacker %d for store", stk.sr_no);
    
    // Assign move to stacker
    move.sr_no = stk.sr_no;
}
```

### Pattern 2: Check Before Dispatching

```cpp
// Verify stacker can accept store
if (ds_stk_check_availability(stackerId, MOVE_STORE) == GP.GOOD) {
    dispatchStoreMove(stackerId, moveId);
} else {
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Stacker %d not available for store", stackerId);
}
```

### Pattern 3: Update Stats on Completion

```cpp
// Move completed successfully
ds_stk_update_stats(stackerId, MOVE_STORE, COMP_NORMAL);

// Log updated stats
int stores, retrieves;
ds_stk_get_pending_count(stackerId, &stores, &retrieves);
cs_log_printf(FILELINE, GP_LOGLVL_4, 
              "Stacker %d: %d stores, %d retrieves pending",
              stackerId, stores, retrieves);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Table | [os_stk.md](../01_OSUB/os_stk.md) | STK database operations |
| Stacker Control | [cc_stk.md](../02_CCSUB/cc_stk.md) | Stacker control class |
| Stacker Constants | [Stacker_Constants.md](../05_ICISDefines/Stacker_Constants.md) | SR enumerations |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

