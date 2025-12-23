# System Constants - ICISDefines

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

## System Maximums (MAX Class)

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

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| C++ Equivalent | [global_prm.md](../global_prm.md) | System Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

