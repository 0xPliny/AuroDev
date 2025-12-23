# Program Name Definitions - global_prm

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

## Background Process Names

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_prgnam_FNDWK` | `"p_ar_fndwk"` | Find Work |
| `GP_prgnam_MOVDP` | `"p_ar_movdp"` | Move Dispatcher |
| `GP_prgnam_MODE` | `"p_ar_modechange"` | Mode Change |
| `GP_prgnam_STKDP` | `"p_ar_stkdpQ"` | Stacker Dispatcher |
| `GP_prgnam_HOSTIN` | `"p_ar_hostin"` | Host In |
| `GP_prgnam_HOSTOUT` | `"p_ar_hostout"` | Host Out |
| `GP_prgnam_STKDPU` | `"p_ar_stkdpU"` | Stacker Dispatcher UDP |
| `GP_prgnam_SRCMP` | `"p_ar_srcmp"` | Stacker Completer |
| `GP_prgnam_SRCMPU` | `"p_ar_srcmpU"` | Stacker Completer UDP |
| `GP_prgnam_STKCM` | `"p_cc_stkcmx"` | Stacker Communication |
| `GP_prgnam_MUDPCM` | `"p_cc_mudpcm"` | Vehicle Communication |
| `GP_prgnam_ALLOC` | `"p_ar_alloc"` | Stand Allocator |
| `GP_prgnam_EVENT` | `"p_ar_events"` | Events |
| `GP_prgnam_AGVCP` | `"p_ar_agvcp"` | AGV Completer |

**Usage:**
```cpp
cs_log_printf(FILELINE, CS_LOG_INFO, "Starting process: %s", GP_prgnam_MOVDP);
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Background Processes | [Background Processes](../../01_System_Architecture/02_Background_Processes.md) | Process list |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

