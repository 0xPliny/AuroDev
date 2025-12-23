# ds_movs - Move Database Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_movs.cpp`, `ds_movs.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Module Name** | `ds_movs` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `ds_movs` module provides **database functions and business logic** for move operations in the MHC system. It builds on top of the OSUB `os_movs` generated functions, adding business logic, validation, and complex queries.

**Critical Operations:**
- Move creation and validation
- Move status management
- Move completion logic
- Move cancellation and error handling
- Move querying with business rules

**Key Principle:** DSUB functions provide **business logic** on top of OSUB's **data access** functions.

---

## Key Functions

### ds_movs_create

**Signature:**
```cpp
long ds_movs_create(MOVS* movs, const char* loadId, const char* fromLoc, 
                    const char* toLoc, const char* moveType, const char* area)
```

**Description:**
Creates a new move record with business logic validation and default values.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `movs` | `MOVS*` | Output - Populated move record structure |
| `loadId` | `const char*` | Load identifier |
| `fromLoc` | `const char*` | Source location |
| `toLoc` | `const char*` | Destination location |
| `moveType` | `const char*` | Move type (STORE, RETRIEVE, TRANSFER) |
| `area` | `const char*` | Area identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - move record created |
| `-1` | `GP.BAD` | Failure - validation error or database error |

**Logic Flow:**
1. Validates input parameters (non-empty, valid format)
2. Initializes `MOVS` structure using `MOVS_init()`
3. Sets default values:
   - `move_status` = "PENDING"
   - `priority` = 0
   - `add_date` = current date/time
   - `add_user` = current user
4. Copies input parameters to structure
5. Validates location existence using `ds_locn_validate()`
6. Inserts record using `MOVS_insert()`
7. Logs operation using `cs_log_printf()`

**Usage Example:**
```cpp
MOVS movs;
if (ds_movs_create(&movs, "L00001", "PD01", "A01-01-01", "STORE", "A") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Created move [%s] for load %s", 
                  movs.move_id, movs.loadid);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create move for load %s", loadId);
}
```

---

### ds_movs_complete

**Signature:**
```cpp
long ds_movs_complete(MOVS* movs, const char* finalLocation)
```

**Description:**
Completes a move record, updating status and final location.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `movs` | `MOVS*` | Move record to complete (must be selected first) |
| `finalLocation` | `const char*` | Final location where load was placed |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - move completed |
| `-1` | `GP.BAD` | Failure - invalid state or database error |

**Logic Flow:**
1. Validates move is in completable state (not already COMPLETE or CANCELLED)
2. Updates `move_status` = "COMPLETE"
3. Updates `next_location` = `finalLocation`
4. Updates `modify_date` = current date/time
5. Updates `modify_user` = current user
6. Updates record using `MOVS_update()`
7. Updates inventory location using `ds_invt_update_location()`
8. Logs completion

**Usage Example:**
```cpp
MOVS movs;
// ... select move record ...
if (ds_movs_complete(&movs, "A01-01-01") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Move [%s] completed at location %s", 
                  movs.move_id, finalLocation);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to complete move [%s]", movs.move_id);
}
```

---

### ds_movs_cancel

**Signature:**
```cpp
long ds_movs_cancel(MOVS* movs, const char* reason)
```

**Description:**
Cancels a move record with reason code.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `movs` | `MOVS*` | Move record to cancel |
| `reason` | `const char*` | Cancellation reason |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - move cancelled |
| `-1` | `GP.BAD` | Failure - invalid state or database error |

**Logic Flow:**
1. Validates move is cancellable (not already COMPLETE)
2. Updates `move_status` = "CANCELLED"
3. Updates `cancel_reason` = `reason`
4. Updates `modify_date` = current date/time
5. Updates record using `MOVS_update()`
6. Logs cancellation

---

### ds_movs_select_pending

**Signature:**
```cpp
long ds_movs_select_pending(MOVS* movs, const char* area, long priority)
```

**Description:**
Selects the next pending move for processing, ordered by priority and creation date.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `movs` | `MOVS*` | Output - Selected move record |
| `area` | `const char*` | Area to search (NULL for all areas) |
| `priority` | `long` | Minimum priority (0 = all priorities) |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - move selected |
| `-1` | `GP.BAD` | Failure - no pending moves or database error |

**Logic Flow:**
1. Builds SQL query for pending moves:
   - `move_status` = "PENDING"
   - Optional `area` filter
   - Optional `priority >=` filter
   - ORDER BY `priority DESC, add_date ASC`
   - LIMIT 1
2. Executes query using `ds_sql_execute()`
3. Fetches result into `MOVS` structure
4. Returns `GP.GOOD` if move found

**Usage Example:**
```cpp
MOVS movs;
if (ds_movs_select_pending(&movs, "A", 0) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Selected pending move [%s] for load %s", 
                  movs.move_id, movs.loadid);
    // Process move...
} else {
    // No pending moves
}
```

---

### ds_movs_update_status

**Signature:**
```cpp
long ds_movs_update_status(const char* moveId, const char* newStatus, const char* reason)
```

**Description:**
Updates move status with optional reason.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `moveId` | `const char*` | Move identifier |
| `newStatus` | `const char*` | New status (PENDING, IN_PROGRESS, COMPLETE, CANCELLED, ERROR) |
| `reason` | `const char*` | Optional reason for status change |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - status updated |
| `-1` | `GP.BAD` | Failure - move not found or invalid status |

**Logic Flow:**
1. Selects move by primary key using `MOVS_select_MOVS_PR_KEY()`
2. Validates status transition is allowed
3. Updates `move_status` = `newStatus`
4. If `reason` provided, updates appropriate reason field
5. Updates `modify_date` = current date/time
6. Updates record using `MOVS_update()`

---

### ds_movs_get_by_load

**Signature:**
```cpp
long ds_movs_get_by_load(MOVS* movs, const char* loadId, const char* status)
```

**Description:**
Gets move record(s) for a specific load, optionally filtered by status.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `movs` | `MOVS*` | Output - Move record (or first matching if multiple) |
| `loadId` | `const char*` | Load identifier |
| `status` | `const char*` | Optional status filter (NULL for all statuses) |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - move found |
| `-1` | `GP.BAD` | Failure - no move found or database error |

**Usage Example:**
```cpp
MOVS movs;
if (ds_movs_get_by_load(&movs, "L00001", "PENDING") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Found pending move [%s] for load %s", 
                  movs.move_id, movs.loadid);
}
```

---

## Common Usage Patterns

### Pattern 1: Create and Dispatch Move

```cpp
MOVS movs;
if (ds_movs_create(&movs, loadId, fromLoc, toLoc, "STORE", area) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Created move [%s] for load %s", 
                  movs.move_id, loadId);
    
    // Update status to IN_PROGRESS when dispatched
    if (ds_movs_update_status(movs.move_id, "IN_PROGRESS", NULL) == GP.GOOD) {
        // Dispatch to equipment...
    }
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create move for load %s", loadId);
    return GP.BAD;
}
```

### Pattern 2: Select and Process Pending Move

```cpp
MOVS movs;
if (ds_movs_select_pending(&movs, area, 0) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Processing move [%s] for load %s", 
                  movs.move_id, movs.loadid);
    
    // Update to IN_PROGRESS
    ds_movs_update_status(movs.move_id, "IN_PROGRESS", NULL);
    
    // Dispatch to equipment...
    // ... equipment processing ...
    
    // Complete move when equipment confirms
    ds_movs_complete(&movs, finalLocation);
} else {
    // No pending moves
}
```

### Pattern 3: Error Handling and Cancellation

```cpp
MOVS movs;
// ... select move ...
if (equipmentError) {
    if (ds_movs_cancel(&movs, "EQUIPMENT_ERROR") == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_ERROR, "Cancelled move [%s] due to equipment error", 
                      movs.move_id);
    }
} else if (validationError) {
    if (ds_movs_cancel(&movs, "VALIDATION_ERROR") == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_WARNING, "Cancelled move [%s] due to validation error", 
                      movs.move_id);
    }
}
```

---

## Move Status Values

| Status | Description | Allowed Transitions |
|--------|-------------|---------------------|
| `PENDING` | Move created, not yet dispatched | → IN_PROGRESS, CANCELLED |
| `IN_PROGRESS` | Move dispatched to equipment | → COMPLETE, ERROR, CANCELLED |
| `COMPLETE` | Move successfully completed | (terminal state) |
| `CANCELLED` | Move cancelled | (terminal state) |
| `ERROR` | Move failed with error | → CANCELLED, IN_PROGRESS (retry) |

---

## Dependencies

### Calls OSUB Functions:
- `MOVS_init()`: Initialize structure
- `MOVS_insert()`: Insert record
- `MOVS_update()`: Update record
- `MOVS_select_MOVS_PR_KEY()`: Select by primary key
- `MOVS_select_MOVS_LOAD_ID()`: Select by load ID

### Calls Other DSUB Functions:
- `ds_locn_validate()`: Validate location exists
- `ds_invt_update_location()`: Update inventory location
- `ds_sql_execute()`: Execute custom SQL queries

### Calls CSUB Functions:
- `cs_log_printf()`: Logging
- `cs_dtm_current()`: Get current date/time

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| OSUB Move Functions | [os_movs.md](../01_OSUB/os_movs.md) | Generated CRUD functions |
| Move Table Structure | [os_movs.md](../01_OSUB/os_movs.md) | Table Structure |
| Database Access Patterns | [Database Patterns](../../02_Coding_Standards/03_Database_Access_Patterns.md) | Patterns |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

