# File Path Definitions - global_prm

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

## Configuration File Paths

| Constant | Format | Description |
|----------|--------|-------------|
| `GP_DBINI_FILE` | `"%s\\file\\dbini_%s.cnf"` | Database initialization |
| `GP_TXTDEF_FILE` | `"\\file\\english.msg"` | Default text file |
| `GP_TTYDEF_FILE` | `"%s\\file\\ttydef_%s_%s.cnf"` | TTY definition |
| `GP_LOGDEF_FILE` | `"\\file\\logdef.cnf"` | Log definition |
| `GP_FIL_LOGCNF` | `"\\file\\logdef.cnf"` | Log config file |
| `GP_FIL_WCHCNF` | `"\\file\\watchdef.cnf"` | Watchdog config |
| `GP_MPMEM_FILE` | `"\\shmf\\mpmem.dat"` | Mapped memory save file |
| `GP_FIL_ACTCNF` | `"\\file\\act.cnf"` | Action config |
| `GP_FIL_ERRCNF` | `"\\file\\err.cnf"` | Error config |
| `GP_MAP_ERRCTL` | `"\\shmf\\errctl"` | Error control map file |

**Usage:**
```cpp
char dbiniPath[256];
sprintf(dbiniPath, GP_DBINI_FILE, sysbase, area);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Configuration Files | [Configuration Guide](../../04_Database_Reference/03_Configuration_Files.md) | File paths |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

