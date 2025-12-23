# CCSUB Library - Common Control Subroutines

**Document Version:** 2.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Overview

CCSUB (Common Control Subroutines) is the **core equipment control library** for the MHC/MEM system. It provides C++ classes for equipment control, communication, shared memory management, and utility functions. CCSUB classes are mapped to shared memory, allowing multiple processes to access equipment state simultaneously.

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\`

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                  Background Processes Layer                     │
│    (Dispatchers, Completers, Communications, Utilities)         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        CCSUB Layer                              │
│  ┌──────────────────────┐  ┌──────────────────────────────────┐ │
│  │  Control Classes     │  │  Utility Functions               │ │
│  │  (cc_stk, cc_std,   │  │  (cs_log, cs_msg, cs_reg,       │ │
│  │   cc_zone, cc_agv,   │  │   cs_dtm, cs_err, cs_elt...)   │ │
│  │   cc_rtn, cc_plc...) │  │                                  │ │
│  └──────────────────────┘  └──────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Mapped Memory Layer                          │
│               (Shared Memory Files - .shm)                      │
└─────────────────────────────────────────────────────────────────┘
```

---

## Control Classes (cc_*)

### 1. cc_stk - Stacker Crane Class

**Files:** `cc_stk.h`, `cc_stk.cpp`

The `cc_stacker` class manages stacker crane state, communication, and operations.

#### Class Definition
```cpp
class cc_stacker {
public:
    // Constructor
    cc_stacker();
    cc_stacker(cc_stacker* srP);
    
    // Identity
    long    aisle();                    // Aisle number
    char*   name();                     // Stacker name (e.g., "SR01")
    char*   long_name();                // Long display name
    
    // Status Properties
    long    comm();                     // Communication status
    long    enabled();                  // Enabled flag
    long    error_code();               // Current error code
    long    error_sub_code();           // Error sub-code
    long    func();                     // Current function
    long    hard();                     // Hardware status
    long    mode();                     // Operating mode
    long    schd();                     // Schedule status
    long    soft();                     // Software status
    
    // Position
    long    bay();                      // Current bay
    long    level();                    // Current level
    
    // Fork Operations (MUDP)
    long    forks();                    // Number of forks
    char*   loadid(long fork);          // Load ID on fork
    long    loaded(long fork);          // Fork loaded status
    long    func_step(long fork);       // Current step for fork
    long    fork_need_next_step(long fork);  // Needs next step
    long    fork_need_same_step(long fork);  // Needs same step repeated
    
    // Configuration
    long    double_deep();              // Double-deep capability
    long    space_storage();            // Space storage crane
    long    Use_BCR();                  // BCR usage flag
};

// Global stacker control
cc_stk_ctrl cc_stacker_ctrl;
```

#### Stacker Finder Methods
```cpp
cc_stacker* cc_stk_ctrl::Find_Stk(const char* name);     // By name
cc_stacker* cc_stk_ctrl::Find_Stk(const long aisle);     // By aisle
cc_stacker* cc_stk_ctrl::Find_Stk_Idx(const long idx);   // By index
cc_stacker* cc_stk_ctrl::Find_Stk_Next(cc_stacker* SR);  // Next stacker
```

---

### 2. cc_std - Stand (Station) Class

**Files:** `cc_std.h`, `cc_std.cpp`

The `cc_stand` class manages stations, buffers, and conveyor positions.

#### Class Definition
```cpp
class cc_stand {
public:
    // Constructor
    cc_stand();
    cc_stand(cc_stand* stP);
    
    // Identity
    char*   name();                     // Stand name
    char*   station_name();             // Display name
    char*   description();              // Description
    long    type();                     // Stand type (see GP.ST.TYP)
    
    // Position
    long    aisle();                    // Aisle number
    long    bay();                      // Bay number
    long    level();                    // Level number
    long    zone();                     // Zone number
    
    // Status
    long    available();                // Available for use
    long    enabled();                  // Enabled flag
    long    errorflag();                // Error flag
    long    errorno();                  // Error number
    long    hard();                     // Hardware status
    long    loaded();                   // Load present
    long    loaded2();                  // Second load (dual position)
    long    soft();                     // Software status
    
    // Mode
    long    mode_current();             // Current mode
    long    mode_next();                // Next mode
    long    stor_mode();                // Store mode (STOR/RTRV/BOTH/NONE)
    long    rtrv_mode();                // Retrieve mode
    
    // Load Information
    char*   loadid();                   // Current load ID
    char*   loadid2();                  // Second load ID
    long    load_size();                // Load size
    
    // Relationships
    char*   stacker_name();             // Associated stacker
    char*   plc_name();                 // Associated PLC
    char*   next_station();             // Next station in route
    char*   xref_stand();               // Cross-reference stand
    
    // Actions
    char*   action_sr_comp();           // Action on SR complete
    char*   action_movdp_assign();      // Action on move assign
    char*   action_conv_comp();         // Action on conveyor complete
};

// Global stand control
cc_std_ctrl cc_stand_ctrl;
```

#### Stand Types (GP.ST.TYP)
| Value | Constant | Description |
|-------|----------|-------------|
| 1 | `PDIN` | Pickup/Dropoff Input |
| 2 | `PDOUT` | Pickup/Dropoff Output |
| 3 | `INFED` | Infeed station |
| 4 | `OUTFD` | Outfeed station |
| 5 | `DIVRT` | Divert station |
| 6 | `MERGE` | Merge station |
| 7 | `SRBC` | Stacker BCR |
| 8 | `PDBUF` | PD Buffer |
| 9 | `ACCUM` | Accumulation conveyor |
| 10 | `OTHER` | Other type |

---

### 3. cc_zone - Zone Class

**Files:** `cc_zone.h`, `cc_zone.cpp`

The `cc_zone` class manages tracking zones for conveyor movement.

#### Class Definition
```cpp
class cc_zone {
public:
    // Constructor
    cc_zone();
    cc_zone(cc_zone* zP, char* prefix);
    
    // Identity
    long    zone();                     // Zone number
    char*   zone_name();                // Zone name
    char*   plc_name();                 // Associated PLC
    char*   area();                     // System area
    
    // Load Tracking
    char*   loadid();                   // Current load ID
    char*   loadid(const long idx);     // Load by index (0=Current, 1=Coming)
    long    destination();              // Destination zone
    long    loaded();                   // Zone is loaded
    
    // BCR Data
    char*   loadid_1();                 // BCR read #1
    char*   loadid_2();                 // BCR read #2
    char*   loadid_3();                 // BCR read #3
    double  weight();                   // Weight scale reading
    char*   profil();                   // Profile data
    
    // Configuration
    long    group();                    // Group number
    long    drag_zone();                // Linked drag zone
    bool    real();                     // Real zone flag
    bool    simulated();                // Simulated zone
    bool    weigh_scale();              // Has weight scale
    long    Bar_Code_Scan();            // BCR scan type
    
    // Control
    bool    ok();                       // Zone is OK to move
    bool    empty();                    // Zone is empty
    void    sem_lock();                 // Lock tracking semaphore
    void    sem_unlock();               // Unlock semaphore
};

// Zone control class
class cc_zone_ctrl {
public:
    cc_zone*    Find_Zone(const long zone_name);
    cc_zone*    Find_Zone(cc_zone* ZONE);
    long        zone_add(const long zone, const char* loadid, const long dest);
    long        zone_delete(const long zone);
    long        zone_move(const long from, const long to);
    bool        zone_empty(const long zone);
    bool        zone_ok(const long zone);
};

// Global zone control
cc_zone_ctrl cc_ZONE;
```

---

### 4. cc_agv - AGV Class

**Files:** `cc_agv.h`, `cc_agv.cpp`

Manages Automatic Guided Vehicles.

#### Class Definition
```cpp
class cc_agvs {
public:
    // Operation Modes
    class OperMode {
        enum opermode { MAINTENANCE, MANUAL, LOCAL, AUTO, ERR = 9 };
    };
    
    // Vehicle Modes
    class VehicleMode {
        enum vehiclemodes { ONLINE, OFFLINE };
    };
    
    // Properties
    long    number();                   // AGV number
    char*   short_name();               // Short name
    char*   long_name();                // Long name
    long    point_no();                 // Current point
    long    loop();                     // Loop number
    
    // Status
    long    error();                    // Error code
    bool    loaded();                   // AGV is loaded
    char*   loadid();                   // Load ID on AGV
    char*   loadid_assigned();          // Assigned load ID
    long    oper_mode();                // Operation mode
    long    vehicle_mode();             // Vehicle mode
    long    ok();                       // OK to assign work
    
    // Simulation
    bool    sim();                      // Simulator mode
    long    SimuAction();               // Simulation action
    double  Simu_timer();               // Simulation timer
};

// Global AGV control
cc_agv_ctrl cc_agv;
```

---

### 5. cc_rtn - RTNX Cart Class

**Files:** `cc_rtnx.h`, `cc_rtnx.cpp`

Manages Rail-guided Transport (RTNX) carts.

#### Class Definition
```cpp
class cc_rtns {
public:
    // Cart Modes
    class CartMode {
        enum cartmode { AUTOOFF, AUTOON, ERR };
    };
    
    // Cart States
    class CartState {
        enum cartstates { OFFLINE, ONLINE };
    };
    
    // Properties
    long    number();                   // RTNX number
    char*   short_name();               // Short name
    char*   long_name();                // Long name
    long    point_no();                 // Current point
    long    loop();                     // Loop/floor number
    
    // Status
    long    error();                    // Error code
    bool    loaded();                   // RTNX is loaded
    char*   loadid();                   // Load ID
    long    cart_mode();                // Cart mode
    long    cart_state();               // Cart state
    bool    controller();               // Is controller
};

// Global RTNX control
cc_rtn_ctrl cc_rtn;
```

---

### 6. cc_plc - PLC Class

**Files:** `cc_plc.h`, `cc_plc.cpp`

Manages PLC communication and I/O points.

#### Class Definition
```cpp
class cc_plc {
public:
    // Identity
    char*   name();                     // PLC name
    long    number();                   // PLC number
    
    // Communication
    long    comm();                     // Communication status
    bool    enabled();                  // Enabled flag
    
    // I/O Access
    long    read_bit(const char* bit_name);
    void    write_bit(const char* bit_name, long value);
    long    read_register(const char* reg_name);
    void    write_register(const char* reg_name, long value);
    
    // Finder
    static cc_plc* get_plc(const char* name);
};
```

---

### 7. cc_mem - Memory Management Class

**Files:** `cc_mem.h`, `cc_mem.cpp`

Manages mapped memory (shared memory files).

#### Class Definition
```cpp
class cc_mem {
public:
    // Memory Operations
    void*   attach(const char* name, size_t size);
    void    detach(void* ptr);
    void    flush(void* ptr, size_t size);
    
    // File Management
    long    create(const char* filename, size_t size);
    long    open(const char* filename);
    void    close();
};
```

---

### 8. cc_str - String Operations Class

**Files:** `cc_str.h`, `cc_str.cpp`

**CRITICAL:** Must use `cc_str` for all string operations (not C library functions).

#### Class Definition
```cpp
static class cc_str_class {
public:
    // Copy operations
    void    copy(char* dest, const char* src, size_t destSize);
    void    copy(char* dest, size_t destSize, const char* src);
    
    // Compare operations
    int     comp(const char* str1, const char* str2);
    int     comp(const char* str1, const char* str2, size_t len);
    
    // Concatenation
    void    cat(char* dest, const char* src, size_t destSize);
    void    concat(char* dest, size_t destSize, const char* src);
    
    // Trim operations
    void    trim(char* str);
    void    rtrim(char* str);
    void    ltrim(char* str);
    
    // Case operations
    void    upper(char* str);
    void    lower(char* str);
} cc_str;
```

#### Usage Pattern
```cpp
// CORRECT - Use cc_str
char buffer[50];
cc_str.copy(buffer, sourceStr, sizeof(buffer));

// FORBIDDEN - Never use C library
// strcpy(buffer, sourceStr);  // NEVER!
```

---

### 9. cc_gg - Global Variables Class

**Files:** `cc_gg.h`, `cc_gg.cpp`

Global variables and program information.

#### Class Definition
```cpp
static class cc_gg_class {
public:
    // Program Info
    char*   prgnam();                   // Program name
    long    prgpid();                   // Process ID
    char*   area();                     // System area
    
    // Database Functions
    char*   dbfuncs();                  // Current DB function
    void    set_dbfuncs(const char* name);
    
    // Error State
    long    error();
    void    set_error(long code);
} gg;

// Global text buffers
extern char gg_text[256];
extern char gg_text2[256];
extern char gg_text3[512];
extern char gg_text4[1024];
extern char gg_text5[2048];
```

---

### 10. cc_sys - System Control Class

**Files:** `cc_sys.h`, `cc_sys.cpp`

System-level control and initialization.

---

### 11. cc_sts - Statistics Class

**Files:** `cc_sts.h`, `cc_sts.cpp`

Equipment statistics tracking.

---

### 12. cc_vehicle - Vehicle Base Class

**Files:** `cc_vehicle.h`, `cc_vehicle.cpp`

Base class for AGV and RTNX vehicles.

---

### 13. cc_mudp - MUDP Protocol Class

**Files:** `cc_mudp.h`, `cc_mudp.cpp`

MUDP (Murata UDP) communication protocol implementation.

---

### 14. cc_route - Routing Class

**Files:** `cc_route.h`, `cc_route.cpp`

Load routing logic and path finding.

---

### 15. cc_common - Common Utilities

**Files:** `cc_common.h`, `cc_common.cpp`

Common utility functions and structures.

---

## Utility Functions (cs_*)

### 1. cs_log - Logging Functions

**Files:** `CS_LOG.Cpp`, `CS_LOG.H`

#### Key Functions
```cpp
// Standard logging
void cs_log_printf(const char* prgnam, int level, const char* format, ...);
void cs_log_printf(int level, const char* format, ...);

// Log levels: 0-2 Errors, 3-4 Standard, 5-6 Verbose, 7-10 Trace
```

#### Usage Pattern
```cpp
cs_log_printf(gg.prgnam(), 3, "HstCm: Processing basket [%s], rc=%d", 
              basketId, rc);
              
// Error logging
cs_log_printf(gg.prgnam(), 0, "HstCm: ERROR - Failed to connect, rc=%d", rc);
```

---

### 2. cs_msg - Message Queue Functions

**Files:** `cs_msg.cpp`, `CS_MSG.H`

#### Key Functions
```cpp
long cs_msg_put(const char* queue, void* msg, size_t size);
long cs_msg_get(const char* queue, void* msg, size_t size);
long cs_msg_peek(const char* queue, void* msg, size_t size);
long cs_msg_count(const char* queue);
```

---

### 3. cs_reg - Registry Functions

**Files:** `cs_reg.cpp`, `cs_reg.h`

#### Key Functions
```cpp
// Read registry values
long cs_reg_read(const char* section, const char* key, char* value, size_t size);
long cs_reg_read(const char* section, const char* key, long* value, long default_val);

// Write registry values
long cs_reg_write(const char* section, const char* key, const char* value);
long cs_reg_write(const char* section, const char* key, long value);
```

---

### 4. cs_dtm - Date/Time Functions

**Files:** `cs_dtm.cpp`, `cs_dtm.h`

#### Key Functions
```cpp
double cs_dtm_get();                    // Get current timestamp
void   cs_dtm_fmt(double dtm, char* buf, size_t size);  // Format timestamp
double cs_dtm_diff(double dtm1, double dtm2);           // Time difference
```

---

### 5. cs_err - Error Handling Functions

**Files:** `cs_err.cpp`, `cs_err.h`

#### Key Functions
```cpp
void cs_err_set(long code, const char* msg);
long cs_err_get();
char* cs_err_text(long code);
```

---

### 6. cs_elt - Elements Table Functions

**Files:** `cs_elt.cpp`, `cs_elt.h`

#### Key Functions
```cpp
long cs_elt_read(const char* key, char* value, size_t size);
long cs_elt_read(const char* key, long* value, long default_val);
long cs_elt_write(const char* key, const char* value);
```

---

### 7. cs_txt - Text Translation Functions

**Files:** `cs_txt.cpp`, `CS_TXT.H`

#### Key Functions
```cpp
char* cs_txt_get(long code);            // Get text for code
char* cs_txt_get(const char* key);      // Get text by key
```

---

### 8. cs_sem - Semaphore Functions

**Files:** `CS_SEM.Cpp`, `CS_SEM.H`

#### Key Functions
```cpp
long cs_sem_create(const char* name);
long cs_sem_lock(const char* name);
long cs_sem_unlock(const char* name);
void cs_sem_cleanup(const char* prefix);
```

---

### 9. cs_mpm - Mapped Memory Functions

**Files:** `cs_mpm.cpp`, `cs_mpm.h`

#### Key Functions
```cpp
void* cs_mpm_attach();
void  cs_mpm_detach();
void  cs_mpm_flush();
```

---

### 10. cs_tmr - Timer Functions

**Files:** `cs_tmr.cpp`, `cs_tmr.h`

#### Key Functions
```cpp
double cs_tmr_start();
double cs_tmr_elapsed(double start);
bool   cs_tmr_expired(double start, double limit);
```

---

## File Summary

| Category | Files | Description |
|----------|-------|-------------|
| **Equipment Classes** | 14 | cc_stk, cc_std, cc_zone, cc_agv, cc_rtn, cc_plc... |
| **Utility Classes** | 6 | cc_str, cc_gg, cc_mem, cc_sys, cc_sts, cc_route |
| **Protocol Classes** | 4 | cc_mudp, cc_vehicle, cc_coms, cc_server |
| **Utility Functions** | 16 | cs_log, cs_msg, cs_reg, cs_dtm, cs_err, cs_elt... |
| **Total** | ~40 files | Complete control layer |

---

## Dependencies

### Required Headers
```cpp
#include <global_prm.h>    // Global parameters
#include <cc_gg.h>         // Global variables
#include <cc_str.h>        // String operations (REQUIRED)
#include <cs_log.h>        // Logging
```

---

## Shared Memory Version Control

Each class has a version constant that must be updated when memory layout changes:

```cpp
#define stk_version 14     // cc_stk version
#define std_version 29     // cc_std version
#define zone_version 9     // cc_zone version
```

**IMPORTANT:** Incrementing version invalidates existing shared memory files and forces reinitialization.

---

## Related Documentation

- [DSUB Library](dsub.md) - Database subroutines
- [CSUB Functions](csub.md) - Additional utility functions
- [OSUB Library](osub.md) - VB.NET database access
- [global_prm.h Reference](global_prm.md)
- [ICISDefines.vb Reference](ICISDefines.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Dispatcher | [p_ar_stkdp](../01_Background_Processes/p_ar_stkdp.md) | Uses cc_stk |
| Move Dispatcher | [p_ar_movdp](../01_Background_Processes/p_ar_movdp.md) | Uses cc_std |
| Stacker Communications | [p_cc_stkcmx](../01_Background_Processes/p_cc_stkcmx.md) | Uses cc_mudp |
| Database Access | [DSUB Library](dsub.md) | ds_* functions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2024-12-23 | Comprehensive deep documentation |
| 1.0 | 2024-12-22 | Initial creation |
