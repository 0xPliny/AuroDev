# ICISDefines.vb - VB.NET Constants and Definitions Reference

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\ICISDefines.vb`

**Description:** ICISDefines provides system-wide constants, enumerations, and shared variables for VB.NET dialogs and components in the MHC/MEM system. This file centralizes definitions used across all VB.NET projects, ensuring consistency between the UI layer and C++ background processes.

---

## Table of Contents

1. [System Type Constants](#system-type-constants)
2. [Date/Time Format Constants](#datetime-format-constants)
3. [Global Shared Variables](#global-shared-variables)
4. [Communication Classes](#communication-classes)
5. [Device Status Enumerations](#device-status-enumerations)
6. [Station Types and Modes](#station-types-and-modes)
7. [Move Status Constants](#move-status-constants)
8. [Load and Inventory Constants](#load-and-inventory-constants)
9. [Vehicle Constants](#vehicle-constants)
10. [Stacker Constants](#stacker-constants)
11. [Location Constants](#location-constants)
12. [System Maximums](#system-maximums)
13. [Error Codes](#error-codes)
14. [Cross-Reference with C++](#cross-reference-with-c)

---

## System Type Constants

### `gsSystem_Type`

**Type:** `Public Shared String`

**Description:** Identifies the system type. Set at compile time based on conditional compilation symbols.

**Values:**
| Value | Description |
|-------|-------------|
| `"MCM"` | Material Control Manager |
| `"MEM"` | Murata Equipment Manager |
| `"WCS"` | Warehouse Control System |
| `"MCS"` | Material Control System |
| `"MHC"` | Material Handling Controller |

**Usage:**
```vbnet
If ICISDefines.gsSystem_Type = "MEM" Then
    ' MEM-specific logic
End If
```

**C++ Equivalent:** None (compile-time definition)

---

## Date/Time Format Constants

### Date Format Strings

| Constant | Value | Description | Example Output |
|----------|-------|-------------|----------------|
| `DateTime_SQL` | `"yyyy-MM-dd HH:mm:ss"` | SQL Server datetime format | `2024-01-15 14:30:45` |
| `DateTime_Display` | `"MM/dd/yyyy HH:mm:ss"` | US display format | `01/15/2024 14:30:45` |
| `Date_SQL` | `"yyyy-MM-dd"` | SQL date only | `2024-01-15` |
| `Date_Display` | `"MM/dd/yyyy"` | US date display | `01/15/2024` |
| `Time_Display` | `"HH:mm:ss"` | 24-hour time | `14:30:45` |
| `Time_Short` | `"HH:mm"` | Short time | `14:30` |

**Usage:**
```vbnet
Dim formattedDate As String = DateTime.Now.ToString(ICISDefines.DateTime_SQL)
```

---

## Global Shared Variables

### Session Variables

| Variable | Type | Description |
|----------|------|-------------|
| `gsUserName` | `String` | Current logged-in user name |
| `gsUserID` | `Integer` | Current user's database ID |
| `gsAreaCode` | `String` | Current working area (A, B, C) |
| `gsProjectPath` | `String` | Root path to project files |
| `gsComputerName` | `String` | Current workstation name |

### Connection Variables

| Variable | Type | Description |
|----------|------|-------------|
| `gsConnectionString` | `String` | Database connection string |
| `gsServer` | `String` | Database server name |
| `gsDatabase` | `String` | Database name |

---

## Communication Classes

### COM Class

**Description:** Communication status enumeration for device connections.

```vbnet
Public Class COM
    Public Const OFFL As Integer = 0      ' Offline
    Public Const ONLI As Integer = 1      ' Online
    Public Const RUND As Integer = 2      ' Rundown (going offline)
    Public Const INIT As Integer = 3      ' Initializing (going online)
    Public Const STRT As Integer = 4      ' Start
    Public Const CLER As Integer = 5      ' Clear
    Public Const HOME As Integer = 6      ' Home
    Public Const RSET As Integer = 7      ' Reset
    Public Const ESTP As Integer = 8      ' Emergency Stop
    Public Const DISD As Integer = 9      ' Disabled
    Public Const WAIT As Integer = 10     ' Waiting
    Public Const WAIT_LISTEN As Integer = 11  ' Wait Listen
    Public Const WAIT_OUTPUT As Integer = 12  ' Wait Output
    Public Const ATCH As Integer = 13     ' Attaching
    Public Const TERR As Integer = 14     ' Attach Error
End Class
```

**C++ Equivalent:** `GP.COM` in `global_prm.h`

**Usage:**
```vbnet
If device.CommStatus = ICISDefines.COM.ONLI Then
    ' Device is online
End If
```

---

## Device Status Enumerations

### DEV Class - Device Types

```vbnet
Public Class DEV
    Public Enum DeviceType
        PLC = 0           ' Programmable Logic Controller
        STACKER = 1       ' Stacker Crane
        STATION = 2       ' Station/Stand
        COMMUNICATION = 3 ' Communication Device
        AGV = 4           ' Automated Guided Vehicle
        RTN = 5           ' Rail Guided Vehicle (RTNX)
        LABELER = 6       ' Labeling Device
        ROBOT = 7         ' Robot
        RTNAREASLOT = 8   ' RTN Area Slot
    End Enum
End Class
```

**C++ Equivalent:** `GP.DEV.type_of_device` in `global_prm.h`

### ESTAT Class - Error Status

```vbnet
Public Class ESTAT
    Public Shared ReadOnly iERROR As String = "ERROR"
    Public Shared ReadOnly OFFLINE As String = "OFFLINE"
    Public Shared ReadOnly ONLINE As String = "ONLINE"
End Class
```

---

## Station Types and Modes

### Station Types (ST.TYP)

| Value | Constant | Description |
|-------|----------|-------------|
| 0 | `NONE` | Undefined station type |
| 1 | `PDIN` / `STOR` | Stacker Input (Store) |
| 2 | `PDOUT` / `RTRV` | Stacker Output (Retrieve) |
| 3 | `PDBOTH` / `BOTH` | Bi-Directional Stacker |
| 4 | `PDSHUF` / `SHUF` | Aisle Shuffle |
| 5 | `BCRINPUT` | Bar Code Read Station |
| 6 | `AGVIN` | AGV Input Station |
| 7 | `AGVOUT` | AGV Output Station |
| 8 | `AGVBOTH` | Bi-Directional AGV Station |
| 9 | `TOOL` | Tool Station |
| 10 | `TRANSFER` | Aisle Transfer Station |
| 11 | `RTNIN` | RTN Input Station |
| 12 | `RTNOUT` | RTN Output Station |
| 13 | `RTNBOTH` | Bi-Directional RTN Station |
| 14 | `BLOCK` | Conveyor Block |
| 15 | `OTHER` | Other |
| 16 | `GROUP` | Group Station |
| 17 | `AGVSTD` | Stand-Alone AGV Stand |
| 18 | `AGVTOOL` | AGV Tool Port |
| 19 | `WORK` | Work Station |
| 20 | `LIFTER` | Lifter |
| 21 | `MANIN` | Manual Input |
| 22 | `MANOUT` | Manual Output |
| 23 | `MANBOTH` | Manual Both |
| 24 | `RackBoth` | Rack Pass-Through Both |
| 25 | `RackIn` | Rack Pass-Through In |
| 26 | `RackOut` | Rack Pass-Through Out |
| 27 | `Collector` | Collector Stand |
| 28 | `ShipIn` | Shipping Input |
| 29 | `ShipOut` | Shipping Output |
| 30 | `BATTERYCHARGER` | AGV Battery Charger |
| 31 | `Profiler` | Profile Check Stand |
| 32 | `AGVAPL` | AGV APL Stand |
| 33 | `TCAR` | TCar |
| 34 | `AGVDRP_CRNPCK` | AGV Drop / Crane Pickup |
| 35 | `AGVPCK_CRNDRP` | AGV Pickup / Crane Drop |
| 36 | `BUFFER` | Buffer Conveyor |

**C++ Equivalent:** `GP.ST.TYP` in `global_prm.h`

### Station Modes (ST.MODES)

| Value | Constant | String | Description |
|-------|----------|--------|-------------|
| 0 | `NORMAL` | "Normal" | Normal operation |
| 1 | `STORE` | "Store" | Store mode |
| 2 | `RETRIEVE` | "Retrieve" | Retrieve mode |
| 3 | `MOVE` | "Move" | Station-to-Station move |
| 4 | `CELL2CELL` | "Cell2Cell" | Cell-to-cell shuffle |
| 5 | `FORCE_RETRIEVE` | "Force Retrieve" | Forced retrieve (no checks) |
| 6 | `FORCE_STORE` | "Force Store" | Forced store (no checks) |
| 7 | `STOREIN` | "Store IN" | Workstation store in |
| 8 | `STOREOUT` | "Store OUT" | Workstation store out |
| 9 | `STORECARR` | "Store IN Carrier" | Store empty carrier |
| 10 | `DIRECT` | "Direct" | Direct delivery |
| 11 | `EXERCISE` | "Exercise" | Exercise mode |
| 12 | `SUPPLY` | "Supply" | Supply mode |
| 13 | `RETURN` | "Return" | Return mode |

**C++ Equivalent:** `GP.ST.MODES` in `global_prm.h`

---

## Move Status Constants

### MOVS.STATUS Class

| Constant | Value | Description |
|----------|-------|-------------|
| `ACTV` | "Active" | Move is currently in progress |
| `ASSIGN` | "Assign" | New store waiting on host assignment |
| `ASSIGNWAIT` | "AssignWait" | Wait for shuffle then find location |
| `CANCEL` | "Cancel" | Move transaction cancelled |
| `CMPLT` | "Complete" | Move completed |
| `COLLECTING` | "Collecting" | Collecting at stacker input |
| `CMPREM` | "CompRemove" | Complete followed by remove |
| `HOLD` | "Hold" | Hold until AGV delivers |
| `FULL` | "Full" | Rack is full, find shuffle |
| `GROUPED` | "Grouped" | Grouped with other moves |
| `LOST` | "Lost" | Load is lost |
| `PEND` | "Pending" | Ready to process |
| `QUED` | "Queued" | Waiting for conditions |
| `REJECT` | "Reject" | Rejected load |
| `RMVD` | "Removed" | Load removed from station |
| `REQUEST` | "Request" | Pallet request |
| `SCHED` | "Scheduled" | Output P&D waiting |
| `WAIT` | "Waiting" | Waiting for operator action |
| `WAITMODE` | "WaitMode" | Waiting for station mode change |
| `WAITALL` | "WaitAlloc" | Waiting for allocator |
| `TIMER` | "Timer" | Waiting for timer |
| `PROFILE` | "Profile" | Profile error, waiting response |

**C++ Equivalent:** `GP.MOVS.STATUS` in `global_prm.h`

### MOVS.ACTION Class

| Constant | Value | Description |
|----------|-------|-------------|
| `AGV_MOVE` | "AGV-Move" | AGV move operation |
| `AGV_PICKUP` | "AGV-PickUp" | AGV pickup operation |
| `AGV_DROPOFF` | "AGV-DropOff" | AGV dropoff operation |
| `BCR` | "BCR" | Bar code read operation |
| `CNV_MOVE` | "CONV-Move" | Conveyor move |
| `RTN_MOVE` | "RTN-Move" | RTN move operation |
| `SR_RET` | "SR-Retrieve" | Stacker retrieve |
| `SR_STO` | "SR-Store" | Stacker store |
| `SR_TRF_CELL` | "SR-Trf-Cell" | Stacker cell transfer |
| `SR_TRF_STND` | "SR-Trf-Stand" | Stacker stand transfer |
| `SR_AUDIT` | "SR-Audit" | Stacker audit |

**C++ Equivalent:** `GP.MOVS.ACTION` in `global_prm.h`

### MOVS.TYPE Class

| Constant | Value | Description |
|----------|-------|-------------|
| `CANCEL` | "Cancel" | Cancel move |
| `STORE` | "Store" | Store operation |
| `RETRIEVE` | "Retrieve" | Retrieve operation |
| `AGVMOV` | "AGV-Move" | AGV move |
| `RETRIEVE_4_STORE` | "Retrieve_4_Store" | Retrieve for re-store |
| `SHUFFLE` | "Shuffle" | Shuffle operation |
| `TRANSFER` | "Transfer" | Transfer operation |

**C++ Equivalent:** `GP.MOVS.TYPE` in `global_prm.h`

---

## Load and Inventory Constants

### LOAD.STATUS Class

| Constant | Value | Description |
|----------|-------|-------------|
| `AVAL` | "Available" | Load is available |
| `COMT` | "Committed" | Load is committed |
| `INTRAN` | "Intransit" | Load is in transit |
| `OUTOFSYS` | "OutOfSys" | Load is out of system |
| `INPALL` | "InPall" | Load is in pallet |

**C++ Equivalent:** `GP.LOAD.STATUS` in `global_prm.h`

### LOAD.LOCATION Class

| Constant | Value | Description |
|----------|-------|-------------|
| `INTRAN` | "IN-TR-AN" | In-transit location |
| `OUTSYS` | "OU-TS-YS" | Out-of-system location |

**C++ Equivalent:** `GP_INTRAN_STR`, `GP_OUTSYS_STR` in `global_prm.h`

### INVT.STATUS Class

| Constant | Value | Description |
|----------|-------|-------------|
| `AVAL` | "Available" | Inventory available |
| `COMT` | "Committed" | Inventory committed |
| `INTRAN` | "Intransit" | Inventory in transit |
| `OUTOFSYS` | "OutOfSys" | Out of system |
| `INPALL` | "InPall" | In pallet |

---

## Vehicle Constants

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

### VEHICLE.SCHD Enumeration

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

### VEHICLE.COMP Enumeration (Completion Types)

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

## Stacker Constants

### SR.FUNC Enumeration

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

### SR.SCHD Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NBSY` | Not busy |
| 1 | `BUSY` | Busy |
| 2 | `EROR` | Error |
| 3 | `PEND` | Pending |
| 4 | `QUED` | Queued |

**C++ Equivalent:** `GP.SR.SCHD` in `global_prm.h`

### SR.MODES Enumeration

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

## Location Constants

### LOCN.STATUS Class

| Constant | Value | Description |
|----------|-------|-------------|
| `FULL` | "Full" | Location is occupied |
| `EMPTY` | "Empty" | Location is empty |

### LOCN.PENDING_STATUS Class

| Constant | Value | Description |
|----------|-------|-------------|
| `NONE` | "None" | No pending operation |
| `PENDING` | "Pending" | Operation pending |

### LOCN.LOCKED_FLAG Class

| Constant | Value | Description |
|----------|-------|-------------|
| `INVALID` | "Invalid" | Invalid location |
| `LOCKED` | "Locked" | Location is locked |
| `UNLOCKED` | "Unlocked" | Location is unlocked |

**C++ Equivalent:** `GP.LOCN` in `global_prm.h`

---

## System Maximums

### MAX Class

| Constant | Value | Description |
|----------|-------|-------------|
| `AREAS` | 1 | Maximum working areas |
| `AISLE` | 22 | Maximum aisles |
| `AISLE_ERRS` | 10 | Error history per aisle |
| `SR` | 22 | Maximum stackers per area |
| `Forks` | 5 | Max forks per device |
| `BAY` | 22 | Maximum bays |
| `SIDE` | 2 | Maximum sides |
| `ROW` | 2 | Maximum rows |
| `TIER` | 19 | Maximum tiers |
| `SYS_ROW` | 44 | MAX_AISLE × MAX_ROW |
| `AGVS` | 0 | Maximum AGVs |
| `BCRS` | 0 | Maximum bar code readers |
| `ZONES` | 1 | Maximum zones |
| `PLC` | 13 | Maximum PLCs |
| `WATCHED` | 40 | Max watched processes |
| `prgnam_SIZ` | 20 | Max process name size |
| `FNAME_SIZ` | 200 | Max filename size |
| `G_TEXT_SIZ` | 512 | Max text buffer size |
| `TXT_SIZ` | 1024 | Max cs_getxt buffer |
| `PNT_POINTS` | 2048 | Max path points |
| `PNT_ZONES` | 1024 | Max path zones |
| `PNT_LOOPS` | 1024 | Max path loops |
| `PNT_BRIDGES` | 1024 | Max path bridges |
| `PNT_GROUPS` | 4 | Max path groups |
| `RtNXForks` | 2 | Max RTNX forks |

**C++ Equivalent:** `GP.MAX` in `global_prm.h`

---

## Error Codes

### Station Error Codes (ST.std_err)

| Code | Name | Description |
|------|------|-------------|
| 105 | `ERR_TIME_OUT_BCR` | Bar code no-read |
| 106 | `ERR_TIME_OUT_BCR_MAX` | Max bar code no-reads |
| 107 | `ERR_UNKNOWN_LOAD` | Unknown load at station |
| 108 | `ERR_UNKNOWN_PART` | Unknown part number |
| 109 | `ERR_NO_LOCATION` | No storage locations found |
| 110 | `ERR_DUP_ARRIVE` | Duplicate arrival |
| 111 | `ERR_MISMATCH` | Scanner/PLC mismatch |
| 112 | `ERR_SIZING` | Profile check failed |
| 113 | `ERR_STATE` | Inconsistent status state |
| 114 | `ERR_WRONG_DEST` | Arrived at wrong place |
| 115 | `ERR_STORE_PENDING` | Store pending found |
| 116 | `ERR_BCR_VERIFY` | BCR verification |
| 117 | `ERR_COMM_ERROR` | Communications offline |
| 118 | `ERR_FORK_ERROR` | Fork truck input error |
| 119 | `ERR_OVERRIDE` | Override arrival checks |
| 120 | `ERR_WAIT_OPER` | Waiting for operator |
| 121 | `ERR_ROUTE_BUSY` | Route not available |
| 122 | `ERR_TO_MANY_STORES` | Too many store attempts |
| 123 | `ERR_OPER_ACCEPT` | Operator accepted |
| 124 | `ERR_DUP_HANDLE` | Duplicate carton handles |
| 125 | `ERR_NOT_EMPTY` | Load not empty |
| 126 | `ERR_BCR_NOREAD` | BCR could not read label |
| 127 | `ERR_BARCODE_MISMATCH` | Barcode data mismatch |
| 128 | `ERR_NO_STACK` | Stack not allowed |
| 129 | `ERR_RTNX_PICKUP` | RTNX pickup error |
| 130 | `ERR_RTNX_DROPOFF` | RTNX dropoff error |
| 131 | `ERR_DISP_EMPTY` | Pallet dispenser empty |
| 132 | `ERR_ONE_OFF` | Incorrect move count |
| 133 | `ERR_RET_COM_FAILED` | Retrieve complete rejected |
| 134 | `ERR_LOAD_PROFILE` | Load profile error |
| 200 | `FTP_ARRIVE` | FTP data arrival |

**C++ Equivalent:** `GP.ST.std_err` in `global_prm.h`

---

## Cross-Reference with C++

### Ensuring Consistency

Both `ICISDefines.vb` and `global_prm.h` must be kept synchronized. When adding or modifying constants:

1. **Update Both Files:** Changes to enumerations must be made in both VB.NET and C++ files
2. **Same Values:** Numeric values must match exactly between languages
3. **Same Order:** Enumeration order must be preserved for index lookups
4. **Comment Annotations:** Both files contain comments indicating counterparts:
   - VB.NET: `' Must Match global_prm.h`
   - C++: `// Must Match ICISDefines.vb`

### Key Cross-Reference Points

| VB.NET Class | C++ Equivalent |
|--------------|----------------|
| `ICISDefines.COM` | `GP.COM` |
| `ICISDefines.DEV` | `GP.DEV` |
| `ICISDefines.ST.TYP` | `GP.ST.TYP` |
| `ICISDefines.ST.MODES` | `GP.ST.MODES` |
| `ICISDefines.MOVS` | `GP.MOVS` |
| `ICISDefines.LOAD` | `GP.LOAD` |
| `ICISDefines.VEHICLE` | `GP.VEHICLE` |
| `ICISDefines.SR` | `GP.SR` |
| `ICISDefines.MAX` | `GP.MAX` |

---

## Usage Examples

### Checking Device Status

```vbnet
' Check if a stacker is online and not busy
If stacker.CommStatus = ICISDefines.COM.ONLI AndAlso 
   stacker.Schedule = ICISDefines.SR.SCHD.NBSY Then
    ' Stacker is ready for work
End If
```

### Setting Move Status

```vbnet
' Update move status to pending
move.Status = ICISDefines.MOVS.STATUS.PEND
move.Update()
```

### Station Mode Check

```vbnet
' Check if station is in store mode
If station.Mode = ICISDefines.ST.MODES.STORE Then
    ' Process store operations
End If
```

### Format Date for Database

```vbnet
' Format current time for SQL insert
Dim sqlDate As String = DateTime.Now.ToString(ICISDefines.DateTime_SQL)
```

---

## Related Files

- **C++ Equivalent:** `global_prm.h` - Contains matching C++ definitions
- **VB Interop:** `MHC_Interop` - Marshaling between VB.NET and C++
- **Database Tables:** Generated OSUB classes use these constants
- **UI Controls:** All MHC dialogs reference these constants

---

*Last Updated: 12/23/2024*
*Source: ICISDefines.vb (Lines 1-3700)*
