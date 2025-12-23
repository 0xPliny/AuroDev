# Error Codes - ICISDefines

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

## Station Error Codes (ST.std_err)

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

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Error Handling | [cs_err.md](../03_CSUB/cs_err.md) | Error utilities |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Error Codes |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

