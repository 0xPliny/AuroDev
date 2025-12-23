# System Parameters - global_prm

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

## Return Codes (GP Class)

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

---

## Boolean Constants

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

---

## Log Level Constants

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

**Usage:**
```cpp
cs_log_printf(GP_LOGLVL_3, "Processing move %d", moveId);
```

---

## System Maximums (GP_MAX)

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

---

## Comparison Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `GP_MATCH` | `0` | strcmp match |
| `GP_LESS` | `-1` | Less than |
| `GP_GREATER` | `1` | Greater than |

---

## On/Off Enumeration

```cpp
enum {
    GP_OFF,    // 0 = Discrete status OFF
    GP_ON      // 1 = Discrete status ON
};
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Logging | [cs_log_printf.md](../03_CSUB/cs_log_printf.md) | Log level usage |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

