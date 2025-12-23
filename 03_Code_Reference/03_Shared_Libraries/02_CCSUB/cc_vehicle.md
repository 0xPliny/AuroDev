# cc_vehicle - Vehicle Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_vehicle.cpp`, `cc_vehicle.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_vehicle` |
| **Controller Class** | `cc_vehicle_ctrl` (manages all vehicles) |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_vehicle` module provides **AGV and RTNX vehicle control and status management**. Each vehicle has an instance that manages communication, pathing, scheduling, and multi-cycle operations.

**Critical Operations:**
- Vehicle status monitoring
- Schedule management
- Path and point tracking
- Multi-cycle operation support (MUDP)
- Error detection and recovery

**Vehicle Types:**
- AGV (Automatic Guided Vehicle)
- RTNX (Rail-guided vehicle)

**Relationship to Database:**
- Database state stored in vehicle-specific tables
- Real-time state in mapped memory (accessed via `cc_vehicle` class)
- Database and memory are synchronized

---

## Key Classes

### cc_vehicle

**Purpose:** Individual vehicle instance

**Key Methods:**

#### Status Access Methods

```cpp
long GetSchedule();              // Get schedule status
long GetFunction();              // Get current function
long GetControlFunction();       // Get control function
long GetComm();                  // Get communication status
long GetError();                 // Get error flag
long GetErrorCode();             // Get error code
long GetCurrentPoint();          // Get current point number
long GetNextPoint();             // Get next point number
long GetFinalPoint();            // Get final destination point
char* GetVehicleName();          // Get vehicle name (e.g., "AGV01")
long GetEnroute();               // Get current enroute count
long GetMaxEnroute();            // Get maximum enroute allowed
long GetRouteStatus();           // Get route status
long GetMode();                  // Get operational mode
```

#### Status Set Methods

```cpp
long SetSchedule(long schedule);        // Set schedule status
long SetFunction(long func);            // Set function
long SetControlFunction(long func_ctrl); // Set control function
long SetCurrentPoint(long point);       // Set current point
long SetNextPoint(long point);          // Set next point
long SetFinalPoint(long point);         // Set final destination
long SetRouteStatus(long status);       // Set route status
long SetMode(long mode);                // Set operational mode
```

#### Multi-Cycle Methods (MUDP)

```cpp
long GetCurrentStep();                  // Get current step (1-4)
long GetSubFunction();                  // Get sub-function
long GetNeedNextStep();                 // Get need next step flag
long GetNeedSameStep();                 // Get need same step flag
long SetCurrentStep(long step);         // Set current step
long SetSubFunction(long subfunc);      // Set sub-function
long SetNeedNextStep(long flag);        // Set need next step
long SetNeedSameStep(long flag);        // Set need same step
```

#### Initialization and Control

```cpp
long Initialize(const char* vehicleName); // Initialize vehicle
long Commit();                            // Commit changes to mapped memory
long Refresh();                           // Refresh from mapped memory
```

---

### cc_vehicle_ctrl

**Purpose:** Controller class that manages all vehicle instances

**Key Methods:**

```cpp
cc_vehicle* Find_Vehicle(const char* vehicleName);  // Find vehicle by name
long GetCount();                                     // Get total vehicle count
```

**Usage:**
```cpp
cc_vehicle* AGV = cc_vehicle_ctrl.Find_Vehicle("AGV01");
if (AGV) {
    if (AGV->GetSchedule() == GP.SCHED.NOT_BUSY) {
        // Vehicle available
    }
}
```

---

## Common Usage Patterns

### Pattern 1: Check Vehicle Availability

```cpp
cc_vehicle* AGV = cc_vehicle_ctrl.Find_Vehicle("AGV01");
if (AGV == NULL) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Vehicle AGV01 not found");
    return GP.BAD;
}

if (AGV->GetComm() != GP.COM.ONLI) {
    return GP.BAD;  // Offline
}

if (AGV->GetSchedule() != GP.SCHED.NOT_BUSY) {
    return GP.BAD;  // Busy
}

if (AGV->GetError() == GP_TRUE) {
    return GP.BAD;  // Error
}

if (AGV->GetEnroute() >= AGV->GetMaxEnroute()) {
    return GP.BAD;  // At capacity
}

// Vehicle is available
return GP.GOOD;
```

### Pattern 2: Assign Multi-Cycle Move

```cpp
SQL.start(SQL.READ_WRITE);

cc_vehicle* AGV = cc_vehicle_ctrl.Find_Vehicle("AGV01");
if (AGV == NULL) return GP.BAD;

// Set schedule
AGV->SetSchedule(GP.SCHED.BUSY);

// Set function and step for multi-cycle
AGV->SetFunction(GP.FUNC.MOVE);
AGV->SetCurrentStep(1);  // Step 1: Pick up at station
AGV->SetSubFunction(GP.SUBFUNC.PICK);

// Set points
AGV->SetCurrentPoint(currentPoint);
AGV->SetNextPoint(nextPoint);
AGV->SetFinalPoint(finalPoint);

// Commit
if (AGV->Commit() == GP.GOOD) {
    SQL.commit();
    return GP.GOOD;
} else {
    SQL.rollback();
    return GP.BAD;
}
```

### Pattern 3: Handle Multi-Cycle Step Progression

```cpp
cc_vehicle* AGV = cc_vehicle_ctrl.Find_Vehicle("AGV01");
if (AGV == NULL) return GP.BAD;

// Check if need to proceed to next step
if (AGV->GetNeedNextStep() == GP_TRUE) {
    long currentStep = AGV->GetCurrentStep();
    if (currentStep < 4) {
        // Move to next step
        AGV->SetCurrentStep(currentStep + 1);
        AGV->SetNeedNextStep(GP_FALSE);
        AGV->Commit();
        return GP.GOOD;
    } else {
        // Cycle complete
        AGV->SetSchedule(GP.SCHED.NOT_BUSY);
        AGV->Commit();
        return GP.GOOD;
    }
}

// Check if need to repeat current step
if (AGV->GetNeedSameStep() == GP_TRUE) {
    // Repeat current step (error recovery)
    AGV->SetNeedSameStep(GP_FALSE);
    AGV->Commit();
    return GP.GOOD;
}
```

---

## Related Tables

| Table | Relationship | Description |
|-------|--------------|-------------|
| `MHC_MOVS` | Reference | Moves assigned to vehicle |
| Points Memory | Synchronized | Vehicle pathing points |

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| MUDP Protocol | [cc_mudp.md](cc_mudp.md) | Vehicle Communication |
| Vehicle Dispatcher | [p_ar_agvdp](../01_Background_Processes/p_ar_agvdp.md) | Uses cc_vehicle |
| Vehicle Communications | [p_cc_mudpcm](../01_Background_Processes/p_cc_mudpcm.md) | Updates cc_vehicle |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

