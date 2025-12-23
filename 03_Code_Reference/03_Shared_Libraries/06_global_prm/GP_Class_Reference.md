# GP Class Reference - global_prm

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `global_prm.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\incl\global_prm.h` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `global_parms` class (instantiated as `GP`) provides an object-oriented interface to system constants. Access via static class instance `GP`.

---

## Basic Constants (GP Class)

```cpp
static const long NO = 0;
static const long YES = 1;
static const long ASK = 2;
static const long OFF = 0;
static const long ON = 1;
static const long PEND = 2;
static const long MATCH = 0;
static const long LESS = -1;
static const long GREATER = 1;
static const long GOOD = 0;
static const long BAD = -1;
static const long DBHELD = -1;
static const long UGLY = 1;
static const long BETTER = 2;
static const long EMPTY = 2;
static const long FULL = 3;
static const long FAILED = -2;
static const long TIME_OUT = -3;
static const long Lets_TRY_AGAIN = -4;
static const long RACK_FULL = -5;
static const long RACK_REALLY_FULL = -6;
```

---

## String Methods (GP Class)

| Method | Returns | Description |
|--------|---------|-------------|
| `GP.YES_STR()` | `"Yes"` | Yes string |
| `GP.NO_STR()` | `"No"` | No string |
| `GP.ON_STR()` | `"On"` | On string |
| `GP.OFF_STR()` | `"Off"` | Off string |
| `GP.PEND_STR()` | `"Pend"` | Pending string |
| `GP.NORMAL()` | `"Normal"` | Normal string |
| `GP.RESET()` | `"Reset"` | Reset string |
| `GP.CANCEL_STR()` | `"Can"` | Cancel string |

---

## Index Lookup Functions

The `global_parms` class provides index-to-string lookup functions:

```cpp
// Vehicle function index to name
char* name = GP.VEHICLE_FUNC_idx(GP.VEHICLE.FUNC.MOVE);  // Returns "MOVE"

// Vehicle function name to index
long idx = GP.VEHICLE_FUNC_idx("MOVE");  // Returns GP.VEHICLE.FUNC.MOVE

// Stacker function index to name
char* name = GP.SR_FUNC_idx(GP.SR.FUNC.STOR);  // Returns "STOR"

// Station type index to name
char* name = GP.ST_TYPE_idx(GP.ST.TYP.PDIN);  // Returns "PD-IN"

// Station mode index to name
char* name = GP.ST_MODES_idx(GP.ST.MODES.STORE);  // Returns "Store"
```

---

## GP Subclasses

### GP.VEHICLE

See [Equipment_Parameters.md](Equipment_Parameters.md) for complete VEHICLE reference.

### GP.SR

See [Equipment_Parameters.md](Equipment_Parameters.md) for complete SR reference.

### GP.ST

See [Equipment_Parameters.md](Equipment_Parameters.md) for complete ST reference.

### GP.MOVS

See [Move_Parameters.md](Move_Parameters.md) for complete MOVS reference.

---

## Usage Examples

### Checking Return Codes

```cpp
int result = performDatabaseOperation();
switch (result) {
    case GP.GOOD:
        cs_log_printf(GP_LOGLVL_3, "Operation successful");
        break;
    case GP.BAD:
        cs_log_printf(GP_LOGLVL_1, "Operation failed");
        break;
    case GP.RACK_FULL:
        cs_log_printf(GP_LOGLVL_2, "No storage space");
        break;
}
```

### Using Vehicle Functions

```cpp
// Set vehicle function
vehicle.function = GP.VEHICLE.FUNC.MOVE;
char* funcName = GP.VEHICLE_FUNC_idx(vehicle.function);
cs_log_printf(GP_LOGLVL_3, "Vehicle function: %s", funcName);

// Check schedule
if (vehicle.schedule == GP.VEHICLE.SCHD.NBSY) {
    // Vehicle is available
}
```

### Station Mode Handling

```cpp
// Set station mode
station.mode = GP.ST.MODES.STORE;

// Get mode string for logging
char* modeName = GP.ST_MODES_idx(station.mode);
cs_log_printf(GP_LOGLVL_3, "Station mode: %s", modeName);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Equipment Parameters | [Equipment_Parameters.md](Equipment_Parameters.md) | GP.VEHICLE, GP.SR, GP.ST |
| Move Parameters | [Move_Parameters.md](Move_Parameters.md) | GP.MOVS |
| VB.NET Equivalent | [ICISDefines.md](../ICISDefines.md) | Must match |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

