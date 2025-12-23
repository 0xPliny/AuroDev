# Macro Definitions - global_prm

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `global_prm.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\incl\global_prm.h` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Trace Macros

| Macro | Description | Output |
|-------|-------------|--------|
| `GP_TRACE()` | Trace current location | File and line |
| `GP_TRACE_MSG(message)` | Trace with message | Message + location |
| `GP_UNUSED(val)` | Suppress unused warnings | None |
| `FILELINE` | Get file:line string | Formatted string |

**Usage:**
```cpp
GP_TRACE();  // Log current file and line
GP_TRACE_MSG("Processing started");
GP_UNUSED(unusedParam);  // Suppress warning
```

---

## Error Logging Macro

```cpp
#define cs_erlog(text, msgnum) \
    gg_file = __FILE__; \
    gg_line = __LINE__; \
    cs_erlog2(text, msgnum);
```

**Usage:**
```cpp
cs_erlog("Error occurred", 1001);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Logging | [cs_log_printf.md](../03_CSUB/cs_log_printf.md) | Logging functions |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

