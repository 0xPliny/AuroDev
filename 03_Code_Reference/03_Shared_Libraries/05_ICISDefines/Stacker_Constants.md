# Stacker Constants - ICISDefines

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

## SR.FUNC Enumeration - Stacker Functions

| Value | Name | Description |
|-------|------|-------------|
| 0 | `AGV` | Deliver to AGV |
| 1 | `CLER` | Clear error |
| 2 | `CXFR` | Cell-to-cell transfer |
| 3 | `DATC` | Data clear |
| 4 | `DSTC` | Destination change to cell |
| 5 | `DSTS` | Destination change to station |
| 6 | `HOME` | Home function |
| 7 | `UNHM` | Unhome function |
| 8 | `MOVE` | Move function |
| 9 | `RTRV` | Retrieve function |
| 10 | `RSET` | Reset function |
| 11 | `SHUF` | Aisle-to-aisle shuffle |
| 12 | `STOR` | Store function |
| 13 | `OPER` | Operate function |
| 14 | `STRT` | Start function |
| 15 | `SXSX` | Station-to-station transfer |
| 16 | `ERST` | Emergency stop |
| 17 | `MVPK` | Move to pick |
| 18 | `PICK` | Pickup load |
| 19 | `MVDP` | Move to dropoff |
| 20 | `DROP` | Dropoff load |
| 21 | `BUZZ` | Buzzer stop |
| 22 | `VERY` | Integrity check request |
| 23 | `FIRE` | Fire command (0042) |
| 24 | `CALB` | Calibration command |
| 25 | `CLAT` | Clear attribute |
| 26 | `TSAT` | Test attribute |
| 27 | `AUTO` | Auto mode |
| 28 | `SEMI` | Semi-auto mode |
| 29 | `MANL` | Manual mode |
| 30 | `CLRC` | Clear crane only (not forks) |
| 31 | `ENGR` | Engineering command |
| 32 | `TREG` | Time registration |
| 33 | `NONE` | No function |

**C++ Equivalent:** `GP.SR.FUNC` in `global_prm.h`

---

## SR.SCHD Enumeration - Stacker Schedule

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NBSY` | Not busy |
| 1 | `BUSY` | Busy |
| 2 | `EROR` | Error |
| 3 | `PEND` | Pending |
| 4 | `QUED` | Queued |

**C++ Equivalent:** `GP.SR.SCHD` in `global_prm.h`

---

## SR.MODES Enumeration - Stacker Modes

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NORM` | Normal mode |
| 1 | `STOR` | Store only |
| 2 | `RTRV` | Retrieve only |
| 3 | `MOVE` | Move mode |
| 4 | `MANT` | Maintenance |
| 5 | `EXER` | Exercise |

**C++ Equivalent:** `GP.SR.MODES` in `global_prm.h`

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Stacker Control | [cc_stk.md](../02_CCSUB/cc_stk.md) | Stacker operations |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Stacker Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

