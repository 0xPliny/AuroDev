# cs_tmr - Timer Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_tmr.cpp` / `CS_TMR.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Timer Management |

---

## Overview

The `cs_tmr` module provides timer management functions for time-based operations in the MHC/MEM system. It supports interval timers, one-shot timers, and elapsed time measurement.

**Key Responsibilities:**
- Create and manage timers
- Timeout detection
- Elapsed time measurement
- Periodic callbacks

---

## Key Functions

### cs_tmr_create

**Signature:**
```cpp
long cs_tmr_create(int timerId, int intervalMs, int timerType);
```

**Purpose:** Create a new timer.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Unique timer identifier |
| `intervalMs` | `int` | Interval in milliseconds |
| `timerType` | `int` | 0=one-shot, 1=repeating |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Timer created |
| `GP.BAD` | Creation failed |

---

### cs_tmr_start

**Signature:**
```cpp
long cs_tmr_start(int timerId);
```

**Purpose:** Start a timer.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Timer identifier |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Timer started |
| `GP.BAD` | Timer not found |

---

### cs_tmr_stop

**Signature:**
```cpp
long cs_tmr_stop(int timerId);
```

**Purpose:** Stop a running timer.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Timer identifier |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Timer stopped |
| `GP.BAD` | Timer not found |

---

### cs_tmr_expired

**Signature:**
```cpp
bool cs_tmr_expired(int timerId);
```

**Purpose:** Check if a timer has expired.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Timer identifier |

| Return Value | Description |
|--------------|-------------|
| `true` | Timer has expired |
| `false` | Timer still running |

---

### cs_tmr_elapsed

**Signature:**
```cpp
long cs_tmr_elapsed(int timerId);
```

**Purpose:** Get elapsed time since timer started.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Timer identifier |

| Return Value | Description |
|--------------|-------------|
| `>= 0` | Elapsed milliseconds |
| `-1` | Timer not found |

---

### cs_tmr_reset

**Signature:**
```cpp
long cs_tmr_reset(int timerId);
```

**Purpose:** Reset timer to start from zero.

| Parameter | Type | Description |
|-----------|------|-------------|
| `timerId` | `int` | Timer identifier |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Timer reset |
| `GP.BAD` | Timer not found |

---

## Common Usage Patterns

### Pattern 1: Communication Timeout

```cpp
#define COMM_TIMEOUT_TIMER 1
#define COMM_TIMEOUT_MS 5000

// Create timeout timer
cs_tmr_create(COMM_TIMEOUT_TIMER, COMM_TIMEOUT_MS, 0);  // one-shot

// Start when waiting for response
cs_tmr_start(COMM_TIMEOUT_TIMER);

// In polling loop
while (!responseReceived) {
    if (cs_tmr_expired(COMM_TIMEOUT_TIMER)) {
        cs_log_printf(FILELINE, GP_LOGLVL_1, "Communication timeout");
        break;
    }
    // Check for response...
}
```

### Pattern 2: Periodic Statistics Collection

```cpp
#define STATS_TIMER 2
#define STATS_INTERVAL_MS 30000  // 30 seconds

// Create repeating timer
cs_tmr_create(STATS_TIMER, STATS_INTERVAL_MS, 1);  // repeating
cs_tmr_start(STATS_TIMER);

// In main loop
if (cs_tmr_expired(STATS_TIMER)) {
    collectStatistics();
    cs_tmr_reset(STATS_TIMER);
}
```

### Pattern 3: Measure Operation Duration

```cpp
#define MEASURE_TIMER 3

cs_tmr_create(MEASURE_TIMER, 0, 0);  // Just for measurement
cs_tmr_start(MEASURE_TIMER);

// Perform operation
performDatabaseOperation();

long elapsed = cs_tmr_elapsed(MEASURE_TIMER);
cs_log_printf(FILELINE, GP_LOGLVL_4, "Operation took %ld ms", elapsed);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Date/Time | [cs_dtm.md](cs_dtm.md) | Date/time functions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

