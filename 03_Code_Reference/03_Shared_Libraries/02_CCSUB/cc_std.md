# cc_std - Stand Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_std.cpp` / `cc_std.h` |
| **Library** | CCSUB (Common Control Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Core Equipment Control |

---

## Overview

The `cc_std` class provides stand/station control functionality for the MHC/MEM system. It manages station status, mode changes, load presence detection, and error handling for all station types including P&D stations, conveyor blocks, manual stations, and AGV/RTNX stands.

**Key Responsibilities:**
- Station status monitoring and management
- Mode change handling (Store, Retrieve, Normal, etc.)
- Load presence/absence detection
- Error flag management
- Station communication status
- Arrival/departure handling

---

## Class Definition

```cpp
class cc_std {
private:
    int     station_id;           // Station identifier
    char    station_name[32];     // Station name
    int     station_type;         // Station type (GP.ST.TYP)
    int     current_mode;         // Current station mode
    int     error_code;           // Current error code
    bool    load_present;         // Load presence flag
    bool    comm_online;          // Communication status
    
public:
    cc_std();
    ~cc_std();
    
    // Initialization
    long init(int stationId);
    long init_from_db(const char* stationName);
    
    // Status Methods
    long get_status(STTN* sttn);
    long set_status(int status);
    long get_mode();
    long set_mode(int mode);
    
    // Load Management
    bool is_load_present();
    long set_load_present(bool present);
    long signal_arrival();
    long signal_departure();
    
    // Error Management
    long get_error_code();
    long set_error(int errorCode);
    long clear_error();
    bool has_error();
    
    // Communication
    bool is_online();
    long set_online(bool online);
    
    // Mode Operations
    long change_mode(int newMode);
    bool can_accept_store();
    bool can_accept_retrieve();
};
```

---

## Key Methods

### cc_std::init

**Signature:**
```cpp
long cc_std::init(int stationId);
```

**Purpose:** Initialize the station control object from station ID.

| Parameter | Type | Description |
|-----------|------|-------------|
| `stationId` | `int` | Station identifier from database |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Initialization successful |
| `GP.BAD` | Station not found or invalid |

**Logic Flow:**
1. Validate station ID
2. Load station record from MHC_STTN table
3. Initialize internal state variables
4. Set communication status based on PLC state

---

### cc_std::get_mode / cc_std::set_mode

**Signature:**
```cpp
long cc_std::get_mode();
long cc_std::set_mode(int mode);
```

**Purpose:** Get or set station operating mode.

| Mode Value | Constant | Description |
|------------|----------|-------------|
| 0 | `GP.ST.MODES.NORMAL` | Normal operation |
| 1 | `GP.ST.MODES.STORE` | Store mode only |
| 2 | `GP.ST.MODES.RETRIEVE` | Retrieve mode only |
| 3 | `GP.ST.MODES.MOVE` | Station-to-station move |
| 4 | `GP.ST.MODES.CELL2CELL` | Cell-to-cell shuffle |
| 5 | `GP.ST.MODES.FORCE_RETRIEVE` | Forced retrieve |
| 6 | `GP.ST.MODES.FORCE_STORE` | Forced store |

---

### cc_std::signal_arrival

**Signature:**
```cpp
long cc_std::signal_arrival();
```

**Purpose:** Signal load arrival at station, triggering arrival processing.

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Arrival signaled successfully |
| `GP.BAD` | Error signaling arrival |

**Logic Flow:**
1. Set load_present flag to true
2. Update station record in database
3. Trigger arrival processing in p_ar_arive
4. Log arrival event

---

### cc_std::signal_departure

**Signature:**
```cpp
long cc_std::signal_departure();
```

**Purpose:** Signal load departure from station.

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Departure signaled successfully |
| `GP.BAD` | Error signaling departure |

**Logic Flow:**
1. Set load_present flag to false
2. Update station record in database
3. Clear any pending station errors
4. Log departure event

---

### cc_std::set_error / cc_std::clear_error

**Signature:**
```cpp
long cc_std::set_error(int errorCode);
long cc_std::clear_error();
```

**Purpose:** Set or clear station error condition.

| Error Code | Constant | Description |
|------------|----------|-------------|
| 105 | `ERR_TIME_OUT_BCR` | Bar code no-read |
| 107 | `ERR_UNKNOWN_LOAD` | Unknown load at station |
| 109 | `ERR_NO_LOCATION` | No storage locations found |
| 110 | `ERR_DUP_ARRIVE` | Duplicate arrival |
| 117 | `ERR_COMM_ERROR` | Communications offline |

---

### cc_std::change_mode

**Signature:**
```cpp
long cc_std::change_mode(int newMode);
```

**Purpose:** Change station operating mode with validation.

| Parameter | Type | Description |
|-----------|------|-------------|
| `newMode` | `int` | New mode value (GP.ST.MODES) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Mode changed successfully |
| `GP.BAD` | Invalid mode or mode change not allowed |

**Logic Flow:**
1. Validate new mode value
2. Check if mode change is allowed (no active moves)
3. Update station record
4. Notify dispatchers of mode change
5. Log mode change event

---

## Common Usage Patterns

### Pattern 1: Initialize Station and Check Status

```cpp
cc_std station;

// Initialize from station ID
if (station.init(stationId) == GP.GOOD) {
    // Check current mode
    int mode = station.get_mode();
    cs_log_printf(FILELINE, GP_LOGLVL_3, "Station mode: %s", 
                  GP.ST_MODES_idx(mode));
    
    // Check load presence
    if (station.is_load_present()) {
        cs_log_printf(FILELINE, GP_LOGLVL_3, "Load present at station");
    }
}
```

### Pattern 2: Handle Arrival Processing

```cpp
cc_std station;
station.init(stationId);

// Signal arrival when load detected
if (plc_load_present(stationId)) {
    if (station.signal_arrival() == GP.GOOD) {
        cs_log_printf(FILELINE, GP_LOGLVL_3, 
                      "Arrival signaled for station %d", stationId);
    }
}
```

### Pattern 3: Mode Change with Validation

```cpp
cc_std station;
station.init(stationId);

// Request mode change to Store Only
if (station.change_mode(GP.ST.MODES.STORE) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, 
                  "Station mode changed to Store");
} else {
    cs_log_printf(FILELINE, GP_LOGLVL_1, 
                  "Mode change failed - active moves pending");
}
```

### Pattern 4: Error Handling

```cpp
cc_std station;
station.init(stationId);

// Check for errors
if (station.has_error()) {
    int errCode = station.get_error_code();
    cs_log_printf(FILELINE, GP_LOGLVL_1, 
                  "Station error: %d", errCode);
    
    // Attempt error recovery
    if (errCode == GP.ST.std_err.ERR_COMM_ERROR) {
        // Attempt reconnection
        station.set_online(true);
        station.clear_error();
    }
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Station Table | [os_sttn.md](../01_OSUB/os_sttn.md) | STTN database operations |
| Station Constants | [Station_Constants.md](../05_ICISDefines/Station_Constants.md) | ST.TYP, ST.MODES |
| Stacker Control | [cc_stk.md](cc_stk.md) | Related equipment control |
| Error Codes | [Error_Codes.md](../05_ICISDefines/Error_Codes.md) | Station error codes |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

