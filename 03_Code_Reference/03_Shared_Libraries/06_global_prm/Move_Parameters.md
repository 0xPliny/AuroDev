# Move Parameters - global_prm

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

## GP.MOVS - Move Operations

### MOVS.STATUS Class (Methods)

| Method | Value | Description |
|--------|-------|-------------|
| `ACTV()` | "Active" | In progress |
| `ASSIGN()` | "Assign" | Awaiting assignment |
| `ASSIGNWAIT()` | "AssignWait" | Wait for shuffle |
| `CANCEL()` | "Cancel" | Cancelled |
| `CMPLT()` | "Complete" | Completed |
| `COLLECTING()` | "Collecting" | Collecting |
| `CMPREM()` | "CompRemove" | Complete + remove |
| `HOLD()` | "Hold" | On hold |
| `FULL()` | "Full" | Rack full |
| `GROUPED()` | "Grouped" | Grouped |
| `LOST()` | "Lost" | Lost |
| `PEND()` | "Pending" | Pending |
| `QUED()` | "Queued" | Queued |
| `REJECT()` | "Reject" | Rejected |
| `RMVD()` | "Removed" | Removed |
| `REQUEST()` | "Request" | Request |
| `SCHED()` | "Scheduled" | Scheduled |
| `WAIT()` | "Waiting" | Waiting |
| `WAITMODE()` | "WaitMode" | Wait mode change |
| `WAITALL()` | "WaitAlloc" | Wait allocator |
| `TIMER()` | "Timer" | Timer wait |
| `PROFILE()` | "Profile" | Profile error |

**Usage:**
```cpp
if (move.status == GP.MOVS.STATUS.PEND()) {
    // Move is pending
}
```

### MOVS.TYPE Class (Methods)

| Method | Value | Description |
|--------|-------|-------------|
| `CANCEL()` | "Cancel" | Cancel |
| `STORE()` | "Store" | Store |
| `RETRIEVE()` | "Retrieve" | Retrieve |
| `AGVMOV()` | "AGV-Move" | AGV move |
| `RETRIEVE_4_STORE()` | "Retrieve_4_Store" | Re-store |
| `SHUFFLE()` | "Shuffle" | Shuffle |
| `TRANSFER()` | "Transfer" | Transfer |

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Move Functions | [ds_movs.md](../04_DSUB/ds_movs.md) | Move operations |
| VB.NET Equivalent | [Move_Constants.md](../05_ICISDefines/Move_Constants.md) | ICISDefines |
| Original File | [global_prm.md](../global_prm.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

