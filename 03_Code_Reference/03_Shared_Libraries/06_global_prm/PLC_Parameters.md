# PLC Parameters - global_prm

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

## PLC Configuration

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_PLC` | `13` | Max PLCs configured |
| `GP_MAX_DSC` | `13680` | Max discretes in system |
| `GP_MAX_PLC_REGB` | `16` | Max bits in PLC registers |

---

## Discrete Ranges

| Type | Begin | Last | End | Description |
|------|-------|------|-----|-------------|
| **Input** | `GP_BEG_INPT` (1000) | `GP_LST_INPT` (14679) | `GP_END_INPT` (14680) | Input discretes |
| **Output** | `GP_BEG_OTPT` (15000) | `GP_LST_OTPT` (28679) | `GP_END_OTPT` (28680) | Output discretes |
| **Internal** | `GP_BEG_INTN` (29000) | `GP_LST_INTN` (42679) | `GP_END_INTN` (42680) | Internal discretes |

**Usage:**
```cpp
// Check if discrete is in input range
if (discrete >= GP_BEG_INPT && discrete <= GP_END_INPT) {
    // Input discrete
}
```

---

## PLC Type Flags

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_UseMXControls` | `1` | Use Mitsubishi MX Controls (Production) |
| `GP_UseMXControls_DEV` | `0` | Don't use MX Controls (Development) |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| PLC Communication | [cc_plc.md](../02_CCSUB/cc_plc.md) | PLC operations |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

