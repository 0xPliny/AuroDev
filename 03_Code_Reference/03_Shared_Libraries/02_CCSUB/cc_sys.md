# cc_sys - System Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_sys.cpp`, `cc_sys.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_sys` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_sys` class provides **system-wide control and status management**. Handles warm start, system state, and global configuration.

**Critical Operations:**
- System initialization
- Warm start coordination
- System state management
- Global configuration access

---

## Key Methods

### cc_sys::Initialize

**Signature:**
```cpp
long Initialize(const char* area)
```

**Description:**
Initializes system control structures for a specific area.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `area` | `const char*` | Area identifier |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - system initialized |
| `-1` | `GP.BAD` | Failure - initialization error |

---

### cc_sys::GetWarmStatus

**Signature:**
```cpp
long GetWarmStatus(const char* area, bool* isWarm)
```

**Description:**
Gets warm start status for an area.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `area` | `const char*` | Area identifier |
| `isWarm` | `bool*` | Output - true if warm, false if cold |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - status retrieved |
| `-1` | `GP.BAD` | Failure - area not found |

---

## Common Usage Patterns

### Pattern 1: System Initialization

```cpp
if (cc_sys.Initialize("A") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "System initialized for area A");
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to initialize system");
    return GP.BAD;
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| System Startup | [System Startup](../../05_Workflows/04_System_Startup_Shutdown.md) | Warm Start |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

