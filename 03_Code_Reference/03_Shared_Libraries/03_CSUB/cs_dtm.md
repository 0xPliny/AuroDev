# cs_dtm - Date/Time Manipulation Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_dtm.cpp`, `cs_dtm.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\` |
| **Module Name** | `cs_dtm` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cs_dtm` module provides **date/time conversion and formatting functions**. Handles conversion between database formats, internal formats, and display formats.

**Critical Operations:**
- Get current system date/time
- Format date/time to string
- Convert between database and internal formats
- Date/time arithmetic

**Key Principle:** MHC uses **internal date/time format** (double) for calculations, and converts to/from database and display formats as needed.

---

## Key Functions

### cs_dtm_current

**Signature:**
```cpp
void cs_dtm_current(double *date)
```

**Description:**
Gets the current system date/time in internal format (double).

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `date` | `double*` | Output - current date/time in internal format |

**Return Values:**
- None (void function)

**Logic Flow:**
1. Gets current system time using Windows API (`GetSystemTime`, `SystemTimeToFileTime`)
2. Converts to internal format (double representing days since epoch)
3. Stores result in `*date`

**Usage Example:**
```cpp
double curDate;
cs_dtm_current(&curDate);

char dateStr[64];
cs_dtm_fmt(dateStr, sizeof(dateStr), curDate, DTM_MASK);
cs_log_printf(FILELINE, CS_LOG_INFO, "Current date/time: %s", dateStr);
```

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp` (implementation)

---

### cs_dtm_fmt

**Signature:**
```cpp
void cs_dtm_fmt(char *str, size_t size, double date, char *format)
```

**Description:**
Formats an internal date/time value to a string using the specified format.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `str` | `char*` | Output buffer |
| `size` | `size_t` | Buffer size in bytes |
| `date` | `double` | Internal date/time value |
| `format` | `char*` | Format string (e.g., `DTM_MASK`, `DTM_SQL_DATE`) |

**Return Values:**
- None (void function)

**Format Constants:**
| Constant | Format | Example |
|----------|--------|---------|
| `DTM_MASK` | Standard display format | `"12/23/2024 14:30:45"` |
| `DTM_SQL_DATE` | SQL Server format | `"2024-12-23 14:30:45.123"` |
| `DTM_DATE_ONLY` | Date only | `"12/23/2024"` |
| `DTM_TIME_ONLY` | Time only | `"14:30:45"` |

**Logic Flow:**
1. Converts internal format (double) to system time structure
2. Formats according to `format` string
3. Copies formatted string to `str` buffer (truncates if too long)

**Usage Example:**
```cpp
double curDate;
cs_dtm_current(&curDate);

char dateStr[64];
cs_dtm_fmt(dateStr, sizeof(dateStr), curDate, DTM_MASK);
cs_log_printf(FILELINE, CS_LOG_INFO, "Formatted date: %s", dateStr);

// For SQL Server
char sqlDateStr[64];
cs_dtm_fmt(sqlDateStr, sizeof(sqlDateStr), curDate, DTM_SQL_DATE);
// Use sqlDateStr in SQL queries
```

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp` (implementation)

---

### cs_dtm_internal

**Signature:**
```cpp
void cs_dtm_internal(double *date, double db_date)
```

**Description:**
Converts a database date/time value to internal format.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `date` | `double*` | Output - internal date/time |
| `db_date` | `double` | Database date/time value |

**Return Values:**
- None (void function)

**Logic Flow:**
1. Converts database format to internal format
2. Stores result in `*date`

**Usage Example:**
```cpp
MOVS movs;
// ... select move record ...
double internalDate;
cs_dtm_internal(&internalDate, movs.add_date);

char dateStr[64];
cs_dtm_fmt(dateStr, sizeof(dateStr), internalDate, DTM_MASK);
cs_log_printf(FILELINE, CS_LOG_INFO, "Move created: %s", dateStr);
```

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp` (implementation)

---

### cs_dtm_database

**Signature:**
```cpp
void cs_dtm_database(double *db_date, double date)
```

**Description:**
Converts an internal date/time value to database format.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `db_date` | `double*` | Output - database date/time |
| `date` | `double` | Internal date/time value |

**Return Values:**
- None (void function)

**Usage Example:**
```cpp
double curDate;
cs_dtm_current(&curDate);

double dbDate;
cs_dtm_database(&dbDate, curDate);

MOVS movs;
movs.add_date = dbDate;
// ... insert record ...
```

**Source Files:**
- `cs_dtm.h`
- `cs_dtm.cpp` (implementation)

---

## Common Usage Patterns

### Pattern 1: Get Current Date/Time for Logging

```cpp
double curDate;
cs_dtm_current(&curDate);

char dateStr[64];
cs_dtm_fmt(dateStr, sizeof(dateStr), curDate, DTM_MASK);
cs_log_printf(FILELINE, CS_LOG_INFO, "Operation started at %s", dateStr);
```

### Pattern 2: Set Add Date on Record Creation

```cpp
MOVS movs;
MOVS_init(&movs);

double curDate;
cs_dtm_current(&curDate);
cs_dtm_database(&movs.add_date, curDate);

// ... set other fields ...
MOVS_insert(&movs);
```

### Pattern 3: Format Date for Display

```cpp
MOVS movs;
// ... select record ...

double internalDate;
cs_dtm_internal(&internalDate, movs.add_date);

char dateStr[64];
cs_dtm_fmt(dateStr, sizeof(dateStr), internalDate, DTM_MASK);
// Display dateStr in UI or log
```

### Pattern 4: Calculate Time Difference

```cpp
double startDate, endDate;
cs_dtm_current(&startDate);

// ... perform operation ...

cs_dtm_current(&endDate);
double duration = endDate - startDate; // Duration in days

char durationStr[64];
sprintf(durationStr, "%.3f seconds", duration * 86400.0); // Convert to seconds
cs_log_printf(FILELINE, CS_LOG_INFO, "Operation took %s", durationStr);
```

---

## Date/Time Format Details

**Internal Format (double):**
- Represents days since epoch (typically January 1, 1900 or 1970)
- Fractional part represents time of day
- Example: `45678.5` = day 45678 at 12:00:00

**Database Format (double):**
- SQL Server datetime format
- Compatible with SQL Server's datetime data type
- May have different epoch or precision

**Display Format (string):**
- Human-readable format
- Format controlled by format constants
- Examples: `"12/23/2024 14:30:45"`, `"2024-12-23 14:30:45.123"`

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Database Access | [OSUB Functions](../01_OSUB/os_movs.md) | Date Fields |
| Logging | [cs_log_printf.md](cs_log_printf.md) | Timestamp Formatting |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

