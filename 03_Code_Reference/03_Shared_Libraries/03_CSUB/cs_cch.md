# cs_cch - Cache Management Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_cch.cpp` / `CS_CCH.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Cache Management |

---

## Overview

The `cs_cch` module provides caching functionality for frequently accessed data in the MHC/MEM system. It reduces database queries by storing commonly used records in memory, improving system performance.

**Key Responsibilities:**
- In-memory data caching
- Cache invalidation and refresh
- Cache hit/miss statistics
- Memory-efficient storage

---

## Key Functions

### cs_cch_init

**Signature:**
```cpp
long cs_cch_init(const char* cacheName, int maxEntries, int entrySize);
```

**Purpose:** Initialize a named cache with specified capacity.

| Parameter | Type | Description |
|-----------|------|-------------|
| `cacheName` | `const char*` | Cache identifier name |
| `maxEntries` | `int` | Maximum cache entries |
| `entrySize` | `int` | Size of each entry in bytes |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Cache initialized |
| `GP.BAD` | Initialization failed |

---

### cs_cch_get

**Signature:**
```cpp
long cs_cch_get(const char* cacheName, const char* key, void* data, int* dataSize);
```

**Purpose:** Retrieve an entry from cache by key.

| Parameter | Type | Description |
|-----------|------|-------------|
| `cacheName` | `const char*` | Cache identifier |
| `key` | `const char*` | Entry key |
| `data` | `void*` | Output: Data buffer |
| `dataSize` | `int*` | In/Out: Buffer size / actual size |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Cache hit - data returned |
| `GP.EMPTY` | Cache miss - not found |
| `GP.BAD` | Error |

---

### cs_cch_put

**Signature:**
```cpp
long cs_cch_put(const char* cacheName, const char* key, void* data, int dataSize);
```

**Purpose:** Store an entry in cache.

| Parameter | Type | Description |
|-----------|------|-------------|
| `cacheName` | `const char*` | Cache identifier |
| `key` | `const char*` | Entry key |
| `data` | `void*` | Data to cache |
| `dataSize` | `int` | Size of data |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Entry cached |
| `GP.FULL` | Cache full |
| `GP.BAD` | Error |

---

### cs_cch_invalidate

**Signature:**
```cpp
long cs_cch_invalidate(const char* cacheName, const char* key);
```

**Purpose:** Invalidate (remove) a specific cache entry.

| Parameter | Type | Description |
|-----------|------|-------------|
| `cacheName` | `const char*` | Cache identifier |
| `key` | `const char*` | Entry key (NULL for all) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Entry invalidated |
| `GP.BAD` | Error |

---

### cs_cch_flush

**Signature:**
```cpp
long cs_cch_flush(const char* cacheName);
```

**Purpose:** Flush all entries from a cache.

| Parameter | Type | Description |
|-----------|------|-------------|
| `cacheName` | `const char*` | Cache identifier (NULL for all caches) |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Cache flushed |
| `GP.BAD` | Error |

---

## Common Usage Patterns

### Pattern 1: Initialize and Use Location Cache

```cpp
// Initialize location cache
cs_cch_init("LOCN_CACHE", 1000, sizeof(LOCN));

// Try to get from cache first
LOCN locn;
int size = sizeof(locn);

if (cs_cch_get("LOCN_CACHE", locationKey, &locn, &size) == GP.EMPTY) {
    // Cache miss - get from database
    os_locn.select_by_key(locationKey, &locn);
    
    // Add to cache
    cs_cch_put("LOCN_CACHE", locationKey, &locn, sizeof(locn));
}
```

### Pattern 2: Invalidate Cache on Update

```cpp
// Update location in database
os_locn.update(&locn);

// Invalidate cached entry
cs_cch_invalidate("LOCN_CACHE", locn.location);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Mapped Memory | [cc_mem.md](../02_CCSUB/cc_mem.md) | Memory management |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

