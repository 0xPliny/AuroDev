# Equipment Parameters - global_prm

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

## GP.VEHICLE - Vehicle Control

### VEHICLE.FUNC Enumeration

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
| 13 | `VERY` | Integrity check |
| 14 | `LOOP` | RTNX loop |
| 15 | `STOP` | Stop RTN |
| 16 | `CHRG` | Battery charge |
| 17 | `POSI` | Position request |
| 18 | `CLRA` | Clear all |
| 19 | `CLR1` | Clear fork 1 |
| 20 | `CLR2` | Clear fork 2 |
| 21 | `PWRD` | Power down |
| 22 | `ALOP` | AGV pseudo loop |
| 23 | `ENGR` | Engineering command |
| 24 | `MSPN` | Manual spin |
| 25 | `NONE` | No function |

### VEHICLE.SCHD Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NBSY` | Not busy |
| 1 | `BUSY` | Controller busy |
| 2 | `PEND` | Pending |
| 3 | `QUED` | Queued |
| 4 | `MOV2PK` | Moving to pick |
| 5 | `CHRG` | Charging |
| 6 | `DSTC` | Destination change |

### VEHICLE.COMP Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `COMP` | Complete |
| 1 | `PICKOVER` | Pick time over |
| 2 | `DROPOVER` | Drop time over |
| 3 | `FULLBIN` | Occupied error |
| 4 | `FBDEEP` | Deep position occupied |
| 5 | `EMPTYBIN` | No load after loading |
| 6 | `UNLDERR` | Load on fork after unload |
| 7 | `PRHBCELL` | Prohibited point |
| 8 | `SZHEIGHT` | Height mismatch |
| 9 | `SZWIDTH` | Width mismatch |
| 10 | `LOADLOGC` | Load-logic mismatch |
| 11 | `NOREAD` | Bar code no read |
| 12 | `MISMATCH` | Bar code mismatch |
| 13 | `MOVEQUE` | Forced complete |
| 14 | `MVTOPICK` | Moved to pickup |
| 15 | `PICKUP` | Picked up |
| 16 | `MVTODORP` | Moved to drop |
| 17 | `DROPOFF` | Dropped off |
| 18 | `RECOVERY` | Recovery after error |
| 19 | `ASSIGNED` | Assigned by comms |
| 20 | `MVTOPOINT` | Moved to point |
| 21 | `TRANERROR` | Transfer error |
| 22 | `ENGRREPORT` | Engineering report |

---

## GP.SR - Stacker Control

### SR.FUNC Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `AGV` | Deliver to AGV |
| 1 | `CLER` | Clear all |
| 2 | `CXFR` | Cell-to-cell transfer |
| 3 | `DATC` | Data clear |
| 4 | `DSTC` | Destination to cell |
| 5 | `DSTS` | Destination to station |
| 6 | `HOME` | Home function |
| 7 | `UNHM` | Unhome function |
| 8 | `MOVE` | Move function |
| 9 | `RTRV` | Retrieve function |
| 10 | `RSET` | Reset function |
| 11 | `SHUF` | Aisle shuffle |
| 12 | `STOR` | Store function |
| 13 | `OPER` | Operate function |
| 14 | `STRT` | Start function |
| 15 | `SXSX` | Station transfer |
| 16 | `ERST` | Emergency stop |
| 17 | `MVPK` | Move to pick |
| 18 | `PICK` | Pickup load |
| 19 | `MVDP` | Move to dropoff |
| 20 | `DROP` | Dropoff load |
| 21 | `BUZZ` | Buzzer stop |
| 22 | `VERY` | Integrity check |
| 23 | `FIRE` | Fire command |
| 24 | `CALB` | Calibration |
| 25 | `CLAT` | Clear attribute |
| 26 | `TSAT` | Test attribute |
| 27 | `AUTO` | Auto mode |
| 28 | `SEMI` | Semi-auto mode |
| 29 | `MANL` | Manual mode |
| 30 | `CLRC` | Clear crane only |
| 31 | `ENGR` | Engineering |
| 32 | `TREG` | Time registration |
| 33 | `NONE` | No function |

### SR.SCHD Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NBSY` | Not busy |
| 1 | `BUSY` | Busy |
| 2 | `EROR` | Error |
| 3 | `PEND` | Pending |
| 4 | `QUED` | Queued |

### SR.MODES Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NORM` | Normal |
| 1 | `STOR` | Store only |
| 2 | `RTRV` | Retrieve only |
| 3 | `MOVE` | Move mode |
| 4 | `MANT` | Maintenance |
| 5 | `EXER` | Exercise |

---

## GP.ST - Station Control

### ST.TYP Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NONE` | Undefined |
| 1 | `PDIN`/`STOR` | Stacker input |
| 2 | `PDOUT`/`RTRV` | Stacker output |
| 3 | `PDBOTH`/`BOTH` | Bi-directional |
| 4 | `PDSHUF`/`SHUF` | Aisle shuffle |
| 5 | `BCRINPUT` | Bar code read |
| 6 | `AGVIN` | AGV input |
| 7 | `AGVOUT` | AGV output |
| 8 | `AGVBOTH` | AGV bi-directional |
| 9 | `TOOL` | Tool station |
| 10 | `TRANSFER` | Aisle transfer |
| 11 | `RTNIN` | RTN input |
| 12 | `RTNOUT` | RTN output |
| 13 | `RTNBOTH` | RTN bi-directional |
| 14 | `BLOCK` | Conveyor block |
| 15 | `OTHER` | Other |
| 16 | `GROUP` | Group |
| 17 | `AGVSTD` | Stand-alone AGV |
| 18 | `AGVTOOL` | AGV tool port |
| 19 | `WORK` | Work station |
| 20 | `LIFTER` | Lifter |
| 21-23 | `MANIN`/`OUT`/`BOTH` | Manual stations |
| 24-26 | `RackBoth`/`In`/`Out` | Rack pass-through |
| 27 | `Collector` | Collector |
| 28-29 | `ShipIn`/`Out` | Shipping |
| 30 | `BATTERYCHARGER` | Battery charger |
| 31 | `Profiler` | Profile check |
| 32 | `AGVAPL` | AGV APL |
| 33 | `TCAR` | TCar |
| 34-35 | `AGVDRP_CRNPCK`/`AGVPCK_CRNDRP` | Combined stands |
| 36 | `BUFFER` | Buffer conveyor |

### ST.MODES Enumeration

| Value | Name | String | Description |
|-------|------|--------|-------------|
| 0 | `NORMAL` | "Normal" | Normal operation |
| 1 | `STORE` | "Store" | Store mode |
| 2 | `RETRIEVE` | "Retrieve" | Retrieve mode |
| 3 | `MOVE` | "Move" | Station move |
| 4 | `CELL2CELL` | "Cell2Cell" | Cell shuffle |
| 5 | `FORCE_RETRIEVE` | "Force Retrieve" | Forced retrieve |
| 6 | `FORCE_STORE` | "Force Store" | Forced store |
| 7 | `STOREIN` | "Store IN" | Workstation store in |
| 8 | `STOREOUT` | "Store OUT" | Workstation store out |
| 9 | `STORECARR` | "Store IN Carrier" | Store carrier |
| 10 | `DIRECT` | "Direct" | Direct delivery |
| 11 | `EXERCISE` | "Exercise" | Exercise mode |
| 12 | `SUPPLY` | "Supply" | Supply mode |
| 13 | `RETURN` | "Return" | Return mode |

---

## GP.DEV - Device Types

| Value | Name | Description |
|-------|------|-------------|
| 0 | `PLC` | Programmable Logic Controller |
| 1 | `STACKER` | Stacker Crane |
| 2 | `STATION` | Station/Stand |
| 3 | `COMMUNICATION` | Communication Device |
| 4 | `AGV` | Automated Guided Vehicle |
| 5 | `RTN` | Rail Guided Vehicle (RTNX) |
| 6 | `LABELER` | Labeling Device |
| 7 | `ROBOT` | Robot |
| 8 | `RTNAREASLOT` | RTN Area Slot |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Vehicle Control | [cc_vehicle.md](../02_CCSUB/cc_vehicle.md) | Vehicle operations |
| Stacker Control | [cc_stk.md](../02_CCSUB/cc_stk.md) | Stacker operations |
| VB.NET Equivalent | [Vehicle_Constants.md](../05_ICISDefines/Vehicle_Constants.md) | ICISDefines |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

