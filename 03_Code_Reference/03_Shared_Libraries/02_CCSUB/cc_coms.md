# cc_coms - Communication Utilities Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_coms.cpp`, `cc_coms.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_coms` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_coms` module provides **communication utilities** for various protocols and communication methods used in the MHC system. This includes serial communication, network communication, and protocol-specific helpers.

**Critical Operations:**
- Serial port communication
- Network socket management
- Protocol message helpers
- Connection status monitoring

**Supported Protocols:**
- Serial (RS-232, RS-485)
- TCP/IP (Ethernet)
- UDP (for MUDP, etc.)

---

## Key Classes

### cc_coms

**Purpose:** Communication utilities

**Key Methods:**

#### Serial Communication

```cpp
long OpenSerialPort(const char* portName, long baudRate, long parity, 
                    long dataBits, long stopBits);
long CloseSerialPort(long portHandle);
long ReadSerial(long portHandle, char* buffer, size_t bufferSize, long timeout);
long WriteSerial(long portHandle, const char* data, size_t dataSize);
long GetSerialStatus(long portHandle);
```

**Description:**
Manages serial port communication for equipment that uses RS-232 or RS-485.

#### Network Communication

```cpp
long OpenSocket(const char* host, int port, long socketType);  // TCP or UDP
long CloseSocket(long socketHandle);
long ReadSocket(long socketHandle, char* buffer, size_t bufferSize, long timeout);
long WriteSocket(long socketHandle, const char* data, size_t dataSize);
long GetSocketStatus(long socketHandle);
```

**Description:**
Manages TCP/IP and UDP socket communication.

#### Connection Status

```cpp
long IsConnected(long handle);
long GetConnectionStatus(long handle);
long Reconnect(long handle);
```

**Description:**
Monitors and manages connection status.

---

## Common Usage Patterns

### Pattern 1: Serial Port Communication

```cpp
cc_coms coms;
long portHandle = coms.OpenSerialPort("COM3", 9600, GP.PARITY.NONE, 8, 1);

if (portHandle > 0) {
    char command[100];
    cc_str.copy(command, sizeof(command), "START\r\n");
    
    if (coms.WriteSerial(portHandle, command, strlen(command)) == GP.GOOD) {
        char response[256];
        if (coms.ReadSerial(portHandle, response, sizeof(response), 1000) == GP.GOOD) {
            cs_log_printf(FILELINE, CS_LOG_INFO, "Response: %s", response);
        }
    }
    
    coms.CloseSerialPort(portHandle);
}
```

### Pattern 2: TCP Socket Communication

```cpp
cc_coms coms;
long socketHandle = coms.OpenSocket("192.168.1.100", 502, GP.SOCKET.TCP);

if (socketHandle > 0) {
    if (coms.IsConnected(socketHandle)) {
        char data[256];
        // Prepare data...
        
        if (coms.WriteSocket(socketHandle, data, strlen(data)) == GP.GOOD) {
            char response[256];
            if (coms.ReadSocket(socketHandle, response, sizeof(response), 2000) == GP.GOOD) {
                // Process response
            }
        }
    }
    
    coms.CloseSocket(socketHandle);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| MUDP Communication | [cc_mudp.md](cc_mudp.md) | Uses cc_coms |
| PLC Communication | [cc_plc.md](cc_plc.md) | Network Communication |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

