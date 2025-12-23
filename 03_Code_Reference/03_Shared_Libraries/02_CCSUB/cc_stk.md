# cc_stk - Stacker Crane Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_stk.cpp`, `cc_stk.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_stacker` |
| **Controller Class** | `cc_stk_ctrl` (manages all stackers) |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_stk` module provides **stacker crane control and status management** through the `cc_stacker` class. Each stacker crane (SR01-SR22 in Auro system) has an instance that manages communication, scheduling, functions, errors, and position tracking.

**Critical Operations:**
- Stacker status monitoring
- Schedule management (assigning work)
- Function control (STORE, RETRIEVE, etc.)
- Error detection and recovery
- Position tracking (bay, level)
- Multi-fork cycle management

**Relationship to Database:**
- Database state stored in `MHC_STK` table (see `os_stk.md`)
- Real-time state in mapped memory (accessed via `cc_stacker` class)
- Database and memory are synchronized

---

## Key Classes

### cc_stacker

**Purpose:** Individual stacker crane instance

**Key Methods:**

#### Status Access Methods

```cpp
long GetSchedule();              // Get schedule status (NOT_BUSY, BUSY, etc.)
long GetFunction();              // Get current function (STORE, RETRIEVE, etc.)
long GetControlFunction();       // Get control function (START, STOP, etc.)
long GetComm();                  // Get communication status (ONLINE, OFFLINE)
long GetHard();                  // Get hardware status
long GetError();                 // Get error flag (TRUE/FALSE)
long GetErrorCode();             // Get error code number
char* GetErrorXlat();            // Get error translation text
long GetBay();                   // Get current bay number
long GetLevel();                 // Get current level number
long GetAisle();                 // Get aisle number
char* GetDeviceName();           // Get device name (e.g., "SR01")
long GetEnroute();               // Get current enroute count
long GetMaxEnroute();            // Get maximum enroute allowed
long GetLoaded();                // Get loaded flag
char* GetLoadId();               // Get current load ID
```

#### Status Set Methods

```cpp
long SetSchedule(long schedule);        // Set schedule status
long SetFunction(long func);            // Set function
long SetControlFunction(long func_ctrl); // Set control function
long SetError(long error_no);           // Set error code
long SetErrorXlat(const char* xlat);    // Set error translation
long SetBay(long bay);                  // Set bay number
long SetLevel(long level);              // Set level number
long SetLoadId(const char* loadid);     // Set load ID
```

#### Fork Cycle Methods (Multi-Fork Stackers)

```cpp
bool GetForkCycleComp();                // Get fork cycle complete flag
long GetForkStepCurr();                 // Get current fork step
long GetForkStepMax();                  // Get maximum fork steps
long GetForkNeedNextStep();            // Get need next step flag
long GetForkNeedSameStep();            // Get need same step flag
long SetForkCycleComp(bool comp);      // Set fork cycle complete
long SetForkStepCurr(long step);        // Set current fork step
long SetForkNeedNextStep(long flag);   // Set need next step
long SetForkNeedSameStep(long flag);   // Set need same step
```

#### Initialization and Control

```cpp
long Initialize(long aisle);            // Initialize stacker for aisle
long Commit();                          // Commit changes to mapped memory
long Refresh();                         // Refresh from mapped memory
```

---

### cc_stk_ctrl

**Purpose:** Controller class that manages all stacker instances

**Key Methods:**

```cpp
cc_stacker* Find_Stk(const char* devicename);  // Find stacker by name
cc_stacker* Find_Stk(long aisle);              // Find stacker by aisle
long GetCount();                                // Get total stacker count
```

**Usage:**
```cpp
cc_stacker* SR = cc_stk_ctrl.Find_Stk("SR01");
if (SR) {
    if (SR->GetSchedule() == GP.SCHED.NOT_BUSY) {
        // Stacker available
    }
}
```

---

## Common Usage Patterns

### Pattern 1: Check Stacker Availability

```cpp
cc_stacker* SR = cc_stk_ctrl.Find_Stk("SR01");
if (SR == NULL) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Stacker SR01 not found");
    return GP.BAD;
}

if (SR->GetComm() != GP.COM.ONLI) {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "Stacker SR01 is offline");
    return GP.BAD;
}

if (SR->GetSchedule() != GP.SCHED.NOT_BUSY) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Stacker SR01 is busy");
    return GP.BAD;
}

if (SR->GetError() == GP_TRUE) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Stacker SR01 has error: %s", 
                  SR->GetErrorXlat());
    return GP.BAD;
}

// Stacker is available
return GP.GOOD;
```

### Pattern 2: Assign Work to Stacker

```cpp
SQL.start(SQL.READ_WRITE);

cc_stacker* SR = cc_stk_ctrl.Find_Stk("SR01");
if (SR == NULL) return GP.BAD;

// Check availability
if (SR->GetSchedule() != GP.SCHED.NOT_BUSY) {
    return GP.BAD;
}

// Set schedule to busy
SR->SetSchedule(GP.SCHED.BUSY);

// Set function
SR->SetFunction(GP.FUNC.STOR);

// Set load and locations
SR->SetLoadId(loadId);
SR->SetBay(targetBay);
SR->SetLevel(targetLevel);

// Commit to mapped memory
if (SR->Commit() == GP.GOOD) {
    SQL.commit();
    cs_log_printf(FILELINE, CS_LOG_INFO, "Assigned work to SR01: %s", loadId);
    return GP.GOOD;
} else {
    SQL.rollback();
    return GP.BAD;
}
```

### Pattern 3: Check Enroute Capacity

```cpp
cc_stacker* SR = cc_stk_ctrl.Find_Stk("SR01");
if (SR == NULL) return GP.BAD;

if (SR->GetEnroute() < SR->GetMaxEnroute()) {
    // Stacker can accept more moves
    return GP.GOOD;
} else {
    // Stacker at capacity
    cs_log_printf(FILELINE, CS_LOG_INFO, "SR01 at capacity: %ld/%ld", 
                  SR->GetEnroute(), SR->GetMaxEnroute());
    return GP.BAD;
}
```

### Pattern 4: Handle Multi-Fork Cycle

```cpp
cc_stacker* SR = cc_stk_ctrl.Find_Stk("SR01");
if (SR == NULL) return GP.BAD;

// Check if fork cycle is complete
if (SR->GetForkCycleComp() == GP_TRUE) {
    // Cycle complete, can proceed to next step
    SR->SetForkNeedNextStep(GP_TRUE);
    SR->Commit();
    return GP.GOOD;
}

// Check if need to repeat current step
if (SR->GetForkNeedSameStep() == GP_TRUE) {
    // Repeat current step
    SR->SetForkStepCurr(SR->GetForkStepCurr());
    SR->SetForkNeedSameStep(GP_FALSE);
    SR->Commit();
    return GP.GOOD;
}
```

---

## Schedule Status Values

| Status | Constant | Description |
|--------|----------|-------------|
| `NOT_BUSY` | `GP.SCHED.NOT_BUSY` | Stacker is idle and available |
| `BUSY` | `GP.SCHED.BUSY` | Stacker is executing a move |
| `QUEUED` | `GP.SCHED.QUEUED` | Work is queued for stacker |
| `ERROR` | `GP.SCHED.ERROR` | Stacker is in error state |

---

## Function Values

| Function | Constant | Description |
|----------|----------|-------------|
| `STOR` | `GP.FUNC.STOR` | Store operation |
| `RTRV` | `GP.FUNC.RTRV` | Retrieve operation |
| `IDLE` | `GP.FUNC.IDLE` | Idle (no operation) |
| `HOME` | `GP.FUNC.HOME` | Return to home position |

---

## Control Function Values

| Control Function | Constant | Description |
|------------------|----------|-------------|
| `START` | `GP.FUNC_CTRL.START` | Start operation |
| `STOP` | `GP.FUNC_CTRL.STOP` | Stop operation |
| `ERROR_RECOVERY` | `GP.FUNC_CTRL.ERROR_RECOVERY` | Error recovery mode |

---

## Related Tables

| Table | Relationship | Description |
|-------|--------------|-------------|
| `MHC_STK` | Synchronized | Database state (see `os_stk.md`) |
| `MHC_MOVS` | Reference | Moves assigned to stacker |
| `MHC_LOCATION` | Reference | Storage locations served by stacker |

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Operations | [os_stk.md](../01_OSUB/os_stk.md) | MHC_STK table |
| Stacker Dispatcher | [p_ar_stkdp](../01_Background_Processes/p_ar_movdp.md) | Uses cc_stk |
| Stacker Communications | [p_cc_stkcmx](../01_Background_Processes/p_cc_stkcmx.md) | Updates cc_stk |
| Mapped Memory | [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md) | Stacker Memory |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

