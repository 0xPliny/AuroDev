# ds_load - Load Database Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_load.cpp`, `ds_load.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Module Name** | `ds_load` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `ds_load` module provides **database functions and business logic** for load operations in the MHC system. It builds on top of the OSUB `os_load` generated functions, adding business logic, validation, and complex queries.

**Critical Operations:**
- Load creation and validation
- Load status management
- Load location tracking
- Load-inventory relationship management
- Load querying with business rules

**Key Principle:** DSUB functions provide **business logic** on top of OSUB's **data access** functions.

---

## Key Functions

### ds_load_create

**Signature:**
```cpp
long ds_load_create(LOAD* load, const char* loadId, const char* loadType, 
                    const char* area, const char* currentLocation)
```

**Description:**
Creates a new load record with business logic validation and default values.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `load` | `LOAD*` | Output - Populated load record structure |
| `loadId` | `const char*` | Load identifier (must be unique) |
| `loadType` | `const char*` | Load type code |
| `area` | `const char*` | Area identifier |
| `currentLocation` | `const char*` | Initial location of load |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - load record created |
| `-1` | `GP.BAD` | Failure - validation error, duplicate load ID, or database error |

**Logic Flow:**
1. Validates input parameters (non-empty, valid format)
2. Checks if load ID already exists using `LOAD_select_LOAD_PR_KEY()`
3. If duplicate, returns `GP.BAD`
4. Initializes `LOAD` structure using `LOAD_init()`
5. Sets default values:
   - `load_status` = "ACTIVE"
   - `add_date` = current date/time
   - `add_user` = current user
6. Copies input parameters to structure
7. Validates location existence using `ds_locn_validate()`
8. Validates load type using `ds_loadtype_validate()`
9. Inserts record using `LOAD_insert()`
10. Logs operation using `cs_log_printf()`

**Usage Example:**
```cpp
LOAD load;
if (ds_load_create(&load, "L00001", "PALLET", "A", "PD01") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Created load %s at location %s", 
                  load.loadid, load.current_location);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create load %s", loadId);
}
```

---

### ds_load_update_location

**Signature:**
```cpp
long ds_load_update_location(const char* loadId, const char* newLocation)
```

**Description:**
Updates the current location of a load and validates the location change.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `loadId` | `const char*` | Load identifier |
| `newLocation` | `const char*` | New location for load |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - location updated |
| `-1` | `GP.BAD` | Failure - load not found, invalid location, or database error |

**Logic Flow:**
1. Selects load by primary key using `LOAD_select_LOAD_PR_KEY()`
2. Validates new location exists using `ds_locn_validate()`
3. Updates `current_location` = `newLocation`
4. Updates `modify_date` = current date/time
5. Updates `modify_user` = current user
6. Updates record using `LOAD_update()`
7. Updates related inventory location if applicable
8. Logs operation

**Usage Example:**
```cpp
if (ds_load_update_location("L00001", "A01-01-01") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Updated load %s location to %s", 
                  "L00001", "A01-01-01");
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to update load location");
}
```

---

### ds_load_get_by_location

**Signature:**
```cpp
long ds_load_get_by_location(LOAD* load, const char* location, const char* status)
```

**Description:**
Gets load record(s) at a specific location, optionally filtered by status.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `load` | `LOAD*` | Output - Load record (or first matching if multiple) |
| `location` | `const char*` | Location to search |
| `status` | `const char*` | Optional status filter (NULL for all statuses) |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - load found |
| `-1` | `GP.BAD` | Failure - no load found or database error |

**Usage Example:**
```cpp
LOAD load;
if (ds_load_get_by_location(&load, "A01-01-01", "ACTIVE") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Found active load %s at location %s", 
                  load.loadid, load.current_location);
}
```

---

### ds_load_set_status

**Signature:**
```cpp
long ds_load_set_status(const char* loadId, const char* newStatus, const char* reason)
```

**Description:**
Updates load status with optional reason.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `loadId` | `const char*` | Load identifier |
| `newStatus` | `const char*` | New status (ACTIVE, IN_TRANSIT, STORED, RETRIEVED, ERROR) |
| `reason` | `const char*` | Optional reason for status change |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - status updated |
| `-1` | `GP.BAD` | Failure - load not found or invalid status |

**Logic Flow:**
1. Selects load by primary key using `LOAD_select_LOAD_PR_KEY()`
2. Validates status transition is allowed
3. Updates `load_status` = `newStatus`
4. If `reason` provided, updates status reason field
5. Updates `modify_date` = current date/time
6. Updates record using `LOAD_update()`

---

### ds_load_get_with_inventory

**Signature:**
```cpp
long ds_load_get_with_inventory(LOAD* load, INVT* invt, const char* loadId)
```

**Description:**
Gets load record and associated inventory record in a single operation.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `load` | `LOAD*` | Output - Load record |
| `invt` | `INVT*` | Output - Inventory record |
| `loadId` | `const char*` | Load identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - both records found |
| `-1` | `GP.BAD` | Failure - load not found or inventory not found |

**Logic Flow:**
1. Selects load by primary key using `LOAD_select_LOAD_PR_KEY()`
2. Selects inventory by load ID using `INVT_select_INVT_LOAD_ID()`
3. Returns both records

**Usage Example:**
```cpp
LOAD load;
INVT invt;
if (ds_load_get_with_inventory(&load, &invt, "L00001") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Load %s: location=%s, item=%s, qty=%ld", 
                  load.loadid, load.current_location, invt.item_id, invt.quantity);
}
```

---

## Common Usage Patterns

### Pattern 1: Create Load and Associate Inventory

```cpp
LOAD load;
if (ds_load_create(&load, loadId, "PALLET", area, initialLocation) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Created load %s", loadId);
    
    // Create inventory record for load
    INVT invt;
    INVT_init(&invt);
    cc_str.copy(invt.loadid, sizeof(invt.loadid), loadId);
    cc_str.copy(invt.item_id, sizeof(invt.item_id), itemId);
    invt.quantity = quantity;
    
    if (INVT_insert(&invt) == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_INFO, "Associated inventory with load %s", loadId);
    }
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create load %s", loadId);
    return GP.BAD;
}
```

### Pattern 2: Update Load Location After Move

```cpp
// After move completes
if (ds_load_update_location(loadId, finalLocation) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Load %s moved to %s", loadId, finalLocation);
    
    // Update load status
    ds_load_set_status(loadId, "STORED", NULL);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to update load location");
}
```

### Pattern 3: Get Load with Full Details

```cpp
LOAD load;
INVT invt;

if (ds_load_get_with_inventory(&load, &invt, loadId) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, 
                  "Load %s: status=%s, location=%s, item=%s, qty=%ld", 
                  load.loadid, load.load_status, load.current_location, 
                  invt.item_id, invt.quantity);
} else {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "Load %s not found", loadId);
}
```

---

## Load Status Values

| Status | Description | Allowed Transitions |
|--------|-------------|---------------------|
| `ACTIVE` | Load is active and available | → IN_TRANSIT, STORED, ERROR |
| `IN_TRANSIT` | Load is being moved | → STORED, RETRIEVED, ERROR |
| `STORED` | Load is stored at location | → IN_TRANSIT, RETRIEVED |
| `RETRIEVED` | Load has been retrieved | (terminal state) |
| `ERROR` | Load has an error condition | → ACTIVE (after resolution) |

---

## Dependencies

### Calls OSUB Functions:
- `LOAD_init()`: Initialize structure
- `LOAD_insert()`: Insert record
- `LOAD_update()`: Update record
- `LOAD_select_LOAD_PR_KEY()`: Select by primary key
- `LOAD_select_LOAD_LOCATION()`: Select by location

### Calls Other DSUB Functions:
- `ds_locn_validate()`: Validate location exists
- `ds_loadtype_validate()`: Validate load type
- `ds_invt_update_location()`: Update inventory location

### Calls CSUB Functions:
- `cs_log_printf()`: Logging
- `cs_dtm_current()`: Get current date/time

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| OSUB Load Functions | [os_load.md](../01_OSUB/os_load.md) | Generated CRUD functions |
| Load Table Structure | [os_load.md](../01_OSUB/os_load.md) | Table Structure |
| Inventory Functions | [ds_invt.md](ds_invt.md) | Inventory operations |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

