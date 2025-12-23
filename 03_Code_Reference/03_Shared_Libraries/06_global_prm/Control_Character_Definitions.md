# Control Character Definitions - global_prm

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

## ASCII Control Characters

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

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Communication | [cc_coms.md](../02_CCSUB/cc_coms.md) | Serial communication |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

