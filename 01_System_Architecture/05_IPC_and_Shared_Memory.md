# IPC and Shared Memory

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

This document describes the Inter-Process Communication (IPC) mechanisms and shared memory structure used in the Auro System for high-performance data sharing between processes.

---

## Shared Memory Architecture

### Mapped Memory Files (.shm)

**Location:** `D:\Auro\shmf\`

**Purpose:** Enable fast, concurrent read/write access to real-time equipment data without disk I/O latency.

**Characteristics:**
- Memory-mapped files backed by disk
- Multiple processes can map the same file
- Data persisted to disk periodically
- Flushed to disk on system shutdown

---

## Shared Memory Structure

### Equipment State Files

#### Stacker State (`Stacker.shm`)
- **Size:** Variable (based on number of stackers)
- **Structure:** Array of stacker state structures
- **Contents:**
  - Schedule status (NOT_BUSY, BUSY, ERROR)
  - Function (IDLE, STORE, RETRIEVE)
  - Current step (for multi-cycle operations)
  - Error flags and codes
  - Current location
  - Load information
- **Accessed By:**
  - `p_ar_stkdp` (reads/writes schedule)
  - `p_cc_stkcmx` (reads/writes state)
  - `p_ar_srcmp` (reads completion status)
  - `mhcMenu.exe` (reads for display)

#### Station State (`Station.shm`)
- **Size:** Variable (based on number of stations)
- **Structure:** Array of station state structures
- **Contents:**
  - Station status (READY, BUSY, ERROR)
  - Load present flag
  - Current load ID
  - Error flags
- **Accessed By:**
  - `p_ar_movdp` (reads/writes station status)
  - `p_ar_arive` (reads arrival status)
  - PLC communication processes

#### PLC State (`PLC.shm`)
- **Size:** Variable (based on number of PLCs)
- **Structure:** Array of PLC state structures
- **Contents:**
  - Communication status (ONLINE, OFFLINE, CONNECTING)
  - Input register values
  - Output register values
  - Error flags
- **Accessed By:**
  - `Kepware_PLC.exe` (reads/writes PLC tags)
  - `p_ar_movdp` (reads station status)
  - Equipment control processes

### Process Table (`Process.shm`)
- **Purpose:** Tracks process status and heartbeats
- **Structure:** Array of process information
- **Contents:**
  - Process ID
  - Process name
  - Status (RUNNING, STOPPED, ERROR)
  - Last heartbeat timestamp
  - Priority
- **Accessed By:**
  - `p_sy_watch` (monitors process health)
  - All processes (update heartbeat)

### Message Queues (`Queue.shm`)
- **Purpose:** Asynchronous message passing between processes
- **Structure:** Multiple queues for different message types
- **Queue Types:**
  - Stacker command queue
  - AGV command queue
  - RTNX command queue
  - Host message queue
  - Error message queue
- **Message Structure:**
  - Message type
  - Timestamp
  - Payload length
  - Payload data
- **Accessed By:**
  - Dispatchers (write commands)
  - Communication processes (read commands)
  - Completers (read completions)

### System Control (`System.shm`)
- **Purpose:** System-wide control flags and state
- **Contents:**
  - Warmstart flag
  - Shutdown flag
  - System status
  - Global error flags
- **Accessed By:**
  - All processes (read system state)
  - System control processes (write flags)

---

## Access Methods

### C++ Access

```cpp
// Example: Accessing stacker state
#include "cc_stk.h"

cc_stk stacker;
stacker.Initialize(aisle_number);
if (stacker.GetSchedule() == NOT_BUSY) {
    stacker.SetSchedule(BUSY);
    stacker.SetFunction(STORE);
    stacker.Commit();
}
```

### VB.NET Access

```vbnet
' Example: Accessing stacker state via CInterop
Dim stackerHandle As IntPtr = VBDLL.GetStackerHandle(aisleNumber)
Dim schedule As Integer = VBDLL.GetStackerSchedule(stackerHandle)
If schedule = NOT_BUSY Then
    VBDLL.SetStackerSchedule(stackerHandle, BUSY)
    VBDLL.SetStackerFunction(stackerHandle, STORE)
End If
```

---

## Synchronization Mechanisms

### Mutexes
- **Purpose:** Prevent concurrent write access
- **Usage:** Acquire mutex before writing, release after
- **Implementation:** Windows named mutexes

### Semaphores
- **Purpose:** Control access to shared resources
- **Usage:** Signal availability, wait for availability
- **Implementation:** Windows semaphores

### Atomic Operations
- **Purpose:** Thread-safe counter updates
- **Usage:** Increment/decrement statistics counters
- **Implementation:** Interlocked operations

---

## Message Queue Operations

### Writing to Queue

```cpp
// Example: Writing stacker command
MessageQueue queue;
Message msg;
msg.type = STORE_COMMAND;
msg.timestamp = GetCurrentTime();
msg.payload = commandData;
queue.Write(msg);
```

### Reading from Queue

```cpp
// Example: Reading stacker command
MessageQueue queue;
Message msg;
if (queue.Read(msg)) {
    ProcessCommand(msg);
    queue.Acknowledge(msg.id);
}
```

---

## Persistence and Recovery

### Flush to Disk
- **Frequency:** Every 5 seconds (configurable)
- **Trigger:** System shutdown, manual flush
- **Process:** `p_sy_shmop.exe` (shared memory operations)

### Recovery on Warmstart
- **Process:** `p_sy_gcini.exe` initializes shared memory
- **Actions:**
  - Loads persisted state from disk
  - Reinitializes structures
  - Clears transient data

---

## Performance Considerations

### Access Patterns
- **Read-Heavy:** Most processes read more than write
- **Write Frequency:** Dispatchers write frequently, completers write on completion
- **Contention:** Low (different processes access different equipment)

### Optimization
- **Memory Alignment:** Structures aligned for efficient access
- **Cache-Friendly:** Related data grouped together
- **Minimal Locking:** Lock only when necessary

---

## Related Documents

- [Architecture Overview](00_Architecture_Overview.md)
- [Component Dependency Map](01_Component_Dependency_Map.md)
- [Process Startup Sequence](04_Process_Startup_Sequence.md)
- [MHC-Developers-Guide IPC](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/01_System_Architecture) - Generic IPC patterns

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Process Details | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process Documentation |
| Shared Libraries | [Code Reference](../03_Code_Reference/03_Shared_Libraries/) | Library Documentation |
| Startup Sequence | [Process Startup Sequence](04_Process_Startup_Sequence.md) | Initialization |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Memory Issues |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

