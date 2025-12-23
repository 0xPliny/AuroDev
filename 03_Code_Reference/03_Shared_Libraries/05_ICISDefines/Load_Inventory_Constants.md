# Load and Inventory Constants - ICISDefines

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ICISDefines.vb` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\ICISDefines.vb` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## LOAD.STATUS Class - Load Status

| Constant | Value | Description |
|----------|-------|-------------|
| `AVAL` | "Available" | Load is available |
| `COMT` | "Committed" | Load is committed |
| `INTRAN` | "Intransit" | Load is in transit |
| `OUTOFSYS` | "OutOfSys" | Load is out of system |
| `INPALL` | "InPall" | Load is in pallet |

**C++ Equivalent:** `GP.LOAD.STATUS` in `global_prm.h`

---

## LOAD.LOCATION Class - Special Location Codes

| Constant | Value | Description |
|----------|-------|-------------|
| `INTRAN` | "IN-TR-AN" | In-transit location |
| `OUTSYS` | "OU-TS-YS" | Out-of-system location |

**C++ Equivalent:** `GP_INTRAN_STR`, `GP_OUTSYS_STR` in `global_prm.h`

---

## INVT.STATUS Class - Inventory Status

| Constant | Value | Description |
|----------|-------|-------------|
| `AVAL` | "Available" | Inventory available |
| `COMT` | "Committed" | Inventory committed |
| `INTRAN` | "Intransit" | Inventory in transit |
| `OUTOFSYS` | "OutOfSys" | Out of system |
| `INPALL` | "InPall" | In pallet |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Load Functions | [ds_load.md](../04_DSUB/ds_load.md) | Load operations |
| Inventory Functions | [ds_invt.md](../04_DSUB/ds_invt.md) | Inventory operations |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Load/Inventory Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

