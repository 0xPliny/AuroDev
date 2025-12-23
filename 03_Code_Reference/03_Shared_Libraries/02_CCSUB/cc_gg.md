# cc_gg - Global Variables Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_gg.cpp`, `cc_gg.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_gg` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_gg` class provides **global variable access** for system-wide state and configuration. Provides access to global parameters stored in mapped memory.

**Critical Operations:**
- Global parameter access
- System-wide state management
- Configuration value access

---

## Key Methods

### cc_gg::GetParameter

**Signature:**
```cpp
long GetParameter(const char* paramName, void* value, size_t valueSize)
```

**Description:**
Gets a global parameter value.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `paramName` | `const char*` | Parameter name |
| `value` | `void*` | Output buffer for value |
| `valueSize` | `size_t` | Size of value buffer |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - parameter retrieved |
| `-1` | `GP.BAD` | Failure - parameter not found |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Global Parameters | [global_prm.md](../global_prm.md) | Parameter definitions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

