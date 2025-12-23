# ds_errors - Error Handling Module

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_errors.cpp` / `ds_errors.h` |
| **Library** | DSUB (Database Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Status** | ✅ Active - Error Management |

---

## Overview

The `ds_errors` module provides error management functions for the MHC/MEM system. It handles error recording, history tracking, error clearing, and error notifications.

**Key Responsibilities:**
- Record errors to database
- Track error history
- Clear errors on resolution
- Generate error notifications
- Error statistics tracking

---

## Key Functions

### ds_errors_create

**Signature:**
```cpp
long ds_errors_create(int deviceType, int deviceId, int errorCode, const char* description);
```

**Purpose:** Create a new error record.

| Parameter | Type | Description |
|-----------|------|-------------|
| `deviceType` | `int` | Device type (GP.DEV) |
| `deviceId` | `int` | Device identifier |
| `errorCode` | `int` | Error code |
| `description` | `const char*` | Error description |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Error created |
| `GP.BAD` | Creation failed |

**Logic Flow:**
1. Create error record in MHC_ERRORS
2. Log error to system log
3. Update device error status
4. Trigger error notification

---

### ds_errors_clear

**Signature:**
```cpp
long ds_errors_clear(int deviceType, int deviceId, int errorCode, const char* clearedBy);
```

**Purpose:** Clear an active error.

| Parameter | Type | Description |
|-----------|------|-------------|
| `deviceType` | `int` | Device type |
| `deviceId` | `int` | Device identifier |
| `errorCode` | `int` | Error code (0 for all) |
| `clearedBy` | `const char*` | User/process clearing error |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Error cleared |
| `GP.EMPTY` | Error not found |
| `GP.BAD` | Clear failed |

**Logic Flow:**
1. Find error record
2. Move to error history
3. Update cleared timestamp
4. Clear device error status

---

### ds_errors_get_active

**Signature:**
```cpp
long ds_errors_get_active(int deviceType, int deviceId, ERRORS* errors, int* count);
```

**Purpose:** Get active errors for a device.

| Parameter | Type | Description |
|-----------|------|-------------|
| `deviceType` | `int` | Device type |
| `deviceId` | `int` | Device identifier (0 for all) |
| `errors` | `ERRORS*` | Output: Array of errors |
| `count` | `int*` | In/Out: Array size / count returned |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Errors retrieved |
| `GP.EMPTY` | No active errors |
| `GP.BAD` | Error |

---

### ds_errors_get_history

**Signature:**
```cpp
long ds_errors_get_history(int deviceType, int deviceId, 
                           const char* startDate, const char* endDate,
                           ERRHIST* history, int* count);
```

**Purpose:** Get error history for a device.

| Parameter | Type | Description |
|-----------|------|-------------|
| `deviceType` | `int` | Device type |
| `deviceId` | `int` | Device identifier |
| `startDate` | `const char*` | Start date filter |
| `endDate` | `const char*` | End date filter |
| `history` | `ERRHIST*` | Output: Array of history records |
| `count` | `int*` | In/Out: Array size / count returned |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | History retrieved |
| `GP.EMPTY` | No history found |
| `GP.BAD` | Error |

---

### ds_errors_count_by_device

**Signature:**
```cpp
long ds_errors_count_by_device(int deviceType, int deviceId, int* count);
```

**Purpose:** Count active errors for a device.

| Parameter | Type | Description |
|-----------|------|-------------|
| `deviceType` | `int` | Device type |
| `deviceId` | `int` | Device identifier |
| `count` | `int*` | Output: Error count |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Count retrieved |
| `GP.BAD` | Error |

---

## Common Usage Patterns

### Pattern 1: Create Error on Failure

```cpp
// Communication failure detected
ds_errors_create(GP.DEV.STACKER, stackerId, ERR_COMM_TIMEOUT,
                 "Communication timeout - no response from stacker");

cs_log_printf(FILELINE, GP_LOGLVL_1, 
              "Stacker %d: Communication error recorded", stackerId);
```

### Pattern 2: Clear Error on Recovery

```cpp
// Error condition resolved
if (ds_errors_clear(GP.DEV.STACKER, stackerId, ERR_COMM_TIMEOUT, "SYSTEM") == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Stacker %d: Communication error cleared", stackerId);
}
```

### Pattern 3: Check for Active Errors

```cpp
ERRORS errors[10];
int count = 10;

if (ds_errors_get_active(GP.DEV.STACKER, stackerId, errors, &count) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Stacker %d has %d active errors", stackerId, count);
    
    for (int i = 0; i < count; i++) {
        cs_log_printf(FILELINE, GP_LOGLVL_2, 
                      "  Error %d: %s", errors[i].error_code, errors[i].description);
    }
}
```

### Pattern 4: Generate Error Report

```cpp
ERRHIST history[100];
int count = 100;

// Get last 24 hours of errors
if (ds_errors_get_history(0, 0, startDate, endDate, history, &count) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Error history: %d records found", count);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Error Table | [os_errors.md](../01_OSUB/os_errors.md) | ERRORS database operations |
| Error History | [os_errhist.md](../01_OSUB/os_errhist.md) | ERRHIST database operations |
| Error Utilities | [cs_err.md](../03_CSUB/cs_err.md) | Error handling utilities |
| Error Codes | [Error_Codes.md](../05_ICISDefines/Error_Codes.md) | Station error codes |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

