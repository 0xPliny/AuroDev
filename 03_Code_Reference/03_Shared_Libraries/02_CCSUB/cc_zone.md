# cc_zone - Zone Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_zone.cpp` / `cc_zone.h` |
| **Library** | CCSUB (Common Control Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Zone Management |

---

## Overview

The `cc_zone` class provides zone management functionality for the MHC/MEM system. Zones are logical groupings of storage locations used for inventory management, location selection, and capacity tracking. This class handles zone configuration, capacity monitoring, and zone-based location selection.

**Key Responsibilities:**
- Zone configuration and initialization
- Capacity tracking (total, used, available)
- Zone-based location selection
- Zone lock/unlock management
- Zone priority handling

---

## Class Definition

```cpp
class cc_zone {
private:
    int     zone_id;              // Zone identifier
    char    zone_name[32];        // Zone name
    int     total_locations;      // Total locations in zone
    int     used_locations;       // Currently occupied
    int     available_locations;  // Available for storage
    int     reserved_locations;   // Reserved count
    int     priority;             // Zone priority for selection
    bool    locked;               // Zone locked flag
    
public:
    cc_zone();
    ~cc_zone();
    
    // Initialization
    long init(int zoneId);
    long init_by_name(const char* zoneName);
    
    // Capacity Methods
    int get_total_locations();
    int get_used_locations();
    int get_available_locations();
    int get_reserved_locations();
    float get_utilization_percent();
    
    // Status Methods
    bool is_full();
    bool is_empty();
    bool is_locked();
    long lock_zone();
    long unlock_zone();
    
    // Location Selection
    long get_store_location(LOCN* locn, int loadSize);
    long get_retrieve_location(LOCN* locn, const char* loadId);
    
    // Statistics
    long refresh_counts();
    long get_zone_stats(ZONE_STATS* stats);
};
```

---

## Key Methods

### cc_zone::init

**Signature:**
```cpp
long cc_zone::init(int zoneId);
```

**Purpose:** Initialize the zone control object from zone ID.

| Parameter | Type | Description |
|-----------|------|-------------|
| `zoneId` | `int` | Zone identifier |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Initialization successful |
| `GP.BAD` | Zone not found |

**Logic Flow:**
1. Validate zone ID
2. Load zone configuration from database
3. Calculate capacity metrics
4. Initialize internal state

---

### cc_zone::get_available_locations

**Signature:**
```cpp
int cc_zone::get_available_locations();
```

**Purpose:** Get count of available locations in zone for storage.

| Return Value | Description |
|--------------|-------------|
| `>= 0` | Number of available locations |

**Logic Flow:**
1. Query MHC_LOCATION for zone
2. Count locations with Status = 'Empty' and Locked_Flag = 'Unlocked'
3. Return count

---

### cc_zone::get_utilization_percent

**Signature:**
```cpp
float cc_zone::get_utilization_percent();
```

**Purpose:** Calculate zone utilization as percentage.

| Return Value | Description |
|--------------|-------------|
| `0.0 - 100.0` | Utilization percentage |

**Calculation:**
```cpp
return (float)used_locations / (float)total_locations * 100.0;
```

---

### cc_zone::get_store_location

**Signature:**
```cpp
long cc_zone::get_store_location(LOCN* locn, int loadSize);
```

**Purpose:** Find an available storage location in zone for the given load size.

| Parameter | Type | Description |
|-----------|------|-------------|
| `locn` | `LOCN*` | Output: Selected location record |
| `loadSize` | `int` | Load size category |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Location found |
| `GP.RACK_FULL` | No available locations in zone |
| `GP.BAD` | Error during selection |

**Logic Flow:**
1. Check if zone is locked
2. Query available locations matching load size
3. Apply selection criteria (FIFO, closest, etc.)
4. Lock selected location
5. Return location record

---

### cc_zone::lock_zone / cc_zone::unlock_zone

**Signature:**
```cpp
long cc_zone::lock_zone();
long cc_zone::unlock_zone();
```

**Purpose:** Lock or unlock entire zone from new operations.

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Operation successful |
| `GP.BAD` | Operation failed |

**Usage:**
- Lock zone during maintenance
- Lock zone when capacity exceeded
- Unlock zone when ready to resume

---

## Common Usage Patterns

### Pattern 1: Check Zone Capacity

```cpp
cc_zone zone;

if (zone.init(zoneId) == GP.GOOD) {
    int available = zone.get_available_locations();
    float utilization = zone.get_utilization_percent();
    
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Zone %d: %d available (%.1f%% utilized)", 
                  zoneId, available, utilization);
    
    if (zone.is_full()) {
        cs_log_printf(FILELINE, GP_LOGLVL_2, 
                      "Zone %d is FULL", zoneId);
    }
}
```

### Pattern 2: Find Storage Location in Zone

```cpp
cc_zone zone;
LOCN locn;

zone.init(zoneId);

if (zone.get_store_location(&locn, loadSize) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Selected location: %s", locn.location);
} else if (zone.is_full()) {
    // Try another zone
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Zone full, trying alternate zone");
}
```

### Pattern 3: Zone Lock for Maintenance

```cpp
cc_zone zone;
zone.init(zoneId);

// Lock zone during maintenance
if (zone.lock_zone() == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Zone %d locked for maintenance", zoneId);
    
    // Perform maintenance operations...
    
    // Unlock when complete
    zone.unlock_zone();
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Zone %d unlocked", zoneId);
}
```

### Pattern 4: Refresh Zone Statistics

```cpp
cc_zone zone;
zone.init(zoneId);

// Refresh counts after inventory operations
zone.refresh_counts();

cs_log_printf(FILELINE, GP_LOGLVL_3, 
              "Zone %d: Total=%d, Used=%d, Available=%d", 
              zoneId,
              zone.get_total_locations(),
              zone.get_used_locations(),
              zone.get_available_locations());
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Location Selection | [ds_get_locn.md](../04_DSUB/ds_get_locn.md) | Location algorithms |
| Location Table | [os_locn.md](../01_OSUB/os_locn.md) | LOCN database operations |
| Location Constants | [Location_Constants.md](../05_ICISDefines/Location_Constants.md) | Location status codes |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

