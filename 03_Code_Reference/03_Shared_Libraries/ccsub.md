# CCSUB - Common Control Subroutines

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub`

**Description:** CCSUB (Common Control Subroutines) provides C++ classes for equipment control, communication, and system management. These classes encapsulate device-specific logic for stackers, vehicles, PLCs, and other equipment types.

---

## Overview

CCSUB classes follow an object-oriented design pattern with static class instances. Key classes include:
- `cc_stk`: Stacker crane control
- `cc_vehicle`: AGV/RTNX vehicle control  
- `cc_mudp`: MUDP protocol communication
- `cc_plc`: PLC communication abstraction
- `cc_str`: String manipulation utilities
- `cc_sys`: System-level control
- `cc_mem`: Mapped memory management

---

## cc_str - String Manipulation Class

**Source:** `cc_str.h`, `cc_str.cpp`

**Description:** Provides safe string manipulation functions to replace standard C library functions. All functions include buffer size checking to prevent overflows.

**Class Instance:** `cc_str` (global static instance)

### `cc_str.copy`

**Signature:**
```cpp
char *copy(char *to_str, size_t sizeInBytes, const char *from_str)
```

**Description:**
Safely copies a string from source to destination with buffer size checking. Equivalent to `strncpy` but safer.

**Parameters:**
- `to_str` (char*): Destination buffer
- `sizeInBytes` (size_t): Size of destination buffer in bytes
- `from_str` (const char*): Source string to copy

**Return Values:**
- Returns pointer to `to_str` on success
- Always null-terminates the destination string

**Logic Flow:**
1. Copies characters from source to destination
2. Ensures destination is null-terminated
3. Respects buffer size limits

**Dependencies:**
- Standard C library: `memcpy`, `strlen`

**Usage Example:**
```cpp
char basketId[BASKET_ID_LEN];
cc_str.copy(basketId, sizeof(basketId), sourceBasketId);
```

**Source Files:**
- `cc_str.h` (line 45)
- `cc_str.cpp` (implementation)

---

### `cc_str.comp`

**Signature:**
```cpp
long comp(const char *s1, const char *s2)
```

**Description:**
Compares two strings. Equivalent to `strcmp` but returns MHC standard return codes.

**Parameters:**
- `s1` (const char*): First string
- `s2` (const char*): Second string

**Return Values:**
- `GP_MATCH` (0): Strings are equal
- Non-zero: Strings differ

**Dependencies:**
- Standard C library: `strcmp`

**Usage Example:**
```cpp
if (cc_str.comp(status, "ACTIVE") == GP_MATCH) {
    // Status is active
}
```

**Source Files:**
- `cc_str.h` (line 39)
- `cc_str.cpp` (implementation)

---

### `cc_str.concat`

**Signature:**
```cpp
char *concat(char *to, size_t sizeInBytes, const char *from)
```

**Description:**
Safely concatenates a string to the end of another string with buffer size checking. Equivalent to `strncat` but safer.

**Parameters:**
- `to` (char*): Destination buffer (must be null-terminated)
- `sizeInBytes` (size_t): Size of destination buffer
- `from` (const char*): Source string to append

**Return Values:**
- Returns pointer to `to` on success
- Always null-terminates the result

**Dependencies:**
- Standard C library: `strlen`, `strncat`

**Usage Example:**
```cpp
char message[256];
cc_str.copy(message, sizeof(message), "Error: ");
cc_str.concat(message, sizeof(message), errorText);
```

**Source Files:**
- `cc_str.h` (line 48)
- `cc_str.cpp` (implementation)

---

### `cc_str.set`

**Signature:**
```cpp
char *set(char *str, char ch, long count)
```

**Description:**
Sets a string buffer to a specific character for a specified count. Useful for initializing buffers.

**Parameters:**
- `str` (char*): String buffer to set
- `ch` (char): Character to fill with
- `count` (long): Number of characters to set

**Return Values:**
- Returns pointer to `str`

**Usage Example:**
```cpp
char buffer[100];
cc_str.set(buffer, ' ', 100);  // Fill with spaces
buffer[99] = '\0';  // Null terminate
```

**Source Files:**
- `cc_str.h` (line 54)
- `cc_str.cpp` (implementation)

---

### `cc_str.isblank`

**Signature:**
```cpp
GP_BOOL isblank(const char *str)
```

**Description:**
Checks if a string is blank (empty or contains only whitespace).

**Parameters:**
- `str` (const char*): String to check

**Return Values:**
- `TRUE` (1): String is blank
- `FALSE` (0): String contains non-whitespace characters

**Usage Example:**
```cpp
if (cc_str.isblank(userInput)) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "User input is blank");
    return GP.BAD;
}
```

**Source Files:**
- `cc_str.h` (line 65)
- cc_str.cpp (implementation)

---

### `cc_str.upper` / `cc_str.lower`

**Signature:**
```cpp
char *upper(char *str)
char *lower(char *str)
```

**Description:**
Converts a string to uppercase or lowercase in place.

**Parameters:**
- `str` (char*): String to convert (modified in place)

**Return Values:**
- Returns pointer to `str`

**Dependencies:**
- Standard C library: `toupper`, `tolower`

**Usage Example:**
```cpp
char status[20];
cc_str.copy(status, sizeof(status), "pending");
cc_str.upper(status);  // Now "PENDING"
```

**Source Files:**
- `cc_str.h` (lines 78-79)
- `cc_str.cpp` (implementation)

---

### `cc_str.atoi`

**Signature:**
```cpp
long atoi(char *in_ptr, long length)
```

**Description:**
Converts a string to an integer, reading only the specified number of characters.

**Parameters:**
- `in_ptr` (char*): String to convert
- `length` (long): Number of characters to read

**Return Values:**
- Integer value of the string

**Dependencies:**
- Standard C library: `atol`, `memcpy`

**Usage Example:**
```cpp
char numStr[10] = "12345";
long value = cc_str.atoi(numStr, 5);  // Returns 12345
```

**Source Files:**
- `cc_str.h` (line 83)
- `cc_str.cpp` (lines 67-88)

---

### Additional cc_str Functions

See `cc_str.h` for complete function list including:
- `ncomp`: Compare with length
- `ncopy`: Copy with length
- `token`: String tokenization
- `clip`: Remove trailing spaces
- `findpat`: Find pattern in string
- `replace`: Replace characters/strings
- `isnumeric`: Check if string is numeric
- `sanitize`: Sanitize input strings
- `sqlSafetyCleanUp`: SQL injection prevention

---

## cc_stk - Stacker Crane Control Class

**Source:** `cc_stk.h`, `cc_stk.cpp`

**Description:** Provides control and status management for stacker crane devices. Manages communication, scheduling, functions, and error handling for stackers.

**Key Properties:**
- Communication status (`comm`)
- Schedule status (`schedule`)
- Current function (`func`)
- Control function (`func_ctrl`)
- Error codes and status
- Fork cycle completion tracking
- Multi-step operation support

**Usage:**
Stacker crane control is typically accessed through mapped memory structures. The `cc_stk` class provides methods to read/write stacker properties and manage crane operations.

**Source Files:**
- `cc_stk.h` (lines 1-941)
- `cc_stk.cpp` (implementation)

---

## cc_vehicle - Vehicle Control Class

**Source:** `cc_vehicle.h`, `cc_vehicle.cpp`

**Description:** Provides control and status management for AGV and RTNX vehicles. Handles vehicle pathing, scheduling, and multi-cycle operations.

**Key Properties:**
- Vehicle identification
- Current point/location
- Schedule status
- Function and control function
- Route status
- Error handling

**Usage:**
Vehicle control integrates with MUDP protocol for communication and uses the Points class for pathing information.

**Source Files:**
- `cc_vehicle.h`
- `cc_vehicle.cpp`

---

## cc_mudp - MUDP Protocol Communication Class

**Source:** `cc_mudp.h`, `cc_mudp.cpp`

**Description:** Handles MUDP (Murata UDP) protocol communication for RTNX and AGV vehicles. Manages message formatting, transmission, and response handling.

**Key Features:**
- Message encoding/decoding
- Multi-cycle protocol support
- Error recovery
- Timeout handling

**Usage:**
Used by vehicle communication processes (`p_cc_mudpcm`) to send commands and receive status from vehicles.

**Source Files:**
- `cc_mudp.h`
- `cc_mudp.cpp`

---

## cc_plc - PLC Communication Class

**Source:** `cc_plc.h`, `cc_plc.cpp`

**Description:** Provides abstraction layer for PLC communication via Kepware OPC or direct protocols. Manages tag reading/writing and connection status.

**Key Features:**
- OPC tag access
- Register/coil mapping
- Connection management
- Error handling

**Usage:**
Used by PLC communication processes to read/write PLC I/O points.

**Source Files:**
- `cc_plc.h`
- `cc_plc.cpp`

---

## cc_sys - System Control Class

**Source:** `cc_sys.h`, `cc_sys.cpp`

**Description:** Provides system-wide control and status management. Handles warm start, system state, and global configuration.

**Key Features:**
- System initialization
- Warm start coordination
- Global state management
- Error tracking

**Source Files:**
- `cc_sys.h`
- `cc_sys.cpp`

---

## cc_mem - Mapped Memory Management Class

**Source:** `cc_mem.h`, `cc_mem.cpp`

**Description:** Manages memory-mapped files for inter-process communication. Provides access to shared memory structures.

**Key Features:**
- Memory mapping
- Shared memory access
- File persistence
- Synchronization

**Usage:**
Used extensively for real-time data sharing between MEM processes.

**Source Files:**
- `cc_mem.h`
- `cc_mem.cpp`

---

## Additional CCSUB Classes

- `cc_gg`: Global globals and system-wide variables
- `cc_prc`: Process management
- `cc_route`: Routing and pathing
- `cc_server`: Thick server communication
- `cc_tim`: Timing utilities
- `cc_coms`: Communication utilities
- `cc_std`: Standard utilities

---

## Notes

1. **Static Classes:** Most CCSUB classes are implemented as static class instances (e.g., `cc_str`, `cc_stk`).

2. **Mapped Memory:** Many classes interact with mapped memory files for real-time data access.

3. **Thread Safety:** Classes are designed for multi-process access but may require synchronization for multi-threaded use.

4. **Error Handling:** Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.).

---

## Related Documentation

- CSUB: `csub.md` for common subroutines
- OSUB: `osub.md` for database access
- Database Reference: `04_Database_Reference` for table schemas

