# Station Constants - ICISDefines

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

## ST.TYP - Station Types

| Value | Constant | Description |
|-------|----------|-------------|
| 0 | `NONE` | Undefined station type |
| 1 | `PDIN` / `STOR` | Stacker Input (Store) |
| 2 | `PDOUT` / `RTRV` | Stacker Output (Retrieve) |
| 3 | `PDBOTH` / `BOTH` | Bi-Directional Stacker |
| 4 | `PDSHUF` / `SHUF` | Aisle Shuffle |
| 5 | `BCRINPUT` | Bar Code Read Station |
| 6 | `AGVIN` | AGV Input Station |
| 7 | `AGVOUT` | AGV Output Station |
| 8 | `AGVBOTH` | Bi-Directional AGV Station |
| 9 | `TOOL` | Tool Station |
| 10 | `TRANSFER` | Aisle Transfer Station |
| 11 | `RTNIN` | RTN Input Station |
| 12 | `RTNOUT` | RTN Output Station |
| 13 | `RTNBOTH` | Bi-Directional RTN Station |
| 14 | `BLOCK` | Conveyor Block |
| 15 | `OTHER` | Other |
| 16 | `GROUP` | Group Station |
| 17 | `AGVSTD` | Stand-Alone AGV Stand |
| 18 | `AGVTOOL` | AGV Tool Port |
| 19 | `WORK` | Work Station |
| 20 | `LIFTER` | Lifter |
| 21 | `MANIN` | Manual Input |
| 22 | `MANOUT` | Manual Output |
| 23 | `MANBOTH` | Manual Both |
| 24 | `RackBoth` | Rack Pass-Through Both |
| 25 | `RackIn` | Rack Pass-Through In |
| 26 | `RackOut` | Rack Pass-Through Out |
| 27 | `Collector` | Collector Stand |
| 28 | `ShipIn` | Shipping Input |
| 29 | `ShipOut` | Shipping Output |
| 30 | `BATTERYCHARGER` | AGV Battery Charger |
| 31 | `Profiler` | Profile Check Stand |
| 32 | `AGVAPL` | AGV APL Stand |
| 33 | `TCAR` | TCar |
| 34 | `AGVDRP_CRNPCK` | AGV Drop / Crane Pickup |
| 35 | `AGVPCK_CRNDRP` | AGV Pickup / Crane Drop |
| 36 | `BUFFER` | Buffer Conveyor |

**C++ Equivalent:** `GP.ST.TYP` in `global_prm.h`

---

## ST.MODES - Station Modes

| Value | Constant | String | Description |
|-------|----------|--------|-------------|
| 0 | `NORMAL` | "Normal" | Normal operation |
| 1 | `STORE` | "Store" | Store mode |
| 2 | `RETRIEVE` | "Retrieve" | Retrieve mode |
| 3 | `MOVE` | "Move" | Station-to-Station move |
| 4 | `CELL2CELL` | "Cell2Cell" | Cell-to-cell shuffle |
| 5 | `FORCE_RETRIEVE` | "Force Retrieve" | Forced retrieve (no checks) |
| 6 | `FORCE_STORE` | "Force Store" | Forced store (no checks) |
| 7 | `STOREIN` | "Store IN" | Workstation store in |
| 8 | `STOREOUT` | "Store OUT" | Workstation store out |
| 9 | `STORECARR` | "Store IN Carrier" | Store empty carrier |
| 10 | `DIRECT` | "Direct" | Direct delivery |
| 11 | `EXERCISE` | "Exercise" | Exercise mode |
| 12 | `SUPPLY` | "Supply" | Supply mode |
| 13 | `RETURN` | "Return" | Return mode |

**C++ Equivalent:** `GP.ST.MODES` in `global_prm.h`

**Usage:**
```vbnet
If station.Mode = ICISDefines.ST.MODES.STORE Then
    ' Process store operations
End If
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Station Operations | [os_sttn.md](../01_OSUB/os_sttn.md) | Station table operations |
| C++ Equivalent | [global_prm.md](../global_prm.md) | Station Constants |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete Reference |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

