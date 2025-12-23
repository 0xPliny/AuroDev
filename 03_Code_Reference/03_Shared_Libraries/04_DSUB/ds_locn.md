# ds_locn - Location Database Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_locn.cpp`, `ds_locn.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Module Name** | `ds_locn` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `ds_locn` module provides **database functions and business logic** for location operations in the MHC system. It builds on top of the OSUB `os_locn` generated functions, adding business logic for location availability, locking, and status management.

**Critical Operations:**
- Location availability checking
- Location status management
- Location locking/unlocking
- Location validation
- Pending status management

---

## Key Functions

### ds_locn_validate

**Signature:**
```cpp
long ds_locn_validate(const char* location)
```

**Description:**
Validates that a location exists and is accessible.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | `const char*` | Location identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - location is valid |
| `-1` | `GP.BAD` | Failure - location not found or locked |

---

### ds_locn_set_pending

**Signature:**
```cpp
long ds_locn_set_pending(const char* location, const char* loadId, const char* pendingStatus)
```

**Description:**
Sets pending status on location (e.g., PEND_STOR, PEND_RTRV).

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | `const char*` | Location identifier |
| `loadId` | `const char*` | Load ID that will occupy location |
| `pendingStatus` | `const char*` | Pending status (PEND_STOR, PEND_RTRV) |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - pending status set |
| `-1` | `GP.BAD` | Failure - location not found or error |

---

### ds_locn_clear_pending

**Signature:**
```cpp
long ds_locn_clear_pending(const char* location)
```

**Description:**
Clears pending status on location after move completes.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | `const char*` | Location identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - pending cleared |
| `-1` | `GP.BAD` | Failure - location not found |

---

## Common Usage Patterns

### Pattern 1: Validate Location Before Use

```cpp
if (ds_locn_validate(targetLocation) == GP.GOOD) {
    // Location is valid, proceed with move
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Invalid location: %s", targetLocation);
    return GP.BAD;
}
```

### Pattern 2: Set Pending Before Store

```cpp
if (ds_locn_set_pending(targetLocation, loadId, "PEND_STOR") == GP.GOOD) {
    // Location is reserved, proceed with store
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to set pending on location %s", targetLocation);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| OSUB Location Functions | [os_locn.md](../01_OSUB/os_locn.md) | Generated CRUD functions |
| Location Selection | [ds_get_locn.md](ds_get_locn.md) | Location selection algorithms |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

