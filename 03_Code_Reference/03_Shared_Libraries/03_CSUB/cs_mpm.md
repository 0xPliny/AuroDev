# cs_mpm - Mapped Memory Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_mpm.cpp` / `CS_MPM.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Mapped Memory |

---

## Overview

The `cs_mpm` module provides mapped memory (shared memory) management functions. Mapped memory allows multiple processes to share data efficiently without database queries or IPC overhead.

**Key Responsibilities:**
- Create and manage mapped memory segments
- Attach/detach processes to shared memory
- Persist shared memory to disk
- Handle memory protection

---

## Key Functions

### cs_mpm_create

**Signature:**
```cpp
long cs_mpm_create(const char* name, int size, void** ptr);
```

**Purpose:** Create a new mapped memory segment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Segment name |
| `size` | `int` | Segment size in bytes |
| `ptr` | `void**` | Output: Pointer to memory |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Segment created |
| `GP.BAD` | Creation failed |

---

### cs_mpm_attach

**Signature:**
```cpp
long cs_mpm_attach(const char* name, void** ptr);
```

**Purpose:** Attach to an existing mapped memory segment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Segment name |
| `ptr` | `void**` | Output: Pointer to memory |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Attached successfully |
| `GP.BAD` | Segment not found or error |

---

### cs_mpm_detach

**Signature:**
```cpp
long cs_mpm_detach(const char* name);
```

**Purpose:** Detach from a mapped memory segment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Segment name |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Detached successfully |
| `GP.BAD` | Error |

---

### cs_mpm_save

**Signature:**
```cpp
long cs_mpm_save(const char* name, const char* filename);
```

**Purpose:** Save mapped memory to disk file.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Segment name |
| `filename` | `const char*` | Output file path |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Saved successfully |
| `GP.BAD` | Save failed |

---

### cs_mpm_load

**Signature:**
```cpp
long cs_mpm_load(const char* name, const char* filename);
```

**Purpose:** Load mapped memory from disk file.

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | `const char*` | Segment name |
| `filename` | `const char*` | Input file path |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Loaded successfully |
| `GP.BAD` | Load failed |

---

## Common Usage Patterns

### Pattern 1: Create Shared Memory for Equipment Data

```cpp
void* equipmentData;

// Create shared memory segment
if (cs_mpm_create("EQUIPMENT_DATA", sizeof(EQUIPMENT_ARRAY), &equipmentData) == GP.GOOD) {
    // Initialize data
    memset(equipmentData, 0, sizeof(EQUIPMENT_ARRAY));
    cs_log_printf(FILELINE, GP_LOGLVL_3, "Equipment shared memory created");
}
```

### Pattern 2: Attach to Existing Shared Memory

```cpp
void* stkData;

// Attach to stacker data segment
if (cs_mpm_attach("STK_DATA", &stkData) == GP.GOOD) {
    STACKER_DATA* stackers = (STACKER_DATA*)stkData;
    // Access stacker data...
}
```

### Pattern 3: Persist to Disk on Shutdown

```cpp
// Save shared memory before shutdown
cs_mpm_save("EQUIPMENT_DATA", GP_MPMEM_FILE);
cs_log_printf(FILELINE, GP_LOGLVL_2, "Shared memory saved to disk");
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| cc_mem Class | [cc_mem.md](../02_CCSUB/cc_mem.md) | High-level memory management |
| System Parameters | [File_Path_Definitions.md](../06_global_prm/File_Path_Definitions.md) | GP_MPMEM_FILE |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

