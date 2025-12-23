# Move Constants - ICISDefines

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

## MOVS.STATUS Class - Move Status Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `ACTV` | "Active" | Move is currently in progress |
| `ASSIGN` | "Assign" | New store waiting on host assignment |
| `ASSIGNWAIT` | "AssignWait" | Wait for shuffle then find location |
| `CANCEL` | "Cancel" | Move transaction cancelled |
| `CMPLT` | "Complete" | Move completed |
| `COLLECTING` | "Collecting" | Collecting at stacker input |
| `CMPREM` | "CompRemove" | Complete followed by remove |
| `HOLD` | "Hold" | Hold until AGV delivers |
| `FULL` | "Full" | Rack is full, find shuffle |
| `GROUPED` | "Grouped" | Grouped with other moves |
| `LOST` | "Lost" | Load is lost |
| `PEND` | "Pending" | Ready to process |
| `QUED` | "Queued" | Waiting for conditions |
| `REJECT` | "Reject" | Rejected load |
| `RMVD` | "Removed" | Load removed from station |
| `REQUEST` | "Request" | Pallet request |
| `SCHED` | "Scheduled" | Output P&D waiting |
| `WAIT` | "Waiting" | Waiting for operator action |
| `WAITMODE` | "WaitMode" | Waiting for station mode change |
| `WAITALL` | "WaitAlloc" | Waiting for allocator |
| `TIMER` | "Timer" | Waiting for timer |
| `PROFILE` | "Profile" | Profile error, waiting response |

**C++ Equivalent:** `GP.MOVS.STATUS` in `global_prm.h`

**Usage:**
```vbnet
move.Status = ICISDefines.MOVS.STATUS.PEND
move.Update()
```

---

## MOVS.ACTION Class - Move Action Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `AGV_MOVE` | "AGV-Move" | AGV move operation |
| `AGV_PICKUP` | "AGV-PickUp" | AGV pickup operation |
| `AGV_DROPOFF` | "AGV-DropOff" | AGV dropoff operation |
| `BCR` | "BCR" | Bar code read operation |
| `CNV_MOVE` | "CONV-Move" | Conveyor move |
| `RTN_MOVE` | "RTN-Move" | RTN move operation |
| `SR_RET` | "SR-Retrieve" | Stacker retrieve |
| `SR_STO` | "SR-Store" | Stacker store |
| `SR_TRF_CELL` | "SR-Trf-Cell" | Stacker cell transfer |
| `SR_TRF_STND` | "SR-Trf-Stand" | Stacker stand transfer |
| `SR_AUDIT` | "SR-Audit" | Stacker audit |

**C++ Equivalent:** `GP.MOVS.ACTION` in `global_prm.h`

---

## MOVS.TYPE Class - Move Type Constants

| Constant | Value | Description |
|----------|-------|-------------|
| `CANCEL` | "Cancel" | Cancel move |
| `STORE` | "Store" | Store operation |
| `RETRIEVE` | "Retrieve" | Retrieve operation |
| `AGVMOV` | "AGV-Move" | AGV move |
| `RETRIEVE_4_STORE` | "Retrieve_4_Store" | Retrieve for re-store |
| `SHUFFLE` | "Shuffle" | Shuffle operation |
| `TRANSFER` | "Transfer" | Transfer operation |

**C++ Equivalent:** `GP.MOVS.TYPE` in `global_prm.h`

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Move Functions | [ds_movs.md](../04_DSUB/ds_movs.md) | Move database operations |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Move Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

