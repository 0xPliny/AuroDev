# ds_sttn - Station Operations Module

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_sttn.cpp` / `ds_sttn.h` |
| **Library** | DSUB (Database Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Status** | ✅ Active - Station Operations |

---

## Overview

The `ds_sttn` module provides business logic functions for station operations. It handles station selection, arrival processing, mode management, and station group operations.

**Key Responsibilities:**
- Station selection for moves
- Arrival and departure processing
- Station mode management
- Station group operations
- Error handling coordination

---

## Key Functions

### ds_sttn_select_for_retrieve

**Signature:**
```cpp
long ds_sttn_select_for_retrieve(int aisle, int loadType, STTN* sttn);
```

**Purpose:** Select output station for a retrieve operation.

| Parameter | Type | Description |
|-----------|------|-------------|
| `aisle` | `int` | Aisle number |
| `loadType` | `int` | Load type |
| `sttn` | `STTN*` | Output: Selected station |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Station selected |
| `GP.EMPTY` | No available station |
| `GP.BAD` | Error |

**Logic Flow:**
1. Query output stations for aisle
2. Filter by mode (can accept retrieves)
3. Check capacity (not full)
4. Apply load type restrictions
5. Return selected station

---

### ds_sttn_process_arrival

**Signature:**
```cpp
long ds_sttn_process_arrival(int stationId, const char* loadId);
```

**Purpose:** Process load arrival at station.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stationId` | `int` | Station identifier |
| `loadId` | `const char*` | Arriving load ID |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Arrival processed |
| `GP.BAD` | Processing failed |

**Logic Flow:**
1. Validate station and load
2. Update station load present flag
3. Update load location
4. Create store move if needed
5. Trigger downstream processing

---

### ds_sttn_process_departure

**Signature:**
```cpp
long ds_sttn_process_departure(int stationId);
```

**Purpose:** Process load departure from station.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stationId` | `int` | Station identifier |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Departure processed |
| `GP.BAD` | Processing failed |

**Logic Flow:**
1. Clear station load present flag
2. Update load location
3. Complete any pending moves
4. Clear station errors

---

### ds_sttn_set_mode

**Signature:**
```cpp
long ds_sttn_set_mode(int stationId, int mode);
```

**Purpose:** Set station operating mode.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stationId` | `int` | Station identifier |
| `mode` | `int` | New mode (GP.ST.MODES) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Mode set |
| `GP.BAD` | Mode change failed |

---

### ds_sttn_get_group_stations

**Signature:**
```cpp
long ds_sttn_get_group_stations(int groupId, STTN* stations, int* count);
```

**Purpose:** Get all stations in a group.

| Parameter | Type | Description |
|-----------|------|-------------|
| `groupId` | `int` | Station group ID |
| `stations` | `STTN*` | Output: Array of stations |
| `count` | `int*` | In/Out: Array size / count returned |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Stations retrieved |
| `GP.BAD` | Error |

---

### ds_sttn_check_availability

**Signature:**
```cpp
long ds_sttn_check_availability(int stationId, int moveType);
```

**Purpose:** Check if station can accept a move type.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stationId` | `int` | Station identifier |
| `moveType` | `int` | Move type |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Station available |
| `GP.BAD` | Station not available |

---

## Common Usage Patterns

### Pattern 1: Select Output Station

```cpp
STTN sttn;

// Select output station for retrieve
if (ds_sttn_select_for_retrieve(aisle, loadType, &sttn) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Selected station %s for output", sttn.station);
    move.to_station = sttn.sttn_no;
}
```

### Pattern 2: Process Arrival

```cpp
// Load arrived at input station
if (ds_sttn_process_arrival(stationId, loadId) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Load %s arrived at station %d", loadId, stationId);
} else {
    cs_log_printf(FILELINE, GP_LOGLVL_1, 
                  "Failed to process arrival for %s", loadId);
}
```

### Pattern 3: Mode Change with Validation

```cpp
// Change station to store-only mode
if (ds_sttn_check_availability(stationId, MOVE_STORE) == GP.GOOD) {
    ds_sttn_set_mode(stationId, GP.ST.MODES.STORE);
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Station %d set to store-only mode", stationId);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Station Table | [os_sttn.md](../01_OSUB/os_sttn.md) | STTN database operations |
| Station Control | [cc_std.md](../02_CCSUB/cc_std.md) | Station control class |
| Station Constants | [Station_Constants.md](../05_ICISDefines/Station_Constants.md) | ST enumerations |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

