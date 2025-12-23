# Vehicle Constants - ICISDefines

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

## VEHICLE.FUNC Enumeration - Vehicle Functions

| Value | Name | Description |
|-------|------|-------------|
| 0 | `CLER` | Clear error |
| 1 | `DSTS` | Destination change to station |
| 2 | `MOVE` | Move function |
| 3 | `RSET` | Reset function |
| 4 | `OPER` | Operate function |
| 5 | `STRT` | Start function |
| 6 | `SXSX` | Station-to-station transfer |
| 7 | `ERST` | Emergency stop |
| 8 | `MVPK` | Move to pick |
| 9 | `PICK` | Pickup load |
| 10 | `MVDP` | Move to dropoff |
| 11 | `DROP` | Dropoff load |
| 12 | `BUZZ` | Buzzer stop |
| 13 | `VERY` | Integrity check request |
| 14 | `LOOP` | RTNX loop command |
| 15 | `STOP` | Stop RTN |
| 16 | `CHRG` | Battery charge command |
| 17 | `POSI` | Position data request |
| 18 | `CLRA` | Clear vehicle and all fork data |
| 19 | `CLR1` | Clear fork 1 data only |
| 20 | `CLR2` | Clear fork 2 data only |
| 21 | `PWRD` | Power down vehicle |
| 22 | `ALOP` | AGV pseudo loop |
| 23 | `ENGR` | Engineering command |
| 24 | `MSPN` | Manual spin |
| 25 | `NONE` | No function |

**C++ Equivalent:** `GP.VEHICLE.FUNC` in `global_prm.h`

---

## VEHICLE.SCHD Enumeration - Vehicle Schedule

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NBSY` | Not busy |
| 1 | `BUSY` | Busy |
| 2 | `PEND` | Pending |
| 3 | `QUED` | Queued |
| 4 | `MOV2PK` | Moving to pick |
| 5 | `CHRG` | Charging |
| 6 | `DSTC` | Destination change |

**C++ Equivalent:** `GP.VEHICLE.SCHD` in `global_prm.h`

---

## VEHICLE.COMP Enumeration - Vehicle Completion Types

| Value | Name | Description |
|-------|------|-------------|
| 0 | `COMP` | Complete |
| 1 | `PICKOVER` | Pick time over error |
| 2 | `DROPOVER` | Drop time over error |
| 3 | `FULLBIN` | Occupied error |
| 4 | `FBDEEP` | Deep position occupied error |
| 5 | `EMPTYBIN` | No load after loading cycle |
| 6 | `UNLDERR` | Load on fork after unloading |
| 7 | `PRHBCELL` | Prohibited point error |
| 8 | `SZHEIGHT` | Load height mismatch |
| 9 | `SZWIDTH` | Load width mismatch |
| 10 | `LOADLOGC` | Load-logic mismatch |
| 11 | `NOREAD` | Bar code no read |
| 12 | `MISMATCH` | Bar code mismatch |
| 13 | `MOVEQUE` | Forced complete from queue |
| 14 | `MVTOPICK` | Moved to pickup |
| 15 | `PICKUP` | Picked up |
| 16 | `MVTODORP` | Moved to drop |
| 17 | `DROPOFF` | Dropped off |
| 18 | `RECOVERY` | Recovery after error |
| 19 | `ASSIGNED` | RTNX assigned by communications |
| 20 | `MVTOPOINT` | Moved to point |
| 21 | `TRANERROR` | RTNX transfer error |
| 22 | `ENGRREPORT` | Engineering report |

**C++ Equivalent:** `GP.VEHICLE.COMP` in `global_prm.h`

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Vehicle Control | [cc_vehicle.md](../02_CCSUB/cc_vehicle.md) | Vehicle operations |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Vehicle Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

