# cs_reg - Windows Registry Access Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_reg.cpp`, `cs_reg.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Module Name** | `cs_reg` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cs_reg` module provides functions to read and write Windows Registry values. Used for configuration storage that persists across application restarts.

**Critical Operations:**
- Read string values from registry
- Write string values to registry
- Registry key management

**Key Principle:** **ALWAYS load configuration from registry** - NEVER hardcode values.

**Registry Path:** `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\{group}\{title}`

---

## Key Functions

### cs_reg_read

**Signature:**
```cpp
long cs_reg_read(char *group, char *title, char *reg, size_t size_reg)
```

**Description:**
Reads a string value from the Windows Registry under `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\{group}\{title}`.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `group` | `char*` | Registry group (e.g., "KTH_ASRS", "AuroDev") |
| `title` | `char*` | Value name (e.g., "MaxRetries", "CellaApiUrl") |
| `reg` | `char*` | Buffer to receive value |
| `size_reg` | `size_t` | Size of buffer in bytes |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - value read |
| `-1` | `GP.BAD` | Failure - key not found, value not found, or error |

**Logic Flow:**
1. Opens registry key: `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\{group}`
2. If key not found, returns `GP.BAD`
3. Reads value `{title}` from the key
4. If value not found, returns `GP.BAD`
5. Copies value to `reg` buffer (truncates if too long)
6. Closes registry key
7. Returns `GP.GOOD`

**Dependencies:**
- Windows API: `RegOpenKeyEx`, `RegQueryValueEx`, `RegCloseKey`

**Usage Example:**
```cpp
int maxRetries = 3; // Default value
char maxRetriesStr[32];
if (cs_reg_read("KTH_ASRS", "MaxRetries", maxRetriesStr, sizeof(maxRetriesStr)) == GP.GOOD) {
    maxRetries = atoi(maxRetriesStr);
}

char apiUrl[256] = "";
if (cs_reg_read("KTH_ASRS", "CellaApiUrl", apiUrl, sizeof(apiUrl)) != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_WARNING, "CellaApiUrl not found in registry, using default");
    cc_str.copy(apiUrl, sizeof(apiUrl), "https://cella.example.com/api");
}
```

**Source Files:**
- `cs_reg.h` (line 28)
- `cs_reg.cpp` (implementation)

---

### cs_reg_write

**Signature:**
```cpp
long cs_reg_write(char *group, char *title, char *reg)
```

**Description:**
Writes a string value to the Windows Registry. Creates the key if it doesn't exist.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `group` | `char*` | Registry group |
| `title` | `char*` | Value name |
| `reg` | `char*` | Value to write (string) |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - value written |
| `-1` | `GP.BAD` | Failure - write error or insufficient permissions |

**Logic Flow:**
1. Opens or creates registry key: `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\{group}`
2. If key creation fails, returns `GP.BAD`
3. Writes value `{title}` = `reg` to the key
4. Closes registry key
5. Returns `GP.GOOD`

**Usage Example:**
```cpp
char apiUrl[256];
cc_str.copy(apiUrl, sizeof(apiUrl), "https://cella.example.com/api");
if (cs_reg_write("KTH_ASRS", "CellaApiUrl", apiUrl) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Updated CellaApiUrl in registry");
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to write CellaApiUrl to registry");
}
```

**Source Files:**
- `cs_reg.h` (line 27)
- `cs_reg.cpp` (implementation)

---

## Common Usage Patterns

### Pattern 1: Read Configuration with Default

```cpp
int maxRetries = 3; // Default
char maxRetriesStr[32];
if (cs_reg_read("KTH_ASRS", "MaxRetries", maxRetriesStr, sizeof(maxRetriesStr)) == GP.GOOD) {
    maxRetries = atoi(maxRetriesStr);
    cs_log_printf(FILELINE, CS_LOG_INFO, "MaxRetries from registry: %d", maxRetries);
} else {
    cs_log_printf(FILELINE, CS_LOG_INFO, "MaxRetries not in registry, using default: %d", 
                  maxRetries);
}
```

### Pattern 2: Read Multiple Configuration Values

```cpp
char apiUrl[256] = "";
char apiKey[128] = "";
int timeout = 5000;

if (cs_reg_read("KTH_ASRS", "CellaApiUrl", apiUrl, sizeof(apiUrl)) != GP.GOOD) {
    cc_str.copy(apiUrl, sizeof(apiUrl), "https://cella.example.com/api");
}

if (cs_reg_read("KTH_ASRS", "CellaApiKey", apiKey, sizeof(apiKey)) != GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "CellaApiKey not found in registry");
    return GP.BAD;
}

char timeoutStr[32];
if (cs_reg_read("KTH_ASRS", "Timeout", timeoutStr, sizeof(timeoutStr)) == GP.GOOD) {
    timeout = atoi(timeoutStr);
}
```

### Pattern 3: Write Configuration

```cpp
// Update configuration after user changes
char newApiUrl[256];
cc_str.copy(newApiUrl, sizeof(newApiUrl), userInput);

if (cs_reg_write("KTH_ASRS", "CellaApiUrl", newApiUrl) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Updated CellaApiUrl in registry: %s", newApiUrl);
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Failed to update registry (may need admin rights)");
    return GP.BAD;
}
```

---

## Registry Structure

**Base Path:** `HKEY_LOCAL_MACHINE\SOFTWARE\Muratec\`

**Example Structure:**
```
HKEY_LOCAL_MACHINE
  SOFTWARE
    Muratec
      KTH_ASRS
        CellaApiUrl = "https://cella.example.com/api"
        CellaApiKey = "abc123..."
        MaxRetries = "3"
        Timeout = "5000"
      AuroDev
        DatabaseServer = "SQLSRV01"
        DatabaseName = "AuroDB"
```

---

## Permissions

- **Reading:** Requires read access to registry (typically available to all users)
- **Writing:** Requires write access to `HKEY_LOCAL_MACHINE` (typically requires administrator privileges)

**Best Practice:** Write registry values during installation or configuration, read them at runtime.

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Configuration Files | [Configuration Guide](../../04_Database_Reference/03_Configuration_Files.md) | Registry vs Files |
| Elements Table | [cs_elt.md](cs_elt.md) | Database-based configuration |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

