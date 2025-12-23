# global_prm.h - Global Parameters and Definitions Reference

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\incl\global_prm.h`

**Description:** `global_prm.h` is the central header file containing system-wide constants, type definitions, macros, and project-specific configuration values for the MHC/MEM C++ codebase. This file is included in virtually all C++ source files.

---

## Table of Contents

1. [Type Definitions](#type-definitions)
2. [Return Codes and Boolean Constants](#return-codes-and-boolean-constants)
3. [Log Level Constants](#log-level-constants)
4. [Project-Specific Maximums](#project-specific-maximums)
5. [Macro Definitions](#macro-definitions)
6. [Control Character Definitions](#control-character-definitions)
7. [PLC Definitions](#plc-definitions)
8. [File Path Definitions](#file-path-definitions)
9. [Program Name Definitions](#program-name-definitions)
10. [Global Variables](#global-variables)
11. [GP Class Reference](#gp-class-reference)
12. [Cross-Reference with VB.NET](#cross-reference-with-vbnet)

---

## Type Definitions

### Fundamental Types

| Type | Base Type | Description | Size |
|------|-----------|-------------|------|
| `GP_BOOL` | `char` | Boolean variable type | 1 byte |
| `GP_UCHAR` | `unsigned char` | Unsigned char | 1 byte |
| `GP_UINT` | `unsigned int` | Unsigned integer | 4 bytes |
| `GP_USHORT` | `unsigned short` | Unsigned short | 2 bytes |
| `GP_ULONG` | `unsigned long` | Unsigned long | 4 bytes |
| `GP_8BITS` | `unsigned char` | 8-bit variable | 1 byte |
| `GP_16BITS` | `unsigned int` | 16-bit variable | 2 bytes |
| `GP_32BITS` | `unsigned long` | 32-bit variable | 4 bytes |
| `GP_TRKBTS` | `unsigned short` | Tracking bits | 2 bytes |

**Usage:**
```cpp
GP_BOOL isEnabled = TRUE;
GP_UINT count = 0;
GP_8BITS statusFlags = 0x00;
```

### Null Definitions

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_NULLP` | `(void *) 0` | NULL pointer |
| `GP_NULLC` | `'\0'` | NULL character |
| `GP_NULL` | `0` | NULL value |

---

## Return Codes and Boolean Constants

### Boolean Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `TRUE` | `1` | True value |
| `FALSE` | `0` | False value |
| `GP_YES` | `1` | System value for YES |
| `GP_NO` | `0` | System value for NO |
| `GP_YES_CHR` | `'Y'` | Character value for YES |
| `GP_NO_CHR` | `'N'` | Character value for NO |
| `GP_YES_STR` | `"Yes"` | String value for YES |
| `GP_NO_STR` | `"No"` | String value for NO |
| `GP_ASK` | `2` | Ask value |

### Return Codes (GP Class)

| Constant | Value | Description | Usage |
|----------|-------|-------------|-------|
| `GP.GOOD` | `0` | Success | Normal function completion |
| `GP.BAD` | `-1` | Failure | Error occurred |
| `GP.DBHELD` | `-1` | Database held | Deadlock detection |
| `GP.UGLY` | `1` | Questionable | Partial success |
| `GP.BETTER` | `2` | Good + more needed | Success with follow-up |
| `GP.EMPTY` | `2` | Empty result | No data found |
| `GP.FULL` | `3` | Full result | Buffer/storage full |
| `GP.FAILED` | `-2` | Specific test failure | Test did not pass |
| `GP.TIME_OUT` | `-3` | Operation timed out | Timeout occurred |
| `GP.Lets_TRY_AGAIN` | `-4` | Try again later | Temporary failure |
| `GP.RACK_FULL` | `-5` | No room in rack | Storage full |
| `GP.RACK_REALLY_FULL` | `-6` | No room at all | Including reserve |

**Usage:**
```cpp
int result = performOperation();
if (result == GP.GOOD) {
    // Success
} else if (result == GP.BAD) {
    // Handle error
}
```

### Comparison Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MATCH` | `0` | strcmp match |
| `GP_LESS` | `-1` | Less than |
| `GP_GREATER` | `1` | Greater than |

### On/Off Enumeration

```cpp
enum {
    GP_OFF,    // 0 = Discrete status OFF
    GP_ON      // 1 = Discrete status ON
};
```

---

## Log Level Constants

### Log Levels (0-10)

| Constant | Value | Description | Usage |
|----------|-------|-------------|-------|
| `GP_LOGLVL_0` | `0` | Minimum logging | Errors only |
| `GP_LOGLVL_1` | `1` | Level 1 | Critical messages |
| `GP_LOGLVL_2` | `2` | Level 2 | Important messages |
| `GP_LOGLVL_3` | `3` | Level 3 | Standard messages |
| `GP_LOGLVL_4` | `4` | Level 4 | Informational |
| `GP_LOGLVL_5` | `5` | Level 5 | Detailed info |
| `GP_LOGLVL_6` | `6` | Level 6 | Verbose |
| `GP_LOGLVL_7` | `7` | Level 7 | Debug |
| `GP_LOGLVL_8` | `8` | Level 8 | Trace |
| `GP_LOGLVL_9` | `9` | Level 9 | Detail trace |
| `GP_LOGLVL_10` | `10` | Maximum logging | All messages |

### Special Log Levels

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_TEST_LOG_LVL_MAIN` | `GP_LOGLVL_6` | Test module start/end |
| `GP_TEST_LOG_LVL_DETAIL` | `GP_LOGLVL_9` | Test details |
| `GP_LOW_LEVEL_TRACE` | `GP_LOGLVL_10` | Low-level routine traces |

**Usage:**
```cpp
cs_log_printf(GP_LOGLVL_3, "Processing move %d", moveId);
```

---

## Project-Specific Maximums

### Storage Configuration

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_AREAS` | `1` | Maximum working areas |
| `GP_MAX_AISLE` | `22` | Maximum aisles |
| `GP_MAX_SIDE` | `1` | Maximum sides |
| `GP_MAX_ROW` | `2` | Maximum rows per aisle |
| `GP_MAX_SYS_ROW` | `44` | MAX_AISLE × MAX_ROW |
| `GP_MAX_BAY` | `22` | Maximum bays |
| `GP_MAX_TIER` | `19` | Maximum tiers |
| `GP_MAX_SR` | `22` | Maximum stackers per area |

### Equipment Counts

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_AGVS` | `0` | Maximum AGVs |
| `GP_MAX_BCRS` | `0` | Maximum bar code readers |
| `GP_MAX_ZONE` | `1` | Maximum zones |
| `GP_MAX_PD` | `1` | Max P&Ds per stacker |
| `GP_MAX_PLC` | `13` | Maximum PLCs |

### Buffer Sizes

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_prgnam_SIZ` | `20` | Max process name size |
| `GP_MAX_FNAME_SIZ` | `200` | Maximum filename size |
| `GP_MAX_G_TEXT_SIZ` | `512` | Max text buffer size |
| `GP_MAX_TXT_SIZ` | `1024` | Max cs_getxt buffer |
| `GP_CONNECTIONS` | `100` | Max client connections |
| `GP_MESSAGES` | `250` | Max stored messages |

### Load Identifiers

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_LOADID` | `8` | Max loadid size |
| `GP_MAX_BITS_FOR_POINT` | `5` | Max bits per point |
| `GP_MAX_LOCN_SIZE` | `1` | Max location sizes |

---

## Macro Definitions

### Trace Macros

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

### Error Logging Macro

```cpp
#define cs_erlog(text, msgnum) \
    gg_file = __FILE__; \
    gg_line = __LINE__; \
    cs_erlog2(text, msgnum);
```

---

## Control Character Definitions

### ASCII Control Characters

| Constant | Value | Description | ASCII Name |
|----------|-------|-------------|------------|
| `GP_CC_NULL` | `0x00` | NULL | NUL |
| `GP_CC_SOH` | `0x01` | Start of Header | SOH |
| `GP_CC_STX` | `0x02` | Start of Text | STX |
| `GP_CC_ETX` | `0x03` | End of Text | ETX |
| `GP_CC_EOT` | `0x04` | End of Transmission | EOT |
| `GP_CC_ENQ` | `0x05` | Enquiry | ENQ |
| `GP_CC_ACK` | `0x06` | Acknowledge | ACK |
| `GP_CC_BEL` | `0x07` | Bell | BEL |
| `GP_CC_BS` | `0x08` | Backspace | BS |
| `GP_CC_TAB` | `0x09` | Tab | HT |
| `GP_CC_LF` | `0x0A` | Line Feed | LF |
| `GP_CC_FF` | `0x0C` | Form Feed/Clear | FF |
| `GP_CC_CR` | `0x0D` | Carriage Return | CR |
| `GP_CC_SO` | `0x0E` | Shift Out | SO |
| `GP_CC_DLE` | `0x10` | Data Link Escape | DLE |
| `GP_CC_DC1` | `0x11` | Device Control 1 (XON) | DC1 |
| `GP_CC_DC2` | `0x12` | Device Control 2 | DC2 |
| `GP_CC_DC3` | `0x13` | Device Control 3 (XOFF) | DC3 |
| `GP_CC_NAK` | `0x15` | Negative Acknowledge | NAK |
| `GP_CC_DLO` | `0x16` | Data Link Option | SYN |
| `GP_CC_ETB` | `0x17` | End Transmission Block | ETB |
| `GP_CC_CAN` | `0x18` | Cancel | CAN |
| `GP_CC_EM` | `0x19` | End of Medium | EM |
| `GP_CC_SUB` | `0x1A` | Substitute | SUB |
| `GP_CC_ESC` | `0x1B` | Escape | ESC |
| `GP_CC_FS` | `0x1C` | File Separator | FS |
| `GP_CC_GS` | `0x1D` | Group Separator | GS |
| `GP_CC_US` | `0x1F` | Unit Separator | US |
| `GP_CC_SOT` | `0x21` | Start of Transmission | ! |

**Usage:**
```cpp
// Check for end of message
if (buffer[pos] == GP_CC_ETX) {
    // Process complete message
}
```

---

## PLC Definitions

### PLC Configuration

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MAX_PLC` | `13` | Max PLCs configured |
| `GP_MAX_DSC` | `13680` | Max discretes in system |
| `GP_MAX_PLC_REGB` | `16` | Max bits in PLC registers |

### Discrete Ranges

| Type | Begin | Last | End | Description |
|------|-------|------|-----|-------------|
| **Input** | `GP_BEG_INPT` (1000) | `GP_LST_INPT` (14679) | `GP_END_INPT` (14680) | Input discretes |
| **Output** | `GP_BEG_OTPT` (15000) | `GP_LST_OTPT` (28679) | `GP_END_OTPT` (28680) | Output discretes |
| **Internal** | `GP_BEG_INTN` (29000) | `GP_LST_INTN` (42679) | `GP_END_INTN` (42680) | Internal discretes |

### PLC Type Flags

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_UseMXControls` | `1` | Use Mitsubishi MX Controls (Production) |
| `GP_UseMXControls_DEV` | `0` | Don't use MX Controls (Development) |

---

## File Path Definitions

### Configuration Files

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

---

## Program Name Definitions

### Background Process Names

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

---

## Global Variables

### External Variables

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

## GP Class Reference

The `global_parms` class (instantiated as `GP`) provides an object-oriented interface to system constants. Access via static class instance `GP`.

### Basic Constants (GP Class)

```cpp
static const long NO = 0;
static const long YES = 1;
static const long ASK = 2;
static const long OFF = 0;
static const long ON = 1;
static const long PEND = 2;
static const long MATCH = 0;
static const long LESS = -1;
static const long GREATER = 1;
static const long GOOD = 0;
static const long BAD = -1;
static const long DBHELD = -1;
static const long UGLY = 1;
static const long BETTER = 2;
static const long EMPTY = 2;
static const long FULL = 3;
static const long FAILED = -2;
static const long TIME_OUT = -3;
static const long Lets_TRY_AGAIN = -4;
static const long RACK_FULL = -5;
static const long RACK_REALLY_FULL = -6;
```

### String Methods (GP Class)

| Method | Returns | Description |
|--------|---------|-------------|
| `GP.YES_STR()` | `"Yes"` | Yes string |
| `GP.NO_STR()` | `"No"` | No string |
| `GP.ON_STR()` | `"On"` | On string |
| `GP.OFF_STR()` | `"Off"` | Off string |
| `GP.PEND_STR()` | `"Pend"` | Pending string |
| `GP.NORMAL()` | `"Normal"` | Normal string |
| `GP.RESET()` | `"Reset"` | Reset string |
| `GP.CANCEL_STR()` | `"Can"` | Cancel string |

### GP.VEHICLE Subclass

See [Vehicle Constants](#gpvehicle---vehicle-control) for complete reference.

### GP.SR Subclass

See [Stacker Constants](#gpsr---stacker-control) for complete reference.

### GP.ST Subclass

See [Station Constants](#gpst---station-control) for complete reference.

### GP.MOVS Subclass

See [Move Constants](#gpmovs---move-operations) for complete reference.

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

### VEHICLE.COMP Enumeration (Completion Types)

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

### VEHICLE.FORK Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NONE` | No forks |
| 1 | `FRK_1` | Fork 1 |
| 2 | `FRK_2` | Fork 2 |
| 3 | `FRK_3` | Fork 3 (quad crane) |
| 4 | `FRK_4` / `BOTH` | Fork 4 / Both forks |

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

### SR.MANUAL Enumeration

| Value | Name | Description |
|-------|------|-------------|
| 0 | `AUTO` | Auto mode |
| 1 | `MANUAL` | Manual mode |
| 2 | `STARTUP` | Starting up |
| 3 | `RUNDOWN` | Running down |

### SR.RunMode Enumeration (MUDP)

| Value | Name | Description |
|-------|------|-------------|
| 0 | `IDLE` | Prior to manual |
| 1 | `AUTO` | Auto mode |
| 2 | `SEMI` | Semi-auto mode |
| 3 | `MANUAL` | Manual mode |

### SR.RunStatus Enumeration (MUDP)

| Value | Name | Description |
|-------|------|-------------|
| 0 | `NONE` | Prior to manual |
| 1 | `OPERATE` | Waiting operate command |
| 2 | `START` | Waiting start command |
| 3 | `AUTO` | Auto status |
| 22 | `STARTREQ` | Start requested |
| 33 | `STOPREQ` | Stop requested |

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

### ST.std_err - Station Error Codes

| Code | Name | Description |
|------|------|-------------|
| 105 | `ERR_TIME_OUT_BCR` | Bar code no-read |
| 106 | `ERR_TIME_OUT_BCR_MAX` | Max no-reads |
| 107 | `ERR_UNKNOWN_LOAD` | Unknown load |
| 108 | `ERR_UNKNOWN_PART` | Unknown part |
| 109 | `ERR_NO_LOCATION` | No location found |
| 110 | `ERR_DUP_ARRIVE` | Duplicate arrival |
| 111 | `ERR_MISMATCH` | Scanner/PLC mismatch |
| 112 | `ERR_SIZING` | Profile check failed |
| 113 | `ERR_STATE` | State inconsistency |
| 114 | `ERR_WRONG_DEST` | Wrong destination |
| 115 | `ERR_STORE_PENDING` | Store pending |
| 116 | `ERR_BCR_VERIFY` | BCR verify |
| 117 | `ERR_COMM_ERROR` | Communication error |
| 118 | `ERR_FORK_ERROR` | Fork truck error |
| 119 | `ERR_OVERRIDE` | Override checks |
| 120 | `ERR_WAIT_OPER` | Wait for operator |
| 121 | `ERR_ROUTE_BUSY` | Route not available |
| 122 | `ERR_TO_MANY_STORES` | Too many stores |
| 123 | `ERR_OPER_ACCEPT` | Operator accepted |
| 124 | `ERR_DUP_HANDLE` | Duplicate handle |
| 125 | `ERR_NOT_EMPTY` | Not empty |
| 126 | `ERR_BCR_NOREAD` | BCR no read |
| 127 | `ERR_BARCODE_MISMATCH` | Barcode mismatch |
| 128 | `ERR_NO_STACK` | Stack not allowed |
| 129 | `ERR_RTNX_PICKUP` | RTNX pickup error |
| 130 | `ERR_RTNX_DROPOFF` | RTNX dropoff error |
| 131 | `ERR_DISP_EMPTY` | Dispenser empty |
| 132 | `ERR_ONE_OFF` | Count mismatch |
| 133 | `ERR_RET_COM_FAILED` | Retrieve complete failed |
| 134 | `ERR_LOAD_PROFILE` | Profile error |
| 200 | `FTP_ARRIVE` | FTP arrival |

---

## GP.MOVS - Move Operations

### MOVS.STATUS Class

| Method | Value | Description |
|--------|-------|-------------|
| `ACTV()` | "Active" | In progress |
| `ASSIGN()` | "Assign" | Awaiting assignment |
| `ASSIGNWAIT()` | "AssignWait" | Wait for shuffle |
| `CANCEL()` | "Cancel" | Cancelled |
| `CMPLT()` | "Complete" | Completed |
| `COLLECTING()` | "Collecting" | Collecting |
| `CMPREM()` | "CompRemove" | Complete + remove |
| `HOLD()` | "Hold" | On hold |
| `FULL()` | "Full" | Rack full |
| `GROUPED()` | "Grouped" | Grouped |
| `LOST()` | "Lost" | Lost |
| `PEND()` | "Pending" | Pending |
| `QUED()` | "Queued" | Queued |
| `REJECT()` | "Reject" | Rejected |
| `RMVD()` | "Removed" | Removed |
| `REQUEST()` | "Request" | Request |
| `SCHED()` | "Scheduled" | Scheduled |
| `WAIT()` | "Waiting" | Waiting |
| `WAITMODE()` | "WaitMode" | Wait mode change |
| `WAITALL()` | "WaitAlloc" | Wait allocator |
| `TIMER()` | "Timer" | Timer wait |
| `PROFILE()` | "Profile" | Profile error |

### MOVS.TYPE Class

| Method | Value | Description |
|--------|-------|-------------|
| `CANCEL()` | "Cancel" | Cancel |
| `STORE()` | "Store" | Store |
| `RETRIEVE()` | "Retrieve" | Retrieve |
| `AGVMOV()` | "AGV-Move" | AGV move |
| `RETRIEVE_4_STORE()` | "Retrieve_4_Store" | Re-store |
| `SHUFFLE()` | "Shuffle" | Shuffle |
| `TRANSFER()` | "Transfer" | Transfer |

---

## Cross-Reference with VB.NET

### Ensuring Consistency

The following classes **must match** between `global_prm.h` and `ICISDefines.vb`:

| C++ (global_prm.h) | VB.NET (ICISDefines.vb) | Notes |
|--------------------|-------------------------|-------|
| `GP.COM` | `ICISDefines.COM` | Communication status |
| `GP.DEV` | `ICISDefines.DEV` | Device types |
| `GP.ST.TYP` | `ICISDefines.ST.TYP` | Station types |
| `GP.ST.MODES` | `ICISDefines.ST.MODES` | Station modes |
| `GP.MOVS` | `ICISDefines.MOVS` | Move status/types |
| `GP.LOAD` | `ICISDefines.LOAD` | Load status |
| `GP.VEHICLE` | `ICISDefines.VEHICLE` | Vehicle functions |
| `GP.SR` | `ICISDefines.SR` | Stacker functions |
| `GP.MAX` | `ICISDefines.MAX` | System maximums |

### Index Lookup Functions

The `global_parms` class provides index-to-string lookup functions:

```cpp
// Vehicle function index to name
char* name = GP.VEHICLE_FUNC_idx(GP.VEHICLE.FUNC.MOVE);  // Returns "MOVE"

// Vehicle function name to index
long idx = GP.VEHICLE_FUNC_idx("MOVE");  // Returns GP.VEHICLE.FUNC.MOVE

// Stacker function index to name
char* name = GP.SR_FUNC_idx(GP.SR.FUNC.STOR);  // Returns "STOR"

// Station type index to name
char* name = GP.ST_TYPE_idx(GP.ST.TYP.PDIN);  // Returns "PD-IN"

// Station mode index to name
char* name = GP.ST_MODES_idx(GP.ST.MODES.STORE);  // Returns "Store"
```

---

## Usage Examples

### Checking Return Codes

```cpp
int result = performDatabaseOperation();
switch (result) {
    case GP.GOOD:
        cs_log_printf(GP_LOGLVL_3, "Operation successful");
        break;
    case GP.BAD:
        cs_log_printf(GP_LOGLVL_1, "Operation failed");
        break;
    case GP.RACK_FULL:
        cs_log_printf(GP_LOGLVL_2, "No storage space");
        break;
}
```

### Using Vehicle Functions

```cpp
// Set vehicle function
vehicle.function = GP.VEHICLE.FUNC.MOVE;
char* funcName = GP.VEHICLE_FUNC_idx(vehicle.function);
cs_log_printf(GP_LOGLVL_3, "Vehicle function: %s", funcName);

// Check schedule
if (vehicle.schedule == GP.VEHICLE.SCHD.NBSY) {
    // Vehicle is available
}
```

### Station Mode Handling

```cpp
// Set station mode
station.mode = GP.ST.MODES.STORE;

// Get mode string for logging
char* modeName = GP.ST_MODES_idx(station.mode);
cs_log_printf(GP_LOGLVL_3, "Station mode: %s", modeName);
```

---

## Related Files

- **VB.NET Equivalent:** `ICISDefines.vb` - Contains matching VB.NET definitions
- **Include Dependencies:** `ICIS_LEN.H`, `cs_dtm.h`, `device_error.h`, `global_com.h`
- **Generated Headers:** OSUB-generated table headers include `global_prm.h`

---

*Last Updated: 12/23/2024*
*Source: global_prm.h (Lines 1-3172)*
