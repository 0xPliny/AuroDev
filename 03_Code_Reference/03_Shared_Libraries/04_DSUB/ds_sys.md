# ds_sys - System Operations Module

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_sys.cpp` / `ds_sys.h` |
| **Library** | DSUB (Database Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Status** | ✅ Active - System Operations |

---

## Overview

The `ds_sys` module provides system-level database operations for the MHC/MEM system. It handles system configuration, warm start initialization, and system state management.

**Key Responsibilities:**
- System configuration retrieval
- Warm start/cold start handling
- System state management
- Area/zone initialization
- System parameter access

---

## Key Functions

### ds_sys_init

**Signature:**
```cpp
long ds_sys_init(const char* area);
```

**Purpose:** Initialize system for specified area.

| Parameter | Type | Description |
|-----------|------|-------------|
| `area` | `const char*` | Area code (e.g., "A") |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Initialization successful |
| `GP.BAD` | Initialization failed |

**Logic Flow:**
1. Load system record from MHC_SYS
2. Initialize area-specific parameters
3. Set up database connections
4. Load configuration elements

---

### ds_sys_get_config

**Signature:**
```cpp
long ds_sys_get_config(const char* paramName, char* value, int valueSize);
```

**Purpose:** Get system configuration parameter.

| Parameter | Type | Description |
|-----------|------|-------------|
| `paramName` | `const char*` | Parameter name |
| `value` | `char*` | Output value buffer |
| `valueSize` | `int` | Buffer size |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Parameter found |
| `GP.EMPTY` | Parameter not found |
| `GP.BAD` | Error |

---

### ds_sys_warm_start

**Signature:**
```cpp
long ds_sys_warm_start();
```

**Purpose:** Perform warm start initialization.

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Warm start successful |
| `GP.BAD` | Warm start failed |

**Logic Flow:**
1. Load system state from database
2. Restore mapped memory from disk
3. Reinitialize equipment states
4. Resume pending operations

---

### ds_sys_cold_start

**Signature:**
```cpp
long ds_sys_cold_start();
```

**Purpose:** Perform cold start (full reset).

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Cold start successful |
| `GP.BAD` | Cold start failed |

**Logic Flow:**
1. Clear mapped memory
2. Reset all equipment states
3. Clear pending moves
4. Initialize system to default state

---

### ds_sys_get_area_count

**Signature:**
```cpp
int ds_sys_get_area_count();
```

**Purpose:** Get number of configured areas.

| Return Value | Description |
|--------------|-------------|
| `>= 0` | Number of areas |

---

### ds_sys_update_state

**Signature:**
```cpp
long ds_sys_update_state(int state);
```

**Purpose:** Update system operational state.

| Parameter | Type | Description |
|-----------|------|-------------|
| `state` | `int` | New system state |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | State updated |
| `GP.BAD` | Update failed |

---

## Common Usage Patterns

### Pattern 1: System Initialization

```cpp
// Initialize system for area "A"
if (ds_sys_init("A") == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_2, "System initialized for area A");
    
    // Perform warm start
    if (ds_sys_warm_start() == GP.GOOD) {
        cs_log_printf(FILELINE, GP_LOGLVL_2, "Warm start complete");
    }
}
```

### Pattern 2: Get Configuration Parameter

```cpp
char dbServer[128];

if (ds_sys_get_config("DATABASE_SERVER", dbServer, sizeof(dbServer)) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, "Database server: %s", dbServer);
}
```

### Pattern 3: System State Management

```cpp
// Set system to running state
ds_sys_update_state(SYS_STATE_RUNNING);

// ... system operations ...

// Set system to shutdown state
ds_sys_update_state(SYS_STATE_SHUTDOWN);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| System Table | [os_sys.md](../01_OSUB/os_sys.md) | SYS database operations |
| System Control | [cc_sys.md](../02_CCSUB/cc_sys.md) | System control class |
| Configuration | [cs_elt.md](../03_CSUB/cs_elt.md) | Element access |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

