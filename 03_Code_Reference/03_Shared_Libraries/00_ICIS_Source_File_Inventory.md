# ICIS Source File Inventory

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Purpose:** Complete inventory of all source files in ICIS directory for documentation tracking

---

## OSUB Files (Generated Database Access Routines)

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\osub\`

**Total Files:** ~180 files (90 .cpp/.h pairs)

### Core Tables (Priority 1 - Document First)
| File | Table | Size | Documented | Priority |
|------|-------|------|------------|----------|
| movs.cpp/h | MHC_MOVS | ~50KB | ❌ | **CRITICAL** |
| load.cpp/h | MHC_LOAD | ~40KB | ❌ | **CRITICAL** |
| locn.cpp/h | MHC_LOCATION | ~35KB | ❌ | **CRITICAL** |
| invt.cpp/h | MHC_INVENTORY | ~30KB | ❌ | **CRITICAL** |
| sys.cpp/h | MHC_SYS | ~25KB | ❌ | **HIGH** |
| stk.cpp/h | MHC_STK | ~30KB | ❌ | **HIGH** |
| sttn.cpp/h | MHC_STTN | ~25KB | ❌ | **HIGH** |

### Supporting Tables (Priority 2)
| File | Table | Documented |
|------|-------|------------|
| ordr.cpp/h | MHC_ORDER | ❌ |
| pall.cpp/h | MHC_PALLET | ❌ |
| groups.cpp/h | MHC_GROUPS | ❌ |
| grpmem.cpp/h | MHC_GRPMEM | ❌ |
| route.cpp/h | MHC_ROUTE | ❌ |
| errors.cpp/h | MHC_ERRORS | ❌ |
| errhist.cpp/h | MHC_ERROR_HISTORY | ❌ |
| stats.cpp/h | MHC_STATS | ❌ |

### View Tables (Priority 3)
| File | View | Documented |
|------|------|------------|
| vlocn_load.cpp/h | VLOCN_LOAD | ❌ |
| vload_invt.cpp/h | VLOAD_INVT | ❌ |
| vmov_load_invt.cpp/h | VMOV_LOAD_INVT | ❌ |
| vlocn_select.cpp/h | VLOCN_SELECT | ❌ |
| vitem.cpp/h | VITEM | ❌ |
| vhstinq.cpp/h | VHSTINQ | ❌ |

### History/Log Tables (Priority 4)
| File | Table | Documented |
|------|-------|------------|
| movs_log.cpp/h | MHC_MOVS_LOG | ❌ |
| load_log.cpp/h | MHC_LOAD_LOG | ❌ |
| invt_log.cpp/h | MHC_INVENTORY_LOG | ❌ |
| locn_log.cpp/h | MHC_LOCATION_LOG | ❌ |
| hstin_log.cpp/h | MHC_HOSTIN_LOG | ❌ |
| hstout_log.cpp/h | MHC_HOSTOUT_LOG | ❌ |

### Configuration Tables (Priority 5)
| File | Table | Documented |
|------|-------|------------|
| elem.cpp/h | MHC_ELEMENTS | ❌ |
| conf.cpp/h | MHC_CONF | ❌ |
| dev.cpp/h | MHC_DEV | ❌ |
| devtyp.cpp/h | MHC_DEVTYP | ❌ |
| sttntype.cpp/h | MHC_STTNTYPE | ❌ |
| loadtype.cpp/h | MHC_LOADTYPE | ❌ |
| movtype.cpp/h | MHC_MOVTYPE | ❌ |

### Security Tables (Priority 6)
| File | Table | Documented |
|------|-------|------------|
| users.cpp/h | MHC_USERS | ❌ |
| ugroup.cpp/h | MHC_UGROUP | ❌ |
| menu.cpp/h | MHC_MENU | ❌ |
| actperm.cpp/h | MHC_ACTPERM | ❌ |

### Zone/Location Tables (Priority 7)
| File | Table | Documented |
|------|-------|------------|
| zolo.cpp/h | MHC_ZOLO | ❌ |
| zord.cpp/h | MHC_ZORD | ❌ |
| zoco.cpp/h | MHC_ZOCO | ❌ |
| zclx.cpp/h | MHC_ZCLX | ❌ |

---

## CCSUB Files (Common Control Subroutines)

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\`

**Total Files:** ~60 files (30 .cpp/.h pairs)

### Critical Files (Priority 1)
| File | Purpose | Documented | Priority |
|------|---------|------------|----------|
| cc_str.cpp/h | String operations | ❌ | **CRITICAL** |
| cc_gg.cpp/h | Global variables | ❌ | **CRITICAL** |
| cc_stk.cpp/h | Stacker control | ❌ | **HIGH** |
| cc_std.cpp/h | Stand control | ❌ | **HIGH** |
| cc_zone.cpp/h | Zone control | ❌ | **HIGH** |

### Equipment Control (Priority 2)
| File | Purpose | Documented |
|------|---------|------------|
| cc_agv.cpp/h | AGV control | ❌ |
| cc_rtnx.cpp/h | RTNX control | ❌ |
| cc_plc.cpp/h | PLC communication | ❌ |
| cc_vehicle.cpp/h | Vehicle base class | ❌ |

### Communication (Priority 3)
| File | Purpose | Documented |
|------|---------|------------|
| cc_mudp.cpp/h | MUDP protocol | ❌ |
| cc_coms.cpp/h | Communications | ❌ |
| cc_server.cpp/h | Server functions | ❌ |

### Utilities (Priority 4)
| File | Purpose | Documented |
|------|---------|------------|
| cc_mem.cpp/h | Memory management | ❌ |
| cc_prc.cpp/h | Process control | ❌ |
| cc_route.cpp/h | Routing logic | ❌ |
| cc_sys.cpp/h | System control | ❌ |
| cc_sts.cpp/h | Statistics | ❌ |
| cc_tim.cpp/h | Timing utilities | ❌ |

### CSUB Functions in CCSUB (Priority 5)
| File | Purpose | Documented |
|------|---------|------------|
| CS_LOG.Cpp/H | Logging | ❌ |
| cs_msg.cpp/CS_MSG.H | Messaging | ❌ |
| cs_reg.cpp/h | Registry | ❌ |
| cs_dtm.cpp/h | Date/time | ❌ |
| cs_elt.cpp/h | Elements | ❌ |
| cs_err.cpp/h | Errors | ❌ |
| cs_sem.cpp/CS_SEM.H | Semaphores | ❌ |
| cs_txt.cpp/CS_TXT.H | Text translation | ❌ |

---

## CSUB Files (Project-Specific Common Subroutines)

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\`

**Total Files:** ~40 files (20 .cpp/.h pairs)

### Critical Files (Priority 1)
| File | Purpose | Documented | Priority |
|------|---------|------------|----------|
| cs_init.cpp | Process initialization | ❌ | **HIGH** |
| cs_plc.cpp/h | PLC operations | ❌ | **HIGH** |

### Equipment-Specific (Priority 2)
| File | Purpose | Documented |
|------|---------|------------|
| cc_agv.cpp/h | AGV operations | ❌ |
| cc_rtnx.cpp/h | RTNX operations | ❌ |
| cc_zone.cpp/h | Zone operations | ❌ |
| cc_group.cpp/h | Group operations | ❌ |
| cc_common.cpp/h | Common utilities | ❌ |

### Host Communication (Priority 3)
| File | Purpose | Documented |
|------|---------|------------|
| cc_hosout.cpp/h | Host output | ❌ |
| CS_HST.Cpp/H | Host functions | ❌ |

---

## DSUB Files (Database Subroutines)

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\`

**Total Files:** ~60 files (30 .cpp/.h pairs)

### Core SQL (Priority 1 - Already Documented)
| File | Purpose | Documented | Status |
|------|---------|------------|--------|
| ds_sql.cpp/h | SQL interface | ✅ | Overview done |
| ds_sql_SQL.cpp/h | SQL Server impl | ❌ | Needs detail |
| ds_exec.cpp/h | Stored procedures | ❌ | Needs detail |

### Move Operations (Priority 2)
| File | Purpose | Documented |
|------|---------|------------|
| ds_movs.cpp/h | Move operations | ❌ |
| ds_movs_counts.cpp/h | Move counting | ❌ |
| ds_movs_over.cpp/h | Move overflow | ❌ |

### Location Operations (Priority 3)
| File | Purpose | Documented |
|------|---------|------------|
| ds_get_locn.cpp/h | Location selection | ❌ |
| ds_locn_counts.cpp/h | Location counting | ❌ |
| ds_util_locn.cpp/h | Location utilities | ❌ |

### Stand/Stacker Operations (Priority 4)
| File | Purpose | Documented |
|------|---------|------------|
| ds_stand.cpp/h | Stand operations | ❌ |
| ds_sr.cpp/h | Stacker operations | ❌ |
| ds_group.cpp/h | Group operations | ❌ |

### Load Operations (Priority 5)
| File | Purpose | Documented |
|------|---------|------------|
| ds_create_load.cpp/h | Load creation | ❌ |
| ds_get_loadid.cpp/h | Load ID retrieval | ❌ |
| ds_get_loadType.cpp/h | Load type retrieval | ❌ |

### Getter Functions (Priority 6)
| File | Purpose | Documented |
|------|---------|------------|
| ds_get_MoveId.cpp/h | Move ID generation | ❌ |
| ds_get_MoveNo.cpp/h | Move number | ❌ |
| ds_get_order.cpp/h | Order retrieval | ❌ |
| ds_get_Pallet.cpp/h | Pallet retrieval | ❌ |
| [30+ more ds_get_* files] | Various getters | ❌ |

### Host Communication (Priority 7)
| File | Purpose | Documented |
|------|---------|------------|
| ds_hostout.cpp/h | Host output messages | ❌ |

---

## Documentation Status Summary

| Category | Total Files | Documented | Remaining | Progress |
|----------|-------------|------------|-----------|----------|
| OSUB | ~180 | 0 | 180 | 0% |
| CCSUB | ~60 | 0 | 60 | 0% |
| CSUB | ~40 | 0 | 40 | 0% |
| DSUB | ~60 | 0 | 60 | 0% |
| **TOTAL** | **~340** | **0** | **340** | **0%** |

---

## Documentation Priority Order

1. **Phase 1:** OSUB Core Tables (movs, load, locn, invt, sys, stk, sttn)
2. **Phase 2:** CCSUB Critical (cc_str, cc_gg, cc_stk, cc_std, cc_zone)
3. **Phase 3:** DSUB Core (ds_sql detail, ds_movs, ds_get_locn)
4. **Phase 4:** OSUB Supporting Tables
5. **Phase 5:** CCSUB Equipment Control
6. **Phase 6:** Remaining files

---

*Last Updated: 2024-12-23*

