# ds_get_locn - Location Selection Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_get_locn.cpp`, `ds_get_locn.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Module Name** | `ds_get_locn` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `ds_get_locn` module provides **location selection algorithms** for finding available storage locations based on business rules, zones, size constraints, and optimization criteria.

**Critical Operations:**
- Location selection for store operations
- Zone-based location selection
- Size-appropriate location selection
- Optimization zone consideration
- Double-deep location handling

**Key Principle:** Location selection uses complex business rules to optimize warehouse space utilization.

---

## Key Functions

### ds_get_locn_for_store

**Signature:**
```cpp
long ds_get_locn_for_store(LOCN* locn, const char* loadType, long loadSize, 
                           const char* zone, const char* area)
```

**Description:**
Selects an available location for storing a load based on load type, size, zone, and area.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `locn` | `LOCN*` | Output - Selected location record |
| `loadType` | `const char*` | Load type (PALLET, CASE, etc.) |
| `loadSize` | `long` | Load size code |
| `zone` | `const char*` | Optional zone preference |
| `area` | `const char*` | Area identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - location selected |
| `-1` | `GP.BAD` | Failure - no available location found |

**Logic Flow:**
1. Builds query for available locations:
   - `location_status` = "AVAIL"
   - `location_size` >= `loadSize`
   - Optional `zone` filter
   - `area` = specified area
2. Orders by optimization criteria:
   - `opt_zone` (optimization zone)
   - `search_key_1`, `search_key_2` (search keys)
   - `store_count` (exercise count)
3. Selects first matching location
4. Returns location in `locn` structure

**Usage Example:**
```cpp
LOCN locn;
if (ds_get_locn_for_store(&locn, "PALLET", 1, "ZONE1", "A") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Selected location %s for store", locn.location);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "No available location found");
}
```

---

### ds_get_locn_by_zone

**Signature:**
```cpp
long ds_get_locn_by_zone(LOCN* locn, const char* zone, long loadSize, const char* area)
```

**Description:**
Selects location within a specific zone.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `locn` | `LOCN*` | Output - Selected location |
| `zone` | `const char*` | Zone identifier |
| `loadSize` | `long` | Load size code |
| `area` | `const char*` | Area identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - location selected |
| `-1` | `GP.BAD` | Failure - no location in zone |

---

## Common Usage Patterns

### Pattern 1: Select Location for Store

```cpp
LOCN locn;
if (ds_get_locn_for_store(&locn, loadType, loadSize, preferredZone, area) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Selected location %s for load %s", 
                  locn.location, loadId);
    
    // Set pending status
    ds_locn_set_pending(locn.location, loadId, "PEND_STOR");
    
    // Create move to selected location
    ds_movs_create(&movs, loadId, currentLocation, locn.location, "STORE", area);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "No available location found for store");
}
```

### Pattern 2: Zone-Based Selection

```cpp
LOCN locn;
if (ds_get_locn_by_zone(&locn, "ZONE1", loadSize, area) == GP.GOOD) {
    // Use location in preferred zone
} else {
    // Fall back to any available location
    ds_get_locn_for_store(&locn, loadType, loadSize, NULL, area);
}
```

---

## Location Selection Criteria

**Priority Order:**
1. Location status = AVAIL
2. Location size >= load size
3. Zone match (if specified)
4. Optimization zone (opt_zone)
5. Search keys (search_key_1, search_key_2)
6. Exercise count (store_count) - prefer less exercised locations

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Location Functions | [ds_locn.md](ds_locn.md) | Location operations |
| OSUB Location Functions | [os_locn.md](../01_OSUB/os_locn.md) | Generated CRUD functions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

