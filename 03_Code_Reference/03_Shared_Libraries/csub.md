# CSUB - Common Subroutines

**Document Version:** 2.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

**Source Location:** 
- `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub`
- `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub` (Mixed with CCSUB)

**Description:** CSUB (Common Subroutines) provides fundamental utility functions used throughout the MHC/MEM system. These include logging, messaging, registry access, date/time handling, and other common operations.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Application Layer                          │
│    (Background Processes, UI Dialogs, Communication Services)   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        CSUB Layer                               │
│  ┌─────────────┐  ┌─────────────┐  ┌────────────────────────┐   │
│  │  Logging    │  │ Messaging   │  │ Configuration Access   │   │
│  │  (cs_log)   │  │ (cs_msg)    │  │ (cs_elt, cs_reg, cs_cnf)│  │
│  └─────────────┘  └─────────────┘  └────────────────────────┘   │
│  ┌─────────────┐  ┌─────────────┐  ┌────────────────────────┐   │
│  │ Date/Time   │  │  Semaphores │  │  Text Translation      │   │
│  │ (cs_dtm)    │  │  (cs_sem)   │  │  (cs_txt)              │   │
│  └─────────────┘  └─────────────┘  └────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              Operating System / File System Layer               │
└─────────────────────────────────────────────────────────────────┘
```

---

## Overview

CSUB functions are organized by functionality:
- **cs_log**: Logging functions
- **cs_msg**: Inter-process message queues
- **cs_reg**: Windows Registry access
- **cs_dtm**: Date/time manipulation
- **cs_elt**: Element/configuration access
- **cs_err**: Error handling utilities

---

## cs_log - Logging Functions

**Source:** `CS_LOG.H`, `CS_LOG.Cpp`

**Description:** Provides file-based logging with rotation, size limits, and semaphore-based synchronization for multi-process access.

### `cs_log_printf`

**Signature:**
```cpp
long cs_log_printf(const char *name, long loglvl, const char *msgtxt, ...)
```

**Description:**
Formats and writes a log message to a named log file. Supports printf-style formatting. Messages are written based on log level filtering.

**Parameters:**
- `name` (const char*): Log file name as defined in `log.cnf` configuration file
- `loglvl` (long): Log level (0-10). Message is written if `loglvl <= configured_log_level`
- `msgtxt` (const char*): Format string (printf-style)
- `...`: Variable arguments for format string

**Return Values:**
- `GP.GOOD` (0): Success - message written
- `GP.BAD` (-1): Failure - log file error

**Logic Flow:**
1. Opens log file (cached for performance)
2. Checks log level against configuration
3. Formats message with timestamp, file/line, and user text
4. Writes to log file
5. Checks file size and rotates if needed
6. Uses semaphore for thread-safe writes

**Dependencies:**
- Calls: `cs_log_open()`, `cs_log_check()`, `cs_dtm_current()`, `cs_dtm_fmt()`, `cc_str.concat()`
- Configuration: `log.cnf` file in project File directory
- Files: Log files in `{sysbase}\logs\` directory

**Usage Example:**
```cpp
cs_log_printf(FILELINE, CS_LOG_ERROR, "HstCm: Failed to process basket [%s], rc=%d", basketId, rc);
cs_log_printf(FILELINE, CS_LOG_INFO, "Component: Starting initialization");
cs_log_printf(FILELINE, CS_LOG_WARNING, "Component: Retry attempt %d of %d", retryCount, maxRetries);
```

**Log Levels:**
- `CS_LOG_ERROR` (0): Error messages
- `CS_LOG_WARNING` (1-5): Warning messages
- `CS_LOG_INFO` (6-7): Informational messages
- `CS_LOG_DEBUG` (8-10): Debug/trace messages

**Source Files:**
- `CS_LOG.H` (line 57)
- `CS_LOG.Cpp` (implementation)

---

### `cs_log_msg`

**Signature:**
```cpp
long cs_log_msg(char *name, long loglvl, long msgnum, ...)
```

**Description:**
Writes a log message using a message number that references a message file (e.g., `English.msg`). The message text is looked up from the message file.

**Parameters:**
- `name` (char*): Log file name
- `loglvl` (long): Log level
- `msgnum` (long): Message number to look up
- `...`: Variable arguments for message parameters

**Return Values:**
- `GP.GOOD` (0): Success
- `GP.BAD` (-1): Failure

**Dependencies:**
- Calls: Message file lookup functions
- Files: Message files (e.g., `English.msg`)

**Source Files:**
- `CS_LOG.H` (line 55)
- `CS_LOG.Cpp` (implementation)

---

### `cs_log_close`

**Signature:**
```cpp
long cs_log_close(char *name)
```

**Description:**
Closes a log file and releases resources. Typically called during program shutdown.

**Parameters:**
- `name` (char*): Log file name

**Return Values:**
- `GP.GOOD` (0): Success
- `GP.BAD` (-1): Failure

**Source Files:**
- `CS_LOG.H` (line 53)
- `CS_LOG.Cpp` (implementation)

---

### Log Configuration (log.cnf)

Log files are configured in `log.cnf` file with format:
```
logname,filename,max_size,truncate_size,keep_days
```

Example:
```
ErrorLog,error.log,10485760,5242880,30
MoveLog,move.log,5242880,2621440,7
```

---

## cs_msg - Inter-Process Message Queues

**Source:** `CS_MSG.H`, `cs_msg.cpp`

**Description:** Provides inter-process message queue functionality using mapped memory files. Supports FIFO message passing between processes.

### `cs_msg_create`

**Signature:**
```cpp
long cs_msg_create(char *quenam, long msgsiz, long nummsg, char *prcnam)
```

**Description:**
Creates a new message queue with specified parameters. Creates both header and data files in mapped memory.

**Parameters:**
- `quenam` (char*): Queue name (used in file names)
- `msgsiz` (long): Size of each message in bytes
- `nummsg` (long): Maximum number of messages in queue
- `prcnam` (char*): Process name to signal when messages arrive

**Return Values:**
- `GP.GOOD` (0): Success - queue created
- `GP.BAD` (-1): Failure - creation error

**Logic Flow:**
1. Creates header file (`msg_{quenam}.head`) in mapped memory
2. Creates data file (`msg_{quenam}.data`) in mapped memory
3. Initializes queue structure with parameters
4. Sets up semaphore for synchronization

**Dependencies:**
- Calls: `cc_mem` functions for mapped memory
- Files: Creates files in `{sysbase}\shmf\` directory

**Usage Example:**
```cpp
if (cs_msg_create("MoveQueue", sizeof(MOVE_MSG), 100, "p_ar_movdp") != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create message queue");
    return GP.BAD;
}
```

**Source Files:**
- `CS_MSG.H` (line 52)
- `cs_msg.cpp` (implementation)

---

### `cs_msg_put`

**Signature:**
```cpp
long cs_msg_put(char *quenam, void *buf, char cntrl, char WakEm, char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Puts a message into the queue. Adds message to end of queue and optionally signals the receiving process.

**Parameters:**
- `quenam` (char*): Queue name
- `buf` (void*): Pointer to message data
- `cntrl` (char): Control flag
- `WakEm` (char): If non-zero, signals the receiving process
- `file` (char*): Optional source file name for logging
- `line` (long): Optional line number for logging
- `name` (char*): Optional function name for logging

**Return Values:**
- `GP.GOOD` (0): Success - message queued
- `GP.BAD` (-1): Failure - queue full or error

**Logic Flow:**
1. Opens queue (maps memory files)
2. Checks if queue has space
3. Copies message data to queue
4. Updates queue pointers
5. Signals receiving process if `WakEm` is set
6. Updates database flag if configured

**Dependencies:**
- Calls: `cc_mem` functions, semaphore functions
- Tables: May update database if `db_update` flag is set

**Usage Example:**
```cpp
MOVE_MSG msg;
// ... populate msg ...
if (cs_msg_put("MoveQueue", &msg, 0, 1, __FILE__, __LINE__, "DispatchMove") != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to queue move message");
}
```

**Source Files:**
- `CS_MSG.H` (line 63)
- `cs_msg.cpp` (implementation)

---

### `cs_msg_get`

**Signature:**
```cpp
long cs_msg_get(char *quenam, void *buf, char* file = NULL, long line = 0, char* name = NULL)
long cs_msg_get(char *quenam, void *buf, const long this_Size, char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Gets the next message from the queue (FIFO). Removes message from queue.

**Parameters:**
- `quenam` (char*): Queue name
- `buf` (void*): Buffer to receive message data
- `this_Size` (long): Optional buffer size for validation
- `file` (char*): Optional source file name for logging
- `line` (long): Optional line number for logging
- `name` (char*): Optional function name for logging

**Return Values:**
- `GP.GOOD` (0): Success - message retrieved
- `GP.BAD` (-1): Failure - queue empty or error

**Logic Flow:**
1. Opens queue
2. Checks if queue has messages
3. Copies message data from queue
4. Updates queue pointers
5. Updates database flag if configured

**Usage Example:**
```cpp
MOVE_MSG msg;
if (cs_msg_get("MoveQueue", &msg, __FILE__, __LINE__, "ProcessMove") == GP.GOOD) {
    // Process message
} else {
    // No messages available
}
```

**Source Files:**
- `CS_MSG.H` (lines 57-58)
- `cs_msg.cpp` (implementation)

---

### `cs_msg_peek`

**Signature:**
```cpp
long cs_msg_peek(char *quenam, void *buf, char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Peeks at the next message in the queue without removing it. Non-destructive read.

**Parameters:**
- Same as `cs_msg_get`

**Return Values:**
- `GP.GOOD` (0): Success - message available
- `GP.BAD` (-1): Failure - queue empty

**Source Files:**
- `CS_MSG.H` (lines 61-62)
- `cs_msg.cpp` (implementation)

---

### `cs_msg_info`

**Signature:**
```cpp
long cs_msg_info(char *quenam, long *laston, long *lastof, long *nummsg, long *msgsiz, long *maxmsg, char *prgnam, size_t sizeInBytes)
```

**Description:**
Retrieves information about a message queue (status, counts, process name).

**Parameters:**
- `quenam` (char*): Queue name
- `laston` (long*): Output - index of last message put
- `lastof` (long*): Output - index of last message read
- `nummsg` (long*): Output - current number of messages
- `msgsiz` (long*): Output - message size
- `maxmsg` (long*): Output - maximum messages
- `prgnam` (char*): Output - process name (buffer)
- `sizeInBytes` (size_t): Size of prgnam buffer

**Return Values:**
- `GP.GOOD` (0): Success
- `GP.BAD` (-1): Failure

**Source Files:**
- `CS_MSG.H` (line 64)
- `cs_msg.cpp` (implementation)

---

## cs_reg - Windows Registry Access

**Source:** `cs_reg.h`, `cs_reg.cpp`

**Description:** Provides functions to read and write Windows Registry values. Used for configuration storage.

### `cs_reg_read`

**Signature:**
```cpp
long cs_reg_read(char *group, char *title, char *reg, size_t size_reg)
```

**Description:**
Reads a string value from the Windows Registry under `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\{group}\{title}`.

**Parameters:**
- `group` (char*): Registry group (e.g., "KTH_ASRS")
- `title` (char*): Value name (e.g., "MaxRetries")
- `reg` (char*): Buffer to receive value
- `size_reg` (size_t): Size of buffer

**Return Values:**
- `GP.GOOD` (0): Success - value read
- `GP.BAD` (-1): Failure - key not found or error

**Logic Flow:**
1. Opens registry key `SOFTWARE\Muratec\{group}`
2. Reads value `{title}`
3. Copies to buffer
4. Closes registry key

**Dependencies:**
- Windows API: `RegOpenKeyEx`, `RegQueryValueEx`, `RegCloseKey`

**Usage Example:**
```cpp
int maxRetries;
char apiUrl[256];
cs_reg_read("KTH_ASRS", "MaxRetries", (char*)&maxRetries, sizeof(maxRetries));
cs_reg_read("KTH_ASRS", "CellaApiUrl", apiUrl, sizeof(apiUrl), "");
```

**Source Files:**
- `cs_reg.h` (line 28)
- `cs_reg.cpp` (implementation)

---

### `cs_reg_write`

**Signature:**
```cpp
long cs_reg_write(char *group, char *title, char *reg)
```

**Description:**
Writes a string value to the Windows Registry.

**Parameters:**
- `group` (char*): Registry group
- `title` (char*): Value name
- `reg` (char*): Value to write

**Return Values:**
- `GP.GOOD` (0): Success
- `GP.BAD` (-1): Failure

**Source Files:**
- `cs_reg.h` (line 27)
- `cs_reg.cpp` (implementation)

---

## cs_dtm - Date/Time Manipulation

**Source:** `cs_dtm.h`, `cs_dtm.cpp`

**Description:** Provides date/time conversion and formatting functions. Handles conversion between database formats, internal formats, and display formats.

### `cs_dtm_current`

**Signature:**
```cpp
void cs_dtm_current(double *date)
```

**Description:**
Gets the current system date/time in internal format (double).

**Parameters:**
- `date` (double*): Output - current date/time

**Return Values:**
- None (void function)

**Usage Example:**
```cpp
double curDate;
cs_dtm_current(&curDate);
cs_dtm_fmt(dateStr, sizeof(dateStr), curDate, DTM_MASK);
```

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp`

---

### `cs_dtm_fmt`

**Signature:**
```cpp
void cs_dtm_fmt(char *str, size_t size, double date, char *format)
```

**Description:**
Formats an internal date/time value to a string using the specified format.

**Parameters:**
- `str` (char*): Output buffer
- `size` (size_t): Buffer size
- `date` (double): Internal date/time value
- `format` (char*): Format string (e.g., `DTM_MASK`, `DTM_SQL_DATE`)

**Return Values:**
- None (void function)

**Format Constants:**
- `DTM_MASK`: Standard display format (e.g., "MM/DD/YYYY HH:MM:SS")
- `DTM_SQL_DATE`: SQL Server format (e.g., "YYYY-MM-DD HH:MM:SS.mmm")

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp`

---

### `cs_dtm_internal`

**Signature:**
```cpp
void cs_dtm_internal(double *date, double db_date)
```

**Description:**
Converts a database date/time value to internal format.

**Parameters:**
- `date` (double*): Output - internal date/time
- `db_date` (double): Database date/time value

**Return Values:**
- None (void function)

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp`

---

## cs_elt - Element/Configuration Access

**Source:** `cs_elt.h`, `cs_elt.cpp`

**Description:** Provides access to configuration elements stored in the database `ELEMENTS` table. Used for site-specific configuration values.

**Key Functions:**
- `cs_elt_read`: Read element value
- `cs_elt_write`: Write element value

**Usage:**
Configuration elements are accessed by group and element name, providing a flexible configuration system.

**Source Files:**
- `cs_elt.h`
- `cs_elt.cpp`

---

## cs_err - Error Handling Utilities

**Source:** `cs_err.h`, `cs_err.cpp`

**Description:** Provides error code translation and error handling utilities.

**Key Functions:**
- Error code to text translation
- Error logging
- Error recovery coordination

**Source Files:**
- `cs_err.h`
- `cs_err.cpp`

---

## Additional CSUB Functions

See source files for:
- `cs_cch`: Cache management
- `cs_mpm`: Mapped memory management
- `cs_tmr`: Timer utilities
- `cs_txt`: Text utilities
- `cs_sem`: Semaphore utilities
- `cs_seq`: Sequence number generation

---

## Notes

1. **Thread Safety:** Most CSUB functions use semaphores or are designed for single-threaded use within processes.

2. **Error Handling:** Functions return standard MHC return codes. Check return values before proceeding.

3. **Configuration:** Many functions depend on configuration files (e.g., `log.cnf`, `DBINI.CNF`).

4. **Mapped Memory:** Message queues and some utilities use mapped memory for inter-process communication.

---

## Related Documentation

- [CCSUB Library](ccsub.md) - Control classes
- [DSUB Library](dsub.md) - Database subroutines
- [OSUB Library](osub.md) - VB.NET database access
- [global_prm.h Reference](global_prm.md)
- [Configuration Files](../../06_Configuration/00_Configuration_Overview.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Control Classes | [CCSUB Library](ccsub.md) | Equipment control |
| Database Access | [DSUB Library](dsub.md) | SQL operations |
| Process Initialization | [p_ar_movdp](../01_Background_Processes/p_ar_movdp.md) | Uses cs_init |
| Logging Setup | [Quick Start Guide](../../00_Quick_Start_Guide.md) | Configuration |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2024-12-23 | Added architecture diagram and cross-references |
| 1.0 | 2024-12-22 | Initial creation |
