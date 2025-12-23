# cc_plc - PLC Communication Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_plc.cpp`, `cc_plc.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_plc` |
| **Controller Class** | `cc_plc_ctrl` (manages all PLCs) |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_plc` module provides **PLC communication abstraction** for reading and writing PLC I/O points via Kepware OPC server or direct protocols. Supports multiple PLC brands (Allen Bradley, Mitsubishi, etc.) through a unified interface.

**Critical Operations:**
- PLC tag reading/writing
- Connection management
- Register and coil access
- Error handling and retry logic

**PLC Naming Convention:**
- PLCs are named `PLC001`, `PLC002`, etc.

**Communication Methods:**
- Kepware OPC Server (primary)
- Direct protocol (Mitsubishi, Allen Bradley)

---

## Key Classes

### cc_plc

**Purpose:** Individual PLC instance

**Key Methods:**

#### Connection Methods

```cpp
long Connect();                    // Connect to PLC
long Disconnect();                 // Disconnect from PLC
long GetConnectionStatus();        // Get connection status (ONLINE/OFFLINE)
long Reconnect();                  // Reconnect to PLC
```

#### Tag Read/Write Methods

```cpp
long ReadTag(const char* tagName, void* value, long dataType);  // Read OPC tag
long WriteTag(const char* tagName, void* value, long dataType); // Write OPC tag
long ReadRegister(long address, long* value);                    // Read register
long WriteRegister(long address, long value);                   // Write register
long ReadCoil(long address, bool* value);                        // Read coil (bit)
long WriteCoil(long address, bool value);                        // Write coil (bit)
```

#### Status Methods

```cpp
char* GetPLCName();               // Get PLC name (e.g., "PLC001")
long GetError();                  // Get error flag
long GetErrorCode();              // Get error code
char* GetErrorDescription();      // Get error description
```

#### Initialization

```cpp
long Initialize(const char* plcName);  // Initialize PLC instance
```

---

### cc_plc_ctrl

**Purpose:** Controller class that manages all PLC instances

**Key Methods:**

```cpp
cc_plc* Find_PLC(const char* plcName);  // Find PLC by name
long GetCount();                        // Get total PLC count
```

**Usage:**
```cpp
cc_plc* PLC = cc_plc_ctrl.Find_PLC("PLC001");
if (PLC) {
    if (PLC->GetConnectionStatus() == GP.COM.ONLI) {
        // PLC is online
    }
}
```

---

## Common Usage Patterns

### Pattern 1: Read PLC Tag

```cpp
cc_plc* PLC = cc_plc_ctrl.Find_PLC("PLC001");
if (PLC == NULL) return GP.BAD;

if (PLC->GetConnectionStatus() != GP.COM.ONLI) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "PLC001 is offline");
    return GP.BAD;
}

bool conveyorStart = false;
if (PLC->ReadTag("Channel1.Device1.Conveyor_Start", &conveyorStart, GP.DATATYPE.BOOL) == GP.GOOD) {
    if (conveyorStart) {
        cs_log_printf(FILELINE, CS_LOG_INFO, "Conveyor start signal is ON");
    }
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to read PLC tag");
    return GP.BAD;
}
```

### Pattern 2: Write PLC Coil

```cpp
cc_plc* PLC = cc_plc_ctrl.Find_PLC("PLC001");
if (PLC == NULL) return GP.BAD;

bool startCommand = true;
if (PLC->WriteCoil(100, startCommand) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Sent start command to PLC001 coil 100");
    return GP.GOOD;
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to write PLC coil");
    return GP.BAD;
}
```

### Pattern 3: Read PLC Register

```cpp
cc_plc* PLC = cc_plc_ctrl.Find_PLC("PLC002");
if (PLC == NULL) return GP.BAD;

long statusCode = 0;
if (PLC->ReadRegister(200, &statusCode) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "PLC002 register 200 = %ld", statusCode);
    return GP.GOOD;
} else {
    return GP.BAD;
}
```

### Pattern 4: Handle PLC Connection Loss

```cpp
cc_plc* PLC = cc_plc_ctrl.Find_PLC("PLC001");
if (PLC == NULL) return GP.BAD;

if (PLC->GetConnectionStatus() != GP.COM.ONLI) {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "PLC001 connection lost, attempting reconnect");
    
    if (PLC->Reconnect() == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_INFO, "PLC001 reconnected successfully");
        return GP.GOOD;
    } else {
        cs_log_printf(FILELINE, CS_LOG_ERROR, "PLC001 reconnect failed");
        return GP.BAD;
    }
}
```

---

## Data Types

| Data Type | Constant | Description |
|-----------|----------|-------------|
| `BOOL` | `GP.DATATYPE.BOOL` | Boolean (true/false) |
| `INT` | `GP.DATATYPE.INT` | Integer (16-bit) |
| `LONG` | `GP.DATATYPE.LONG` | Long integer (32-bit) |
| `FLOAT` | `GP.DATATYPE.FLOAT` | Floating point |
| `STRING` | `GP.DATATYPE.STRING` | String |

---

## Related Tables

| Table | Relationship | Description |
|-------|--------------|-------------|
| `MHC_STTN` | Reference | Stations controlled by PLCs |
| Kepware Config | Synchronized | OPC tag definitions |

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| PLC Communications | [p_cc_kepcm](../01_Background_Processes/p_cc_kepcm.md) | Uses cc_plc |
| PLC Mapping | [PLC and Communication Mapping](../../04_Database_Reference/02_PLC_Mapping.md) | Tag Definitions |
| Kepware Configuration | [KEPWAREplcdef.xml](../../04_Database_Reference/02_PLC_Mapping.md) | Tag Mapping |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

