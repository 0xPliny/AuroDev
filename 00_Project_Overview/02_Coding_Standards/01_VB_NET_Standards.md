# VB.NET Standards

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes VB.NET coding standards specific to the Auro System. For generic VB.NET standards, see [MHC-Developers-Guide VB.NET Standards](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards).

---

## Auro-Specific VB.NET Patterns

### Dialog Naming
- **Prefix:** `frm_` for forms
- **Examples:** `frm_Store.vb`, `frm_MoveInq.vb`, `frm_Inventory_Inquiry.vb`
- **Location:** `MSVB Programs\dilg\mhcMenu\`

### Database Access
- **Generated Routines:** Use `z_*.vb` files from `osub\` directory
- **Example:**
```vbnet
Dim moveRecord As New z_movs_def
If TBL_select_TBL_PR_KEY("MOVS", moveID, moveRecord) Then
    ' Process moveRecord
End If
```

### Mapped Memory Access
- **Method:** Use `VBDLL` class for accessing shared memory
- **Example:**
```vbnet
Dim stackerHandle As IntPtr = VBDLL.GetStackerHandle(aisleNumber)
Dim schedule As Integer = VBDLL.GetStackerSchedule(stackerHandle)
```

---

## Related Documents

- [Standards Overview](00_Standards_Overview.md)
- [Database Access Patterns](03_Database_Access_Patterns.md)
- [MHC-Developers-Guide VB.NET](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/02_Coding_Standards)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Access | [Database Access Patterns](03_Database_Access_Patterns.md) | OSUB Usage |
| UI Dialogs | [Code Reference](../03_Code_Reference/02_UI_Dialogs/) | Dialog Documentation |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

