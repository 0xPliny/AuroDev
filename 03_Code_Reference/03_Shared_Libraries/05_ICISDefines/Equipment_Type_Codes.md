# Equipment Type Codes - ICISDefines

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

## DEV Class - Device Types

**Description:** Enumeration of device types in the MHC system.

```vbnet
Public Class DEV
    Public Enum DeviceType
        PLC = 0           ' Programmable Logic Controller
        STACKER = 1       ' Stacker Crane
        STATION = 2      ' Station/Stand
        COMMUNICATION = 3 ' Communication Device
        AGV = 4          ' Automated Guided Vehicle
        RTN = 5          ' Rail Guided Vehicle (RTNX)
        LABELER = 6      ' Labeling Device
        ROBOT = 7        ' Robot
        RTNAREASLOT = 8  ' RTN Area Slot
    End Enum
End Class
```

### Device Type Values

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

**Usage:**
```vbnet
If device.DeviceType = ICISDefines.DEV.DeviceType.STACKER Then
    ' Handle stacker-specific logic
End If
```

**C++ Equivalent:** `GP.DEV.type_of_device` in `global_prm.h`

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| C++ Equivalent | [global_prm.md](../global_prm.md) | Device Types |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

