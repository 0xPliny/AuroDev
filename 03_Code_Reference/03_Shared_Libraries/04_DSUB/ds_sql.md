# ds_sql - SQL Interface Module

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_sql.cpp` / `ds_sql.h` |
| **Library** | DSUB (Database Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Status** | ✅ Active - Core Database Interface |

---

## Overview

The `ds_sql` module provides the core SQL interface for the MHC/MEM system. It wraps database operations, manages connections, handles transactions, and provides error handling for all database access.

**Key Responsibilities:**
- Database connection management
- Transaction control (start, commit, rollback)
- SQL execution and parameter binding
- Error handling and deadlock detection
- Connection pooling

---

## Key Functions

### SQL.start / SQL.commit / SQL.rollback

**Signature:**
```cpp
long SQL.start();
long SQL.commit();
long SQL.rollback();
```

**Purpose:** Transaction management functions.

| Function | Description |
|----------|-------------|
| `SQL.start()` | Begin a database transaction |
| `SQL.commit()` | Commit current transaction |
| `SQL.rollback()` | Rollback current transaction |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Operation successful |
| `GP.BAD` | Operation failed |

**Usage:**
```cpp
SQL.start();

// Database operations...
int rc = os_movs.insert(&move);
if (rc != GP.GOOD) {
    SQL.rollback();
    return GP.BAD;
}

SQL.commit();
return GP.GOOD;
```

---

### SQL.execute

**Signature:**
```cpp
long SQL.execute(const char* sql);
```

**Purpose:** Execute a SQL statement.

| Parameter | Type | Description |
|-----------|------|-------------|
| `sql` | `const char*` | SQL statement to execute |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Execution successful |
| `GP.BAD` | Execution failed |
| `GP.DBHELD` | Deadlock detected |

**Note:** Prefer generated OSUB functions over raw SQL.

---

### SQL.open_cursor / SQL.fetch / SQL.close_cursor

**Signature:**
```cpp
long SQL.open_cursor(const char* sql, CURSOR* cursor);
long SQL.fetch(CURSOR* cursor, void* record);
long SQL.close_cursor(CURSOR* cursor);
```

**Purpose:** Cursor-based record iteration.

| Function | Description |
|----------|-------------|
| `open_cursor` | Open cursor for query |
| `fetch` | Fetch next record |
| `close_cursor` | Close cursor |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Operation successful |
| `GP.EMPTY` | No more records |
| `GP.BAD` | Error |

---

### SQL.get_error

**Signature:**
```cpp
long SQL.get_error(char* errorMsg, int msgSize);
```

**Purpose:** Get last database error message.

| Parameter | Type | Description |
|-----------|------|-------------|
| `errorMsg` | `char*` | Output buffer |
| `msgSize` | `int` | Buffer size |

| Return Value | Description |
|--------------|-------------|
| Error code | Database error code |

---

### Ds_check_error

**Signature:**
```cpp
long Ds_check_error(long rc, const char* operation);
```

**Purpose:** Check and log database errors.

| Parameter | Type | Description |
|-----------|------|-------------|
| `rc` | `long` | Return code to check |
| `operation` | `const char*` | Operation description |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | No error |
| `GP.BAD` | Error logged |
| `GP.DBHELD` | Deadlock - retry recommended |

---

## Common Usage Patterns

### Pattern 1: Standard Transaction

```cpp
SQL.start();

// Perform operations
int rc = ds_movs_create(&move);
if (rc != GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_1, "Move creation failed");
    SQL.rollback();
    return GP.BAD;
}

rc = ds_load_update_location(&load, newLocation);
if (rc != GP.GOOD) {
    cs_log_printf(FILELINE, GP_LOGLVL_1, "Load update failed");
    SQL.rollback();
    return GP.BAD;
}

SQL.commit();
return GP.GOOD;
```

### Pattern 2: Error Handling with Deadlock Retry

```cpp
int maxRetries = 3;
int retryCount = 0;

while (retryCount < maxRetries) {
    SQL.start();
    
    int rc = performDatabaseOperation();
    
    if (rc == GP.GOOD) {
        SQL.commit();
        return GP.GOOD;
    } else if (rc == GP.DBHELD) {
        // Deadlock - retry
        SQL.rollback();
        retryCount++;
        Sleep(100);  // Brief delay before retry
    } else {
        SQL.rollback();
        return GP.BAD;
    }
}

cs_log_printf(FILELINE, GP_LOGLVL_1, "Operation failed after %d retries", maxRetries);
return GP.BAD;
```

### Pattern 3: Cursor-Based Query

```cpp
CURSOR cursor;
MOVS move;

// Open cursor for pending moves
SQL.open_cursor("SELECT * FROM MHC_MOVS WHERE Status = 'Pending'", &cursor);

while (SQL.fetch(&cursor, &move) == GP.GOOD) {
    // Process each move
    processPendingMove(&move);
}

SQL.close_cursor(&cursor);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Move Operations | [ds_movs.md](ds_movs.md) | Move functions |
| Load Operations | [ds_load.md](ds_load.md) | Load functions |
| OSUB Library | [00_OSUB_Index.md](../01_OSUB/00_OSUB_Index.md) | Generated DB access |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

