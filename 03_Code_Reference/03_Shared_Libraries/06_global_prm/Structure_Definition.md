# Structure Definitions - global_prm

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

## Fundamental Types

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

---

## Null Definitions

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_NULLP` | `(void *) 0` | NULL pointer |
| `GP_NULLC` | `'\0'` | NULL character |
| `GP_NULL` | `0` | NULL value |

**Usage:**
```cpp
char* buffer = GP_NULLP;
if (buffer == GP_NULLP) {
    // Handle null pointer
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Original File | [global_prm.md](../global_prm.md) | Type Definitions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

