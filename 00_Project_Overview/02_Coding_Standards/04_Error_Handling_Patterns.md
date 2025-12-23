# Error Handling Patterns

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes error handling patterns used in the Auro System, including error detection, logging, and recovery procedures.

---

## Error Detection

### Mapped Memory Error Flags
- **Process:** Monitor error flags in shared memory
- **Pattern:**
```cpp
cc_stk stacker;
if (stacker.GetError()) {
    int errorCode = stacker.GetErrorCode();
    // Handle error
}
```

### Database Errors
- **Pattern:** Check return values from database operations
- **Example:**
```vbnet
If Not TBL_select_TBL_PR_KEY("MOVS", moveID, moveRecord) Then
    ' Handle error - record not found
End If
```

---

## Error Logging

### Database Logging
- **Table:** `MHC_ERROR`
- **Process:** `p_sy_errdev.exe` creates error records
- **History:** `MHC_ERROR_HISTORY` table

### File Logging
- **Process:** `p_sy_erlog.exe` writes to log files
- **Location:** `D:\Auro\Logs\`

---

## Error Recovery

### Automatic Recovery
- **Pattern:** Processes attempt automatic recovery
- **Example:** Retry failed operations, reset error flags

### Manual Recovery
- **Pattern:** Operator intervention via UI dialogs
- **Example:** `frm_MEI_Recovery.vb`, `frm_Sttn_Recovery.vb`

---

## Related Documents

- [Standards Overview](00_Standards_Overview.md)
- [Equipment Fault Handling](../05_Workflows/03_Equipment_Fault_Handling.md)
- [Troubleshooting](../07_Troubleshooting/00_Troubleshooting_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Error Workflow | [Equipment Fault Handling](../05_Workflows/03_Equipment_Fault_Handling.md) | Error Flow |
| Error Tables | [Database Reference](../04_Database_Reference/01_Core_Tables/MHC_ERROR.md) | Error Schema |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Error Solutions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

