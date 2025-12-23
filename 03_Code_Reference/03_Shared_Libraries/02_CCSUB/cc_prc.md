# cc_prc - Process Control Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_prc.cpp`, `cc_prc.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_prc` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_prc` class provides **process control and management** functions. Handles process table management, process status, and inter-process coordination.

**Critical Operations:**
- Process table management
- Process status tracking
- Process heartbeat monitoring
- Process coordination

---

## Key Methods

### cc_prc::Register

**Signature:**
```cpp
long Register(const char* processName)
```

**Description:**
Registers a process in the process table.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `processName` | `const char*` | Process name (e.g., "p_ar_movdp") |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - process registered |
| `-1` | `GP.BAD` | Failure - registration error |

---

### cc_prc::UpdateHeartbeat

**Signature:**
```cpp
long UpdateHeartbeat(const char* processName)
```

**Description:**
Updates process heartbeat timestamp.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `processName` | `const char*` | Process name |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - heartbeat updated |
| `-1` | `GP.BAD` | Failure - process not found |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Process Management | [Process Control](../../05_Workflows/02_Inter_Process_Communication.md) | Process Table |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

