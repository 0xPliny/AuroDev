# cs_log_printf - Logging Function

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `CS_LOG.cpp`, `CS_LOG.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Function Name** | `cs_log_printf` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

`cs_log_printf` is the **primary logging function** used throughout the MHC/MEM system. It provides file-based logging with rotation, size limits, and semaphore-based synchronization for multi-process access.

**Critical Operations:**
- Format and write log messages
- Log level filtering
- Automatic log rotation
- Thread-safe multi-process logging
- Timestamp and source location tracking

**Key Principle:** **ALWAYS use `cs_log_printf` for logging** - NEVER use `printf`, `cout`, or other standard output functions.

---

## Function Signature

```cpp
long cs_log_printf(const char *name, long loglvl, const char *msgtxt, ...)
```

---

## Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Log file name as defined in `log.cnf` configuration file. Use `FILELINE` macro for automatic file/line tracking. |
| `loglvl` | `long` | Log level (0-10). Message is written if `loglvl <= configured_log_level` for the log file. |
| `msgtxt` | `const char*` | Format string (printf-style) with optional format specifiers. |
| `...` | Variable arguments | Arguments for format specifiers in `msgtxt`. |

---

## Return Values

| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - message written to log file |
| `-1` | `GP.BAD` | Failure - log file error, file not found, or write failure |

---

## Logic Flow

1. **Log File Lookup:**
   - Looks up log file configuration from `log.cnf` using `name` parameter
   - If log file not found in cache, opens and caches the file handle

2. **Log Level Check:**
   - Compares `loglvl` parameter with configured log level for the file
   - If `loglvl > configured_log_level`, message is **not written** (filtered)
   - Returns `GP.GOOD` even if filtered (not an error condition)

3. **Message Formatting:**
   - Gets current timestamp using `cs_dtm_current()` and `cs_dtm_fmt()`
   - Extracts file name and line number from `name` parameter (if `FILELINE` macro used)
   - Formats message with: `[timestamp] [file:line] [level] message_text`

4. **File Write:**
   - Acquires semaphore for thread-safe write
   - Writes formatted message to log file
   - Releases semaphore

5. **Log Rotation Check:**
   - Checks current log file size
   - If size exceeds `max_size` from configuration, rotates log file
   - Creates new log file and archives old one

6. **Return:**
   - Returns `GP.GOOD` on success
   - Returns `GP.BAD` on file error

---

## Dependencies

### Called Functions:
- `cs_log_open()`: Opens or retrieves cached log file handle
- `cs_log_check()`: Checks log level and file size
- `cs_dtm_current()`: Gets current system date/time
- `cs_dtm_fmt()`: Formats date/time to string
- `cc_str.concat()`: Concatenates formatted message components

### Configuration Files:
- `log.cnf`: Log file definitions (location: `File\log.cnf`)

### Log File Location:
- Log files stored in: `{sysbase}\logs\` directory
- Example: `D:\Auro\Logs\error.log`

---

## Log Levels

| Level | Constant | Description | Typical Usage |
|-------|----------|-------------|---------------|
| `0` | `CS_LOG_ERROR` | Error messages | Critical errors requiring immediate attention |
| `1-5` | `CS_LOG_WARNING` | Warning messages | Non-critical issues, retries, recoverable errors |
| `6-7` | `CS_LOG_INFO` | Informational messages | Normal operations, status updates |
| `8-10` | `CS_LOG_DEBUG` | Debug/trace messages | Detailed debugging, function entry/exit, data dumps |

**Log Level Filtering:**
- If configured log level is `3`, messages with level `0-3` are written
- Messages with level `4-10` are filtered (not written)

---

## Log Configuration (log.cnf)

Log files are configured in `log.cnf` with the following format:

```
logname,filename,max_size,truncate_size,keep_days
```

**Parameters:**
- `logname`: Name used in `cs_log_printf` calls
- `filename`: Physical log file name
- `max_size`: Maximum file size in bytes before rotation
- `truncate_size`: Size to truncate to when rotating (typically half of max_size)
- `keep_days`: Number of days to keep rotated log files

**Example log.cnf:**
```
ErrorLog,error.log,10485760,5242880,30
MoveLog,move.log,5242880,2621440,7
CommLog,comm.log,5242880,2621440,7
DebugLog,debug.log,10485760,5242880,7
```

---

## Usage Patterns

### Pattern 1: Error Logging

```cpp
if (operationFailed) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "HstCm: Failed to process basket [%s], rc=%d", 
                  basketId, returnCode);
    return GP.BAD;
}
```

**Output Example:**
```
[2024-12-23 14:30:45.123] [hstcm.cpp:245] [ERROR] HstCm: Failed to process basket [B001], rc=-1
```

### Pattern 2: Informational Logging

```cpp
cs_log_printf(FILELINE, CS_LOG_INFO, "Component: Starting initialization");
cs_log_printf(FILELINE, CS_LOG_INFO, "Load %s moved from %s to %s", 
              loadId, fromLocation, toLocation);
```

**Output Example:**
```
[2024-12-23 14:30:45.123] [movdp.cpp:123] [INFO] Component: Starting initialization
[2024-12-23 14:30:45.456] [movdp.cpp:234] [INFO] Load L00001 moved from PD01 to A01-01-01
```

### Pattern 3: Warning Logging

```cpp
if (retryCount < maxRetries) {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "Component: Retry attempt %d of %d", 
                  retryCount, maxRetries);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Component: Max retries exceeded");
}
```

**Output Example:**
```
[2024-12-23 14:30:45.123] [comm.cpp:567] [WARNING] Component: Retry attempt 2 of 3
```

### Pattern 4: Debug/Trace Logging

```cpp
cs_log_printf(FILELINE, CS_LOG_DEBUG, "Function entry: ProcessMove(loadId=%s, from=%s, to=%s)", 
              loadId, fromLoc, toLoc);

// ... processing ...

cs_log_printf(FILELINE, CS_LOG_DEBUG, "Function exit: ProcessMove, rc=%d", returnCode);
```

**Output Example:**
```
[2024-12-23 14:30:45.123] [movdp.cpp:89] [DEBUG] Function entry: ProcessMove(loadId=L00001, from=PD01, to=A01-01-01)
[2024-12-23 14:30:45.789] [movdp.cpp:156] [DEBUG] Function exit: ProcessMove, rc=0
```

### Pattern 5: Component Prefix Convention

**Always include component prefix in log messages:**

```cpp
// ✅ CORRECT - Component prefix included
cs_log_printf(FILELINE, CS_LOG_ERROR, "HstCm: Failed to process basket [%s]", basketId);
cs_log_printf(FILELINE, CS_LOG_INFO, "REST API: Received store request for load %s", loadId);
cs_log_printf(FILELINE, CS_LOG_WARNING, "Cella: API timeout, retrying...");

// ❌ WRONG - No component prefix
cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to process basket [%s]", basketId);
```

**Component Prefix Examples:**
- `HstCm:` - Host Communication
- `REST API:` - REST API service
- `Cella:` - Cella API integration
- `p_ar_movdp:` - Move Dispatcher process
- `p_cc_mudpcm:` - MUDP Communication process

---

## FILELINE Macro

The `FILELINE` macro is defined to automatically include source file and line number in log messages.

**Definition:**
```cpp
#define FILELINE __FILE__ ":" __LINE__
```

**Usage:**
```cpp
cs_log_printf(FILELINE, CS_LOG_ERROR, "Error message");
```

**Expands to:**
```cpp
cs_log_printf("movdp.cpp:245", CS_LOG_ERROR, "Error message");
```

**Benefits:**
- Automatic source location tracking
- No need to manually update line numbers
- Helps with debugging

---

## Log Message Format

The formatted log message has the following structure:

```
[timestamp] [file:line] [level] component_prefix: message_text
```

**Example:**
```
[2024-12-23 14:30:45.123] [movdp.cpp:245] [ERROR] HstCm: Failed to process basket [B001], rc=-1
```

**Components:**
- `[timestamp]`: Date and time with millisecond precision
- `[file:line]`: Source file and line number (from `name` parameter)
- `[level]`: Log level (ERROR, WARNING, INFO, DEBUG)
- `component_prefix:`: Component identifier (convention, not automatic)
- `message_text`: Formatted message text

---

## Performance Considerations

1. **File Caching:**
   - Log file handles are cached to avoid repeated file opens
   - First call opens file, subsequent calls use cached handle

2. **Log Level Filtering:**
   - Messages filtered by log level are not formatted or written
   - Minimal performance impact for filtered messages

3. **Semaphore Synchronization:**
   - Semaphore ensures thread-safe writes in multi-process environment
   - May cause slight delay if multiple processes write simultaneously

4. **Log Rotation:**
   - Rotation check occurs after each write
   - Rotation itself may cause brief delay during file operations

**Best Practices:**
- Use appropriate log levels (don't use DEBUG in production)
- Avoid excessive logging in tight loops
- Use INFO level for normal operations, ERROR for failures

---

## Common Usage Examples

### Example 1: Database Operation Logging

```cpp
SQL.start(SQL.READ_WRITE);

MOVS movs;
MOVS_init(&movs);
cc_str.copy(movs.loadid, sizeof(movs.loadid), "L00001");
cc_str.copy(movs.area, sizeof(movs.area), "A");

if (MOVS_select_MOVS_PR_KEY(&movs, GP_TRUE) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "p_ar_movdp: Selected move for load [%s], status=%s", 
                  movs.loadid, movs.move_status);
    
    // Update move
    cc_str.copy(movs.move_status, sizeof(movs.move_status), "COMP");
    if (MOVS_update(&movs) == GP.GOOD) {
        SQL.commit();
        cs_log_printf(FILELINE, CS_LOG_INFO, "p_ar_movdp: Move [%s] completed successfully", 
                      movs.loadid);
    } else {
        SQL.rollback();
        cs_log_printf(FILELINE, CS_LOG_ERROR, "p_ar_movdp: Failed to update move [%s], rc=%d", 
                      movs.loadid, GP.BAD);
    }
} else {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "p_ar_movdp: Move not found for load [%s]", 
                  movs.loadid);
}
```

### Example 2: Error with Return Code

```cpp
long rc = SomeFunction();
if (rc != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Component: Function failed, rc=%d", rc);
    return GP.BAD;
}
```

### Example 3: Status Update

```cpp
cs_log_printf(FILELINE, CS_LOG_INFO, "Stacker %s: Schedule=%ld, Function=%ld, Comm=%ld", 
              stackerName, schedule, function, comm);
```

### Example 4: Multi-Value Logging

```cpp
cs_log_printf(FILELINE, CS_LOG_DEBUG, "Route calculation: from=%s, to=%s, cost=%ld, path=%s", 
              fromStation, toStation, cost, pathString);
```

---

## Forbidden Functions

**NEVER use these standard C/C++ output functions in MHC code:**

| Forbidden Function | Use Instead |
|-------------------|-------------|
| `printf()`, `fprintf()` | `cs_log_printf()` |
| `cout`, `cerr` | `cs_log_printf()` |
| `System.Diagnostics.Debug.WriteLine()` (VB.NET) | `cs_log_printf()` (via C++ wrapper) |

**Reason:** Standard output functions do not provide:
- Log file management
- Log rotation
- Multi-process synchronization
- Configurable log levels
- Timestamp and source location tracking

---

## Related Functions

| Function | Purpose | Document |
|----------|---------|----------|
| `cs_log_msg` | Log message using message number | [cs_log.md](cs_log.md) |
| `cs_log_close` | Close log file | [cs_log.md](cs_log.md) |

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| CSUB Overview | [CSUB Library](../csub.md) | cs_log Functions |
| Coding Standards | [C++ Standards](../../02_Coding_Standards/02_CPP_Standards.md) | Logging Standards |
| Log Configuration | [Configuration Files](../../04_Database_Reference/03_Configuration_Files.md) | log.cnf |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

