# cs_msg - Inter-Process Message Queue Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_msg.cpp`, `CS_MSG.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Module Name** | `cs_msg` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cs_msg` module provides **inter-process message queue functionality** using mapped memory files. It enables FIFO message passing between MEM background processes, dispatchers, completers, and communication services.

**Critical Operations:**
- Create message queues in mapped memory
- Put messages into queues
- Get messages from queues (FIFO)
- Peek at messages without removing
- Queue status and information

**Key Principle:** Message queues use **mapped memory** for fast, shared access between processes without disk I/O.

---

## Key Functions

### cs_msg_create

**Signature:**
```cpp
long cs_msg_create(char *quenam, long msgsiz, long nummsg, char *prcnam)
```

**Description:**
Creates a new message queue with specified parameters. Creates both header and data files in mapped memory.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `quenam` | `char*` | Queue name (used in file names, e.g., "MoveQueue") |
| `msgsiz` | `long` | Size of each message in bytes |
| `nummsg` | `long` | Maximum number of messages in queue |
| `prcnam` | `char*` | Process name to signal when messages arrive |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - queue created |
| `-1` | `GP.BAD` | Failure - creation error, file exists, or invalid parameters |

**Logic Flow:**
1. Validates parameters (non-empty queue name, positive sizes)
2. Creates header file: `msg_{quenam}.head` in mapped memory directory
3. Creates data file: `msg_{quenam}.data` in mapped memory directory
4. Initializes queue structure:
   - Message size = `msgsiz`
   - Maximum messages = `nummsg`
   - Read pointer = 0
   - Write pointer = 0
   - Message count = 0
   - Process name = `prcnam`
5. Sets up semaphore for synchronization
6. Flushes mapped memory to disk

**Dependencies:**
- Calls: `cc_mem` functions for mapped memory operations
- Files: Creates files in `{sysbase}\shmf\` directory

**Usage Example:**
```cpp
if (cs_msg_create("MoveQueue", sizeof(MOVE_MSG), 100, "p_ar_movdp") != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create message queue MoveQueue");
    return GP.BAD;
}
cs_log_printf(FILELINE, CS_LOG_INFO, "Created message queue MoveQueue (size=%ld, max=%ld)", 
              sizeof(MOVE_MSG), 100);
```

**Source Files:**
- `CS_MSG.h` (line 52)
- `cs_msg.cpp` (implementation)

---

### cs_msg_put

**Signature:**
```cpp
long cs_msg_put(char *quenam, void *buf, char cntrl, char WakEm, 
                char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Puts a message into the queue. Adds message to end of queue and optionally signals the receiving process.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `quenam` | `char*` | Queue name |
| `buf` | `void*` | Pointer to message data (must be `msgsiz` bytes) |
| `cntrl` | `char` | Control flag (reserved for future use) |
| `WakEm` | `char` | If non-zero, signals the receiving process |
| `file` | `char*` | Optional source file name for logging |
| `line` | `long` | Optional line number for logging |
| `name` | `char*` | Optional function name for logging |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - message queued |
| `-1` | `GP.BAD` | Failure - queue full, queue not found, or error |

**Logic Flow:**
1. Opens queue (maps memory files into address space)
2. Acquires semaphore for thread-safe access
3. Checks if queue has space (`nummsg < maxmsg`)
4. If queue full, releases semaphore and returns `GP.BAD`
5. Copies message data from `buf` to queue at write pointer
6. Updates write pointer (wraps around if needed)
7. Increments message count
8. If `WakEm` is set, signals the receiving process using process name
9. Updates database flag if configured (for monitoring)
10. Releases semaphore
11. Flushes mapped memory to disk

**Dependencies:**
- Calls: `cc_mem` functions, semaphore functions
- Tables: May update database if `db_update` flag is set

**Usage Example:**
```cpp
MOVE_MSG msg;
MOVS_init(&msg.movs);
cc_str.copy(msg.movs.loadid, sizeof(msg.movs.loadid), "L00001");
cc_str.copy(msg.movs.from_location, sizeof(msg.movs.from_location), "PD01");
cc_str.copy(msg.movs.to_location, sizeof(msg.movs.to_location), "A01-01-01");

if (cs_msg_put("MoveQueue", &msg, 0, 1, __FILE__, __LINE__, "DispatchMove") != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to queue move message for load %s", 
                  msg.movs.loadid);
    return GP.BAD;
}
cs_log_printf(FILELINE, CS_LOG_INFO, "Queued move message for load %s", msg.movs.loadid);
```

**Source Files:**
- `CS_MSG.h` (line 63)
- `cs_msg.cpp` (implementation)

---

### cs_msg_get

**Signature:**
```cpp
long cs_msg_get(char *quenam, void *buf, char* file = NULL, long line = 0, char* name = NULL)
long cs_msg_get(char *quenam, void *buf, const long this_Size, 
                char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Gets the next message from the queue (FIFO). Removes message from queue.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `quenam` | `char*` | Queue name |
| `buf` | `void*` | Buffer to receive message data (must be at least `msgsiz` bytes) |
| `this_Size` | `long` | Optional buffer size for validation (second overload) |
| `file` | `char*` | Optional source file name for logging |
| `line` | `long` | Optional line number for logging |
| `name` | `char*` | Optional function name for logging |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - message retrieved |
| `-1` | `GP.BAD` | Failure - queue empty, queue not found, or error |

**Logic Flow:**
1. Opens queue (maps memory files)
2. Acquires semaphore
3. Checks if queue has messages (`nummsg > 0`)
4. If queue empty, releases semaphore and returns `GP.BAD`
5. Copies message data from queue at read pointer to `buf`
6. Updates read pointer (wraps around if needed)
7. Decrements message count
8. Updates database flag if configured
9. Releases semaphore
10. Flushes mapped memory to disk

**Usage Example:**
```cpp
MOVE_MSG msg;
if (cs_msg_get("MoveQueue", &msg, __FILE__, __LINE__, "ProcessMove") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Retrieved move message for load %s", 
                  msg.movs.loadid);
    // Process message...
} else {
    // No messages available (not an error condition)
}
```

**Source Files:**
- `CS_MSG.h` (lines 57-58)
- `cs_msg.cpp` (implementation)

---

### cs_msg_peek

**Signature:**
```cpp
long cs_msg_peek(char *quenam, void *buf, char* file = NULL, long line = 0, char* name = NULL)
```

**Description:**
Peeks at the next message in the queue without removing it. Non-destructive read.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `quenam` | `char*` | Queue name |
| `buf` | `void*` | Buffer to receive message data |
| `file` | `char*` | Optional source file name for logging |
| `line` | `long` | Optional line number for logging |
| `name` | `char*` | Optional function name for logging |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - message available |
| `-1` | `GP.BAD` | Failure - queue empty |

**Logic Flow:**
1. Opens queue
2. Acquires semaphore
3. Checks if queue has messages
4. If empty, releases semaphore and returns `GP.BAD`
5. Copies message data from queue at read pointer to `buf` (does not update pointers)
6. Releases semaphore
7. Returns `GP.GOOD`

**Usage Example:**
```cpp
MOVE_MSG msg;
if (cs_msg_peek("MoveQueue", &msg) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Next message in queue: load %s", 
                  msg.movs.loadid);
    // Inspect message without removing...
}
```

**Source Files:**
- `CS_MSG.h` (lines 61-62)
- `cs_msg.cpp` (implementation)

---

### cs_msg_info

**Signature:**
```cpp
long cs_msg_info(char *quenam, long *laston, long *lastof, long *nummsg, 
                 long *msgsiz, long *maxmsg, char *prgnam, size_t sizeInBytes)
```

**Description:**
Retrieves information about a message queue (status, counts, process name).

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `quenam` | `char*` | Queue name |
| `laston` | `long*` | Output - index of last message put (write pointer) |
| `lastof` | `long*` | Output - index of last message read (read pointer) |
| `nummsg` | `long*` | Output - current number of messages in queue |
| `msgsiz` | `long*` | Output - message size in bytes |
| `maxmsg` | `long*` | Output - maximum number of messages |
| `prgnam` | `char*` | Output - process name (buffer) |
| `sizeInBytes` | `size_t` | Size of `prgnam` buffer |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - information retrieved |
| `-1` | `GP.BAD` | Failure - queue not found or error |

**Usage Example:**
```cpp
long laston, lastof, nummsg, msgsiz, maxmsg;
char prgnam[64];

if (cs_msg_info("MoveQueue", &laston, &lastof, &nummsg, &msgsiz, &maxmsg, 
                prgnam, sizeof(prgnam)) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, 
                  "Queue MoveQueue: %ld/%ld messages, process=%s", 
                  nummsg, maxmsg, prgnam);
}
```

**Source Files:**
- `CS_MSG.h` (line 64)
- `cs_msg.cpp` (implementation)

---

## Common Usage Patterns

### Pattern 1: Producer Process (Dispatcher)

```cpp
// Create queue during initialization
if (cs_msg_create("MoveQueue", sizeof(MOVE_MSG), 100, "p_ar_movdp") != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to create MoveQueue");
    return GP.BAD;
}

// In main loop: Put messages into queue
MOVE_MSG msg;
// ... populate msg ...
if (cs_msg_put("MoveQueue", &msg, 0, 1, __FILE__, __LINE__, "DispatchMove") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Queued move for load %s", msg.movs.loadid);
} else {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "Queue full, cannot queue move for load %s", 
                  msg.movs.loadid);
}
```

### Pattern 2: Consumer Process (Completer)

```cpp
// In main loop: Get messages from queue
MOVE_MSG msg;
if (cs_msg_get("MoveQueue", &msg, __FILE__, __LINE__, "ProcessMove") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Processing move for load %s", msg.movs.loadid);
    
    // Process message...
    if (ProcessMove(&msg) == GP.GOOD) {
        cs_log_printf(FILELINE, CS_LOG_INFO, "Move completed for load %s", msg.movs.loadid);
    } else {
        cs_log_printf(FILELINE, CS_LOG_ERROR, "Move failed for load %s", msg.movs.loadid);
    }
} else {
    // No messages available (normal condition, not an error)
    Sleep(100); // Wait before checking again
}
```

### Pattern 3: Queue Monitoring

```cpp
long laston, lastof, nummsg, msgsiz, maxmsg;
char prgnam[64];

if (cs_msg_info("MoveQueue", &laston, &lastof, &nummsg, &msgsiz, &maxmsg, 
                prgnam, sizeof(prgnam)) == GP.GOOD) {
    double utilization = (double)nummsg / (double)maxmsg * 100.0;
    cs_log_printf(FILELINE, CS_LOG_INFO, 
                  "MoveQueue status: %ld/%ld messages (%.1f%%), process=%s", 
                  nummsg, maxmsg, utilization, prgnam);
    
    if (nummsg > maxmsg * 0.9) {
        cs_log_printf(FILELINE, CS_LOG_WARNING, "MoveQueue nearly full!");
    }
}
```

---

## Message Queue File Structure

**Header File:** `msg_{quenam}.head`
- Queue metadata (size, max messages, pointers, process name)
- Semaphore information
- Status flags

**Data File:** `msg_{quenam}.data`
- Circular buffer of messages
- Size = `msgsiz * nummsg` bytes

**Location:** `{sysbase}\shmf\` directory

---

## Thread Safety

- All queue operations use **semaphores** for synchronization
- Multiple processes can safely read/write to the same queue
- Semaphore ensures atomic operations
- Mapped memory provides fast shared access

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Mapped Memory | [cc_mem.md](../02_CCSUB/cc_mem.md) | Memory mapping |
| Inter-Process Communication | [IPC Guide](../../05_Workflows/02_Inter_Process_Communication.md) | Message Queues |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

