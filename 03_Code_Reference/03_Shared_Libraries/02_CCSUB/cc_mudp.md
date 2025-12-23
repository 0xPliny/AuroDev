# cc_mudp - MUDP Protocol Communication Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_mudp.cpp`, `cc_mudp.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_mudp` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_mudp` module handles **MUDP (Murata UDP) protocol communication** for RTNX and AGV vehicles. MUDP is a UDP-based protocol that supports multi-cycle operations, allowing vehicles to perform complex load transfer sequences across multiple data points (forks).

**Critical Operations:**
- Message encoding/decoding
- Multi-cycle protocol support (4-step cycles)
- Error recovery and retry logic
- Timeout handling
- Response validation

**Protocol Features:**
- UDP-based communication
- Multi-cycle support (steps 1-4)
- Sub-function tracking
- Error level reporting
- Heartbeat/keepalive

**Used By:**
- `p_cc_mudpcm` (Vehicle Communication Process)

---

## Key Classes

### cc_mudp

**Purpose:** MUDP protocol handler

**Key Methods:**

#### Message Encoding/Decoding

```cpp
long EncodeMessage(const MUDP_Message* msg, char* buffer, size_t bufferSize);
long DecodeMessage(const char* buffer, size_t bufferSize, MUDP_Message* msg);
```

**Description:**
Encodes/decodes MUDP protocol messages to/from binary format for UDP transmission.

#### Message Transmission

```cpp
long SendMessage(const char* vehicleIP, int port, const MUDP_Message* msg);
long ReceiveMessage(const char* vehicleIP, int port, MUDP_Message* msg, long timeout);
```

**Description:**
Sends and receives MUDP messages via UDP sockets.

#### Multi-Cycle Support

```cpp
long BuildMultiCycleCommand(long step, long subFunction, const char* loadId, 
                            long fromPoint, long toPoint, MUDP_Message* msg);
long ParseMultiCycleResponse(const MUDP_Message* msg, long* step, long* subFunction, 
                             long* status, long* errorCode);
```

**Description:**
Builds and parses multi-cycle commands/responses for 4-step load transfer operations.

#### Error Handling

```cpp
long GetErrorLevel(const MUDP_Message* msg);
long GetErrorCode(const MUDP_Message* msg);
char* GetErrorDescription(long errorCode);
```

**Description:**
Extracts error information from MUDP messages.

#### Connection Management

```cpp
long Connect(const char* vehicleIP, int port);
long Disconnect();
long IsConnected();
long Reconnect();
```

**Description:**
Manages UDP socket connections to vehicles.

---

## MUDP Message Structure

```cpp
typedef struct MUDP_Message {
    long messageType;        // Message type (COMMAND, RESPONSE, STATUS)
    long vehicleId;          // Vehicle identifier
    long step;               // Current step (1-4)
    long subFunction;        // Sub-function code
    char loadId[24];         // Load ID
    long fromPoint;          // Source point
    long toPoint;            // Destination point
    long status;             // Status code
    long errorCode;          // Error code
    long errorLevel;         // Error level (MUDP specific)
    char timestamp[24];      // Message timestamp
} MUDP_Message;
```

---

## Common Usage Patterns

### Pattern 1: Send Multi-Cycle Command

```cpp
cc_mudp mudp;
MUDP_Message msg;

// Build step 1 command: Pick up at station
if (mudp.BuildMultiCycleCommand(1, GP.SUBFUNC.PICK, loadId, 
                                stationPoint, nextPoint, &msg) == GP.GOOD) {
    
    // Send to vehicle
    if (mudp.SendMessage(vehicleIP, MUDP_PORT, &msg) == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_INFO, "Sent step 1 command to %s", vehicleName);
    } else {
        cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to send MUDP message");
        return GP.BAD;
    }
}
```

### Pattern 2: Receive and Parse Response

```cpp
cc_mudp mudp;
MUDP_Message response;

// Receive response with timeout
if (mudp.ReceiveMessage(vehicleIP, MUDP_PORT, &response, 5000) == GP.GOOD) {
    long step, subFunction, status, errorCode;
    
    if (mudp.ParseMultiCycleResponse(&response, &step, &subFunction, 
                                     &status, &errorCode) == GP.GOOD) {
        if (errorCode != 0) {
            cs_log_printf(FILELINE, CS_LOG_ERROR, "Vehicle error: %s", 
                          mudp.GetErrorDescription(errorCode));
            return GP.BAD;
        }
        
        if (status == GP.MUDP.STATUS.COMPLETE) {
            cs_log_printf(FILELINE, CS_LOG_INFO, "Step %ld complete", step);
            return GP.GOOD;
        }
    }
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Timeout waiting for MUDP response");
    return GP.BAD;
}
```

### Pattern 3: Handle Multi-Cycle Progression

```cpp
cc_mudp mudp;
MUDP_Message msg;

// Step 1: Pick up
mudp.BuildMultiCycleCommand(1, GP.SUBFUNC.PICK, loadId, stationPoint, intermediatePoint, &msg);
mudp.SendMessage(vehicleIP, MUDP_PORT, &msg);
// Wait for completion...

// Step 2: Move to intermediate
mudp.BuildMultiCycleCommand(2, GP.SUBFUNC.MOVE, loadId, intermediatePoint, finalPoint, &msg);
mudp.SendMessage(vehicleIP, MUDP_PORT, &msg);
// Wait for completion...

// Step 3: Drop off
mudp.BuildMultiCycleCommand(3, GP.SUBFUNC.DROP, loadId, finalPoint, 0, &msg);
mudp.SendMessage(vehicleIP, MUDP_PORT, &msg);
// Wait for completion...

// Step 4: Return to home
mudp.BuildMultiCycleCommand(4, GP.SUBFUNC.HOME, "", finalPoint, homePoint, &msg);
mudp.SendMessage(vehicleIP, MUDP_PORT, &msg);
```

---

## MUDP Sub-Functions

| Sub-Function | Constant | Description |
|--------------|----------|-------------|
| `PICK` | `GP.SUBFUNC.PICK` | Pick up load |
| `DROP` | `GP.SUBFUNC.DROP` | Drop off load |
| `MOVE` | `GP.SUBFUNC.MOVE` | Move without load |
| `HOME` | `GP.SUBFUNC.HOME` | Return to home position |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Vehicle Control | [cc_vehicle.md](cc_vehicle.md) | Uses cc_mudp |
| Vehicle Communications | [p_cc_mudpcm](../01_Background_Processes/p_cc_mudpcm.md) | MUDP Process |
| Multi-Cycle Operations | [Workflows](../../05_Workflows/01_Inbound_Move.md) | MUDP Cycles |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

