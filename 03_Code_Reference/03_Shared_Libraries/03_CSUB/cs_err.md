# cs_err - Error Handling Utilities

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_err.cpp`, `cs_err.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Module Name** | `cs_err` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cs_err` module provides **error code translation and error handling utilities**. Converts numeric error codes to human-readable messages and provides error recovery coordination.

**Critical Operations:**
- Error code to text translation
- Error message lookup
- Error logging coordination
- Error recovery coordination

---

## Key Functions

### cs_err_get_message

**Signature:**
```cpp
long cs_err_get_message(long errorCode, char* message, size_t messageSize)
```

**Description:**
Gets human-readable error message for an error code.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `errorCode` | `long` | Numeric error code |
| `message` | `char*` | Output buffer for error message |
| `messageSize` | `size_t` | Size of message buffer |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - message retrieved |
| `-1` | `GP.BAD` | Failure - error code not found |

**Usage Example:**
```cpp
long errorCode = GetLastError();
char errorMsg[256];
if (cs_err_get_message(errorCode, errorMsg, sizeof(errorMsg)) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Error %ld: %s", errorCode, errorMsg);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Unknown error code: %ld", errorCode);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Error Codes | [Error Codes](../../04_Database_Reference/05_Error_Codes.md) | Error code definitions |
| Logging | [cs_log_printf.md](cs_log_printf.md) | Error logging |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

