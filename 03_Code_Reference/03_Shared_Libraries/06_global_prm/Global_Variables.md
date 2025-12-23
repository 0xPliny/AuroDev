# Global Variables - global_prm

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

## External Variables

| Variable | Type | Description |
|----------|------|-------------|
| `gg_text1` - `gg_text5` | `char[512]` | Temporary text buffers |
| `gg_prgnam` | `char[IL_FILNAM_ONE]` | Process name |
| `gg_filename` | `char*` | Used by cs_ioerr |
| `gg_errno` | `long` | System errno value |
| `gg_errmsg` | `long` | Error message number |
| `gg_line` | `long` | Current line number |
| `gg_file` | `char*` | Current file name |
| `gg_areas` | `char[][2]` | Area codes array |

**Usage:**
```cpp
cc_str.copy(gg_text1, "Processing...", sizeof(gg_text1));
cs_log_printf(GP_LOGLVL_3, "%s", gg_text1);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Logging | [cs_log_printf.md](../03_CSUB/cs_log_printf.md) | Uses gg_file, gg_line |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

