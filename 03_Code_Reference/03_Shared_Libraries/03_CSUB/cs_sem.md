# cs_sem - Semaphore Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_sem.cpp` / `CS_SEM.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Process Synchronization |

---

## Overview

The `cs_sem` module provides semaphore functions for inter-process synchronization in the MHC/MEM system. Semaphores prevent race conditions when multiple processes access shared resources.

**Key Responsibilities:**
- Create and manage named semaphores
- Lock/unlock critical sections
- Prevent resource contention
- Support timeout on lock operations

---

## Key Functions

### cs_sem_create

**Signature:**
```cpp
long cs_sem_create(const char* name, int initialCount);
```

**Purpose:** Create a new named semaphore.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Semaphore name |
| `initialCount` | `int` | Initial count (typically 1 for mutex) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Semaphore created |
| `GP.BAD` | Creation failed |

---

### cs_sem_lock

**Signature:**
```cpp
long cs_sem_lock(const char* name, int timeoutMs);
```

**Purpose:** Acquire (lock) a semaphore.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Semaphore name |
| `timeoutMs` | `int` | Timeout in ms (-1 for infinite) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Lock acquired |
| `GP.TIME_OUT` | Timeout waiting for lock |
| `GP.BAD` | Error |

---

### cs_sem_unlock

**Signature:**
```cpp
long cs_sem_unlock(const char* name);
```

**Purpose:** Release (unlock) a semaphore.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Semaphore name |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Lock released |
| `GP.BAD` | Error |

---

### cs_sem_destroy

**Signature:**
```cpp
long cs_sem_destroy(const char* name);
```

**Purpose:** Destroy a semaphore.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Semaphore name |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Semaphore destroyed |
| `GP.BAD` | Error |

---

### cs_sem_try_lock

**Signature:**
```cpp
long cs_sem_try_lock(const char* name);
```

**Purpose:** Try to acquire lock without waiting.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Semaphore name |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Lock acquired |
| `GP.BAD` | Lock not available |

---

## Common Usage Patterns

### Pattern 1: Protect Shared Resource Access

```cpp
#define SEM_DATABASE "SEM_DB_ACCESS"

// Create semaphore at startup
cs_sem_create(SEM_DATABASE, 1);

// Protected database operation
if (cs_sem_lock(SEM_DATABASE, 5000) == GP.GOOD) {
    // Critical section - only one process at a time
    performDatabaseOperation();
    
    cs_sem_unlock(SEM_DATABASE);
} else {
    cs_log_printf(FILELINE, GP_LOGLVL_1, "Timeout waiting for database lock");
}
```

### Pattern 2: Non-Blocking Lock Attempt

```cpp
// Try to acquire lock without blocking
if (cs_sem_try_lock(SEM_RESOURCE) == GP.GOOD) {
    // Got the lock
    processResource();
    cs_sem_unlock(SEM_RESOURCE);
} else {
    // Lock not available - try later
    cs_log_printf(FILELINE, GP_LOGLVL_4, "Resource busy, will retry");
}
```

### Pattern 3: Equipment Access Control

```cpp
#define SEM_STACKER_1 "SEM_STK_1"

// Lock stacker before sending commands
if (cs_sem_lock(SEM_STACKER_1, 10000) == GP.GOOD) {
    // Only this process can command stacker
    sendStackerCommand(stackerId, command);
    
    // Wait for response
    waitForResponse();
    
    cs_sem_unlock(SEM_STACKER_1);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Message Queue | [cs_msg.md](cs_msg.md) | Inter-process communication |
| Mapped Memory | [cs_mpm.md](cs_mpm.md) | Shared memory |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

