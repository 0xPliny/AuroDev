# cc_points - Point/Path Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_points.cpp` / `cc_points.h` |
| **Library** | CCSUB (Common Control Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Vehicle Pathing |

---

## Overview

The `cc_points` class manages the point and path system used by MUDP vehicles (AGVs and RTNX). It handles point definitions, path calculations, zone management, loop control, and bridge coordination for vehicle navigation. This class is critical for RTNX and AGV path planning and traffic control.

**Key Responsibilities:**
- Point definition and management
- Path calculation between points
- Zone/loop/bridge control
- Point locking for traffic management
- Vehicle position tracking

---

## Class Definition

```cpp
class cc_points {
private:
    int     point_count;              // Total points in system
    int     zone_count;               // Total zones
    int     loop_count;               // Total loops
    int     bridge_count;             // Total bridges
    int     group_count;              // Total groups
    
public:
    cc_points();
    ~cc_points();
    
    // Initialization
    long init();
    long load_from_xml(const char* filename);
    
    // Point Methods
    long get_point(int pointId, POINT* point);
    long get_point_by_name(const char* name, POINT* point);
    int get_point_count();
    
    // Path Methods
    long calculate_path(int fromPoint, int toPoint, PATH* path);
    long get_distance(int fromPoint, int toPoint);
    long get_next_point(int currentPoint, int destinationPoint);
    
    // Zone Methods
    long get_zone(int zoneId, ZONE* zone);
    bool is_point_in_zone(int pointId, int zoneId);
    long lock_zone(int zoneId);
    long unlock_zone(int zoneId);
    
    // Loop Methods
    long get_loop(int loopId, LOOP* loop);
    long get_loop_for_point(int pointId);
    long lock_loop(int loopId, int vehicleId);
    long unlock_loop(int loopId, int vehicleId);
    
    // Bridge Methods
    long get_bridge(int bridgeId, BRIDGE* bridge);
    long lock_bridge(int bridgeId, int vehicleId);
    long unlock_bridge(int bridgeId, int vehicleId);
    bool is_bridge_locked(int bridgeId);
    
    // Point Locking
    long lock_point(int pointId, int vehicleId);
    long unlock_point(int pointId, int vehicleId);
    bool is_point_locked(int pointId);
    int get_point_owner(int pointId);
};
```

---

## Key Structures

### POINT Structure

```cpp
struct POINT {
    int     point_id;             // Point identifier
    char    point_name[32];       // Point name
    int     x_coord;              // X coordinate
    int     y_coord;              // Y coordinate
    int     z_coord;              // Z coordinate (level)
    int     zone_id;              // Zone containing point
    int     loop_id;              // Loop containing point
    int     point_type;           // Point type
    bool    is_locked;            // Lock status
    int     locked_by;            // Vehicle ID holding lock
};
```

### PATH Structure

```cpp
struct PATH {
    int     point_count;          // Number of points in path
    int     points[MAX_PATH_POINTS]; // Point IDs in sequence
    int     total_distance;       // Total path distance
    int     estimated_time;       // Estimated travel time
};
```

---

## Key Methods

### cc_points::init

**Signature:**
```cpp
long cc_points::init();
```

**Purpose:** Initialize points system from configuration.

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Initialization successful |
| `GP.BAD` | Initialization failed |

**Logic Flow:**
1. Load points.xml configuration
2. Initialize mapped memory structures
3. Set up zone/loop/bridge relationships
4. Clear all point locks

---

### cc_points::calculate_path

**Signature:**
```cpp
long cc_points::calculate_path(int fromPoint, int toPoint, PATH* path);
```

**Purpose:** Calculate optimal path between two points.

| Parameter | Type | Description |
|-----------|------|-------------|
| `fromPoint` | `int` | Starting point ID |
| `toPoint` | `int` | Destination point ID |
| `path` | `PATH*` | Output: Calculated path |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Path calculated successfully |
| `GP.BAD` | No path available |

**Logic Flow:**
1. Validate source and destination points
2. Apply shortest-path algorithm
3. Consider zone restrictions
4. Build point sequence
5. Calculate total distance

---

### cc_points::lock_point / cc_points::unlock_point

**Signature:**
```cpp
long cc_points::lock_point(int pointId, int vehicleId);
long cc_points::unlock_point(int pointId, int vehicleId);
```

**Purpose:** Lock or unlock a point for vehicle traffic control.

| Parameter | Type | Description |
|-----------|------|-------------|
| `pointId` | `int` | Point to lock/unlock |
| `vehicleId` | `int` | Vehicle requesting lock |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Operation successful |
| `GP.BAD` | Point already locked by another vehicle |

---

### cc_points::lock_loop / cc_points::unlock_loop

**Signature:**
```cpp
long cc_points::lock_loop(int loopId, int vehicleId);
long cc_points::unlock_loop(int loopId, int vehicleId);
```

**Purpose:** Lock or unlock an RTNX loop for vehicle entry.

| Parameter | Type | Description |
|-----------|------|-------------|
| `loopId` | `int` | Loop identifier |
| `vehicleId` | `int` | Vehicle requesting lock |

**Usage:**
- RTNX vehicles lock loops to enter
- Only one vehicle per loop at a time
- Prevents collisions in loop areas

---

### cc_points::lock_bridge / cc_points::unlock_bridge

**Signature:**
```cpp
long cc_points::lock_bridge(int bridgeId, int vehicleId);
long cc_points::unlock_bridge(int bridgeId, int vehicleId);
```

**Purpose:** Lock or unlock a bridge connection between loops.

| Parameter | Type | Description |
|-----------|------|-------------|
| `bridgeId` | `int` | Bridge identifier |
| `vehicleId` | `int` | Vehicle requesting lock |

**Usage:**
- Bridges connect different loops
- Lock required before traversing
- Enables inter-loop travel

---

## Common Usage Patterns

### Pattern 1: Calculate Vehicle Path

```cpp
cc_points points;
PATH path;

points.init();

if (points.calculate_path(fromPoint, toPoint, &path) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Path: %d points, distance=%d", 
                  path.point_count, path.total_distance);
    
    // Display path points
    for (int i = 0; i < path.point_count; i++) {
        cs_log_printf(FILELINE, GP_LOGLVL_4, 
                      "  Point %d: %d", i, path.points[i]);
    }
}
```

### Pattern 2: Lock Point for Vehicle Movement

```cpp
cc_points points;
points.init();

// Try to lock destination point
if (points.lock_point(destPoint, vehicleId) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Point %d locked for vehicle %d", 
                  destPoint, vehicleId);
    
    // After vehicle arrives, unlock
    points.unlock_point(destPoint, vehicleId);
}
```

### Pattern 3: Loop Entry Control for RTNX

```cpp
cc_points points;
points.init();

int loopId = points.get_loop_for_point(destPoint);

// Lock loop before entering
if (points.lock_loop(loopId, vehicleId) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "RTNX %d entering loop %d", vehicleId, loopId);
    
    // Vehicle moves within loop...
    
    // Unlock when exiting
    points.unlock_loop(loopId, vehicleId);
}
```

### Pattern 4: Bridge Traversal

```cpp
cc_points points;
points.init();

// Check if bridge is available
if (!points.is_bridge_locked(bridgeId)) {
    if (points.lock_bridge(bridgeId, vehicleId) == GP.GOOD) {
        cs_log_printf(FILELINE, GP_LOGLVL_3, 
                      "Vehicle %d crossing bridge %d", 
                      vehicleId, bridgeId);
        
        // Vehicle crosses bridge...
        
        points.unlock_bridge(bridgeId, vehicleId);
    }
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Vehicle Control | [cc_vehicle.md](cc_vehicle.md) | Vehicle operations |
| MUDP Protocol | [cc_mudp.md](cc_mudp.md) | MUDP communication |
| System Maximums | [System_Parameters.md](../06_global_prm/System_Parameters.md) | PNT_POINTS, PNT_ZONES, etc. |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

