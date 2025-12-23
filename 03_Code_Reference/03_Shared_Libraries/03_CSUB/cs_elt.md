# cs_elt - Element/Configuration Access Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_elt.cpp`, `cs_elt.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Module Name** | `cs_elt` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cs_elt` module provides **access to configuration elements** stored in the database `ELEMENTS` table. Used for site-specific configuration values that can be changed without code modifications.

**Critical Operations:**
- Read element values from database
- Write element values to database
- Element group management
- Configuration caching

**Key Principle:** Elements provide **flexible configuration** stored in database rather than hardcoded values.

---

## Key Functions

### cs_elt_read

**Signature:**
```cpp
long cs_elt_read(const char* group, const char* element, char* value, size_t valueSize)
```

**Description:**
Reads an element value from the `ELEMENTS` table by group and element name.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `group` | `const char*` | Element group name (e.g., "KTH_ASRS", "Cella") |
| `element` | `const char*` | Element name (e.g., "MaxRetries", "ApiUrl") |
| `value` | `char*` | Output buffer for element value |
| `valueSize` | `size_t` | Size of value buffer |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - value read |
| `-1` | `GP.BAD` | Failure - element not found or database error |

**Logic Flow:**
1. Queries `ELEMENTS` table: `SELECT value FROM MHC_ELEMENTS WHERE group = '{group}' AND element = '{element}'`
2. If found, copies value to buffer
3. Returns `GP.GOOD`
4. If not found, returns `GP.BAD`

**Usage Example:**
```cpp
char apiUrl[256] = "";
if (cs_elt_read("KTH_ASRS", "CellaApiUrl", apiUrl, sizeof(apiUrl)) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Cella API URL: %s", apiUrl);
} else {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "CellaApiUrl not found, using default");
    cc_str.copy(apiUrl, sizeof(apiUrl), "https://cella.example.com/api");
}
```

---

### cs_elt_write

**Signature:**
```cpp
long cs_elt_write(const char* group, const char* element, const char* value)
```

**Description:**
Writes an element value to the `ELEMENTS` table. Creates element if it doesn't exist.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `group` | `const char*` | Element group name |
| `element` | `const char*` | Element name |
| `value` | `const char*` | Value to write |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - value written |
| `-1` | `GP.BAD` | Failure - database error |

**Usage Example:**
```cpp
if (cs_elt_write("KTH_ASRS", "CellaApiUrl", "https://cella.newurl.com/api") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Updated CellaApiUrl element");
}
```

---

## Common Usage Patterns

### Pattern 1: Read Configuration with Default

```cpp
int maxRetries = 3; // Default
char maxRetriesStr[32];
if (cs_elt_read("KTH_ASRS", "MaxRetries", maxRetriesStr, sizeof(maxRetriesStr)) == GP.GOOD) {
    maxRetries = atoi(maxRetriesStr);
}
```

### Pattern 2: Read Multiple Elements

```cpp
char apiUrl[256] = "";
char apiKey[128] = "";
int timeout = 5000;

cs_elt_read("KTH_ASRS", "CellaApiUrl", apiUrl, sizeof(apiUrl));
cs_elt_read("KTH_ASRS", "CellaApiKey", apiKey, sizeof(apiKey));

char timeoutStr[32];
if (cs_elt_read("KTH_ASRS", "Timeout", timeoutStr, sizeof(timeoutStr)) == GP.GOOD) {
    timeout = atoi(timeoutStr);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Registry Access | [cs_reg.md](cs_reg.md) | Alternative configuration method |
| Elements Table | [os_elem.md](../01_OSUB/os_elem.md) | Database table structure |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

