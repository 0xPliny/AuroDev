# cs_txt - Text Translation Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_txt.cpp` / `CS_TXT.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Text Translation |

---

## Overview

The `cs_txt` module provides text translation and message formatting functions. It retrieves messages from the message file (english.msg) and formats them with parameters for logging and display.

**Key Responsibilities:**
- Load and parse message files
- Retrieve messages by message number
- Format messages with parameters
- Support multiple languages (via different message files)

---

## Key Functions

### cs_txt_init

**Signature:**
```cpp
long cs_txt_init(const char* msgFile);
```

**Purpose:** Initialize the text translation system.

| Parameter | Type | Description |
|-----------|------|-------------|
| `msgFile` | `const char*` | Path to message file (e.g., "english.msg") |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Initialization successful |
| `GP.BAD` | File not found or parse error |

---

### cs_txt_get

**Signature:**
```cpp
long cs_txt_get(int msgNum, char* buffer, int bufSize);
```

**Purpose:** Get a message by message number.

| Parameter | Type | Description |
|-----------|------|-------------|
| `msgNum` | `int` | Message number |
| `buffer` | `char*` | Output buffer |
| `bufSize` | `int` | Buffer size |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Message retrieved |
| `GP.EMPTY` | Message not found |
| `GP.BAD` | Error |

---

### cs_txt_format

**Signature:**
```cpp
long cs_txt_format(int msgNum, char* buffer, int bufSize, ...);
```

**Purpose:** Get and format a message with parameters.

| Parameter | Type | Description |
|-----------|------|-------------|
| `msgNum` | `int` | Message number |
| `buffer` | `char*` | Output buffer |
| `bufSize` | `int` | Buffer size |
| `...` | `varargs` | Format parameters |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Message formatted |
| `GP.EMPTY` | Message not found |
| `GP.BAD` | Error |

---

### cs_getxt

**Signature:**
```cpp
char* cs_getxt(int msgNum);
```

**Purpose:** Get message text (returns static buffer).

| Parameter | Type | Description |
|-----------|------|-------------|
| `msgNum` | `int` | Message number |

| Return Value | Description |
|--------------|-------------|
| `char*` | Pointer to message text |
| `NULL` | Message not found |

**Note:** Uses static buffer - copy if needed for persistence.

---

## Message File Format

The message file (`english.msg`) uses the following format:

```
# Comment lines start with #
MSG_NUM|Message text with %s and %d placeholders
1001|Load [%s] arrived at station [%s]
1002|Error %d occurred: %s
1003|Move %d completed successfully
```

---

## Common Usage Patterns

### Pattern 1: Get Simple Message

```cpp
char buffer[256];

if (cs_txt_get(1001, buffer, sizeof(buffer)) == GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, "%s", buffer);
}
```

### Pattern 2: Format Message with Parameters

```cpp
char buffer[256];

cs_txt_format(1001, buffer, sizeof(buffer), loadId, stationName);
// Result: "Load [ABC123] arrived at station [ST01]"

cs_log_printf(FILELINE, GP_LOGLVL_3, "%s", buffer);
```

### Pattern 3: Quick Message Retrieval

```cpp
// Quick access (uses static buffer)
char* msg = cs_getxt(1003);
if (msg != NULL) {
    cs_log_printf(FILELINE, GP_LOGLVL_3, msg, moveId);
}
```

### Pattern 4: Error Message Display

```cpp
// Format error message
char errMsg[512];
cs_txt_format(errCode, errMsg, sizeof(errMsg), 
              deviceName, errorDescription);

// Display to operator
ShowErrorDialog(errMsg);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Logging | [cs_log_printf.md](cs_log_printf.md) | Log message formatting |
| Error Handling | [cs_err.md](cs_err.md) | Error code translation |
| File Paths | [File_Path_Definitions.md](../06_global_prm/File_Path_Definitions.md) | GP_TXTDEF_FILE |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

