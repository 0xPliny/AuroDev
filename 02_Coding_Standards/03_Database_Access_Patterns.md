# Database Access Patterns

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes database access patterns used in the Auro System, including OSUB API usage and direct SQL patterns.

---

## OSUB API Usage

### Generated Routines
- **Source:** Generated from `database_tables.xml` by `p_ut_makdb.exe`
- **Location:** `MSVB Programs\osub\z_*.vb` (VB.NET) or `osub\z_*.cpp` (C++)
- **Naming:** `z_[table_synonym]` (e.g., `z_movs`, `z_load`, `z_invt`)

### VB.NET Pattern
```vbnet
' Select by primary key
Dim moveRecord As New z_movs_def
If TBL_select_TBL_PR_KEY("MOVS", moveID, moveRecord) Then
    ' Process moveRecord
    moveRecord.Move_Status = "COMPLETE"
    TBL_update_TBL_PR_KEY("MOVS", moveID, moveRecord)
End If
```

### C++ Pattern
```cpp
// Select by primary key
z_movs_def moveRecord;
if (TBL_select_TBL_PR_KEY("MOVS", moveID, moveRecord)) {
    // Process moveRecord
    moveRecord.Move_Status = "COMPLETE";
    TBL_update_TBL_PR_KEY("MOVS", moveID, moveRecord);
}
```

---

## Direct SQL Usage

### When to Use
- Complex queries not covered by OSUB
- Joins across multiple tables
- Aggregations and reporting

### Pattern
```vbnet
Dim sql As String = "SELECT * FROM MHC_MOVS WHERE Move_Status = @Status"
Dim cmd As New SqlCommand(sql, connection)
cmd.Parameters.AddWithValue("@Status", "PENDING")
' Execute query
```

---

## Related Documents

- [Standards Overview](00_Standards_Overview.md)
- [Database Overview](../04_Database_Reference/00_Database_Overview.md)
- [MHC-Developers-Guide Database](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/04_Database_Reference)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Database Tables | [Database Reference](../04_Database_Reference/) | Table Documentation |
| OSUB Generation | [Configuration](../06_Configuration/02_XML_Files.md) | database_tables.xml |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

