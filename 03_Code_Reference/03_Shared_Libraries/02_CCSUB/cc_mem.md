# cc_mem - Mapped Memory Management Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_mem.cpp`, `cc_mem.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_mem` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_mem` class provides **mapped memory management** for inter-process communication. Handles creation, mapping, and access to shared memory files.

**Critical Operations:**
- Create mapped memory files
- Map memory files into process address space
- Unmap memory files
- Memory synchronization

---

## Key Methods

### cc_mem::Create

**Signature:**
```cpp
long Create(const char* fileName, size_t size)
```

**Description:**
Creates a new mapped memory file.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `fileName` | `const char*` | Memory file name |
| `size` | `size_t` | Size of memory in bytes |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - file created |
| `-1` | `GP.BAD` | Failure - creation error |

---

### cc_mem::Map

**Signature:**
```cpp
void* Map(const char* fileName, size_t* size)
```

**Description:**
Maps a memory file into process address space.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `fileName` | `const char*` | Memory file name |
| `size` | `size_t*` | Output - actual size of mapped memory |

**Return Values:**
| Return Type | Description |
|-------------|-------------|
| `void*` | Pointer to mapped memory, or NULL on failure |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Message Queues | [cs_msg.md](../03_CSUB/cs_msg.md) | Uses cc_mem |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

