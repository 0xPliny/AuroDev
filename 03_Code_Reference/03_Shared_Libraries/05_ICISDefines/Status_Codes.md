# Status Codes - ICISDefines

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

## COM Class - Communication Status

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

### Communication Status Values

| Value | Constant | Description |
|-------|----------|-------------|
| 0 | `OFFL` | Offline |
| 1 | `ONLI` | Online |
| 2 | `RUND` | Rundown (going offline) |
| 3 | `INIT` | Initializing (going online) |
| 4 | `STRT` | Start |
| 5 | `CLER` | Clear |
| 6 | `HOME` | Home |
| 7 | `RSET` | Reset |
| 8 | `ESTP` | Emergency Stop |
| 9 | `DISD` | Disabled |
| 10 | `WAIT` | Waiting |
| 11 | `WAIT_LISTEN` | Wait Listen |
| 12 | `WAIT_OUTPUT` | Wait Output |
| 13 | `ATCH` | Attaching |
| 14 | `TERR` | Attach Error |

**Usage:**
```vbnet
If device.CommStatus = ICISDefines.COM.ONLI Then
    ' Device is online
End If
```

**C++ Equivalent:** `GP.COM` in `global_prm.h`

---

## ESTAT Class - Error Status

**Description:** Error status constants.

```vbnet
Public Class ESTAT
    Public Shared ReadOnly iERROR As String = "ERROR"
    Public Shared ReadOnly OFFLINE As String = "OFFLINE"
    Public Shared ReadOnly ONLINE As String = "ONLINE"
End Class
```

### Error Status Values

| Constant | Value | Description |
|----------|-------|-------------|
| `iERROR` | `"ERROR"` | Error condition |
| `OFFLINE` | `"OFFLINE"` | Device offline |
| `ONLINE` | `"ONLINE"` | Device online |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| C++ Equivalent | [global_prm.md](../global_prm.md) | Status Codes |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

