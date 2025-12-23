# cc_route - Route Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_route.cpp` / `cc_route.h` |
| **Library** | CCSUB (Common Control Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Route Management |

---

## Overview

The `cc_route` class provides routing functionality for load movement within the MHC/MEM system. It determines the path a load should take through the system, including which conveyors, stations, stackers, and vehicles are involved in a move operation.

**Key Responsibilities:**
- Route calculation for load movements
- Route validation and availability checking
- Multi-leg route support
- Route priority management
- Route blocking/unblocking

---

## Class Definition

```cpp
class cc_route {
private:
    int     route_id;             // Route identifier
    char    route_name[32];       // Route name
    int     source_type;          // Source device type
    int     source_id;            // Source device ID
    int     dest_type;            // Destination device type
    int     dest_id;              // Destination device ID
    int     route_priority;       // Route priority
    bool    route_available;      // Route availability flag
    
public:
    cc_route();
    ~cc_route();
    
    // Initialization
    long init(int routeId);
    long init_by_endpoints(int srcType, int srcId, int destType, int destId);
    
    // Route Methods
    long find_route(int srcType, int srcId, int destType, int destId, ROUTE* route);
    long get_route_steps(int routeId, ROUTE_STEP* steps, int* stepCount);
    long get_best_route(int srcType, int srcId, int destType, int destId);
    
    // Availability
    bool is_route_available(int routeId);
    long check_route_status(int routeId, ROUTE_STATUS* status);
    long block_route(int routeId, const char* reason);
    long unblock_route(int routeId);
    
    // Validation
    long validate_route(int routeId);
    bool can_use_route(int routeId, int loadType, int loadSize);
    
    // Statistics
    long get_route_usage(int routeId, ROUTE_USAGE* usage);
};
```

---

## Key Structures

### ROUTE Structure

```cpp
struct ROUTE {
    int     route_id;             // Route identifier
    char    route_name[32];       // Route name
    int     source_type;          // Source type
    int     source_id;            // Source ID
    int     dest_type;            // Destination type
    int     dest_id;              // Destination ID
    int     priority;             // Route priority
    int     step_count;           // Number of steps
    bool    is_available;         // Availability flag
};
```

### ROUTE_STEP Structure

```cpp
struct ROUTE_STEP {
    int     step_number;          // Step sequence
    int     device_type;          // Device type for step
    int     device_id;            // Device ID
    int     action;               // Action at step
    int     next_step;            // Next step number
};
```

### ROUTE_STATUS Structure

```cpp
struct ROUTE_STATUS {
    bool    is_available;         // Overall availability
    int     blocked_step;         // Step causing block (if any)
    int     blocked_reason;       // Reason code for block
    char    blocked_desc[128];    // Description of block
};
```

---

## Key Methods

### cc_route::find_route

**Signature:**
```cpp
long cc_route::find_route(int srcType, int srcId, int destType, int destId, ROUTE* route);
```

**Purpose:** Find a route between source and destination.

| Parameter | Type | Description |
|-----------|------|-------------|
| `srcType` | `int` | Source device type |
| `srcId` | `int` | Source device ID |
| `destType` | `int` | Destination device type |
| `destId` | `int` | Destination device ID |
| `route` | `ROUTE*` | Output: Found route |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Route found |
| `GP.BAD` | No route available |

**Logic Flow:**
1. Query MHC_ROUTE table for matching routes
2. Filter by availability
3. Sort by priority
4. Return best available route

---

### cc_route::get_route_steps

**Signature:**
```cpp
long cc_route::get_route_steps(int routeId, ROUTE_STEP* steps, int* stepCount);
```

**Purpose:** Get all steps for a route.

| Parameter | Type | Description |
|-----------|------|-------------|
| `routeId` | `int` | Route identifier |
| `steps` | `ROUTE_STEP*` | Output: Array of route steps |
| `stepCount` | `int*` | Output: Number of steps |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Steps retrieved |
| `GP.BAD` | Route not found |

---

### cc_route::check_route_status

**Signature:**
```cpp
long cc_route::check_route_status(int routeId, ROUTE_STATUS* status);
```

**Purpose:** Check current status of route including any blockages.

| Parameter | Type | Description |
|-----------|------|-------------|
| `routeId` | `int` | Route identifier |
| `status` | `ROUTE_STATUS*` | Output: Route status |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Status retrieved |
| `GP.BAD` | Route not found |

**Logic Flow:**
1. Check each step's device status
2. Check for errors on route devices
3. Check for maintenance blocks
4. Return consolidated status

---

### cc_route::block_route / cc_route::unblock_route

**Signature:**
```cpp
long cc_route::block_route(int routeId, const char* reason);
long cc_route::unblock_route(int routeId);
```

**Purpose:** Block or unblock a route from use.

| Parameter | Type | Description |
|-----------|------|-------------|
| `routeId` | `int` | Route identifier |
| `reason` | `const char*` | Reason for block |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Operation successful |
| `GP.BAD` | Operation failed |

**Usage:**
- Block routes during maintenance
- Block routes due to equipment failure
- Unblock when ready to resume

---

## Common Usage Patterns

### Pattern 1: Find Best Route for Move

```cpp
cc_route router;
ROUTE route;

// Find route from station to rack location
if (router.find_route(GP.DEV.STATION, stationId, 
                       GP.DEV.STACKER, stackerId, &route) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Found route %d: %s (priority=%d)", 
                  route.route_id, route.route_name, route.priority);
}
```

### Pattern 2: Check Route Availability Before Use

```cpp
cc_route router;
ROUTE_STATUS status;

if (router.check_route_status(routeId, &status) == GP.GOOD) {
    if (status.is_available) {
        cs_log_printf(FILELINE, GP_LOGLVL_3, 
                      "Route %d is available", routeId);
    } else {
        cs_log_printf(FILELINE, GP_LOGLVL_2, 
                      "Route %d blocked at step %d: %s", 
                      routeId, status.blocked_step, status.blocked_desc);
    }
}
```

### Pattern 3: Get Route Steps for Execution

```cpp
cc_route router;
ROUTE_STEP steps[MAX_ROUTE_STEPS];
int stepCount;

if (router.get_route_steps(routeId, steps, &stepCount) == GP.GOOD) {
    for (int i = 0; i < stepCount; i++) {
        cs_log_printf(FILELINE, GP_LOGLVL_4, 
                      "Step %d: Device type=%d, id=%d, action=%d",
                      steps[i].step_number,
                      steps[i].device_type,
                      steps[i].device_id,
                      steps[i].action);
    }
}
```

### Pattern 4: Block Route for Maintenance

```cpp
cc_route router;

// Block route during maintenance
if (router.block_route(routeId, "Scheduled maintenance") == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Route %d blocked for maintenance", routeId);
    
    // Perform maintenance...
    
    // Unblock when complete
    router.unblock_route(routeId);
    cs_log_printf(FILELINE, GP_LOGLVL_2, 
                  "Route %d unblocked", routeId);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Route Table | [os_route.md](../01_OSUB/os_route.md) | ROUTE database operations |
| Move Functions | [ds_movs.md](../04_DSUB/ds_movs.md) | Move operations |
| Station Control | [cc_std.md](cc_std.md) | Station operations |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

