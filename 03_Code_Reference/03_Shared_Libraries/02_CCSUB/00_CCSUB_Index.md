# CCSUB Library - Individual File Documentation Index

**Document Version:** 1.0  
**Last Updated:** 2024-12-23

---

## Overview

This directory contains individual file-level documentation for each CCSUB (Common Control Subroutines) module. CCSUB provides C++ classes for equipment control, communication, and system management.

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\`

**Important:** CCSUB classes are **NOT GENERATED** - they are hand-written C++ classes.

---

## Priority 1 - Most Used Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_str.md` | `cc_str` | ✅ Complete | [cc_str.md](cc_str.md) |
| `cc_stk.md` | `cc_stk` | ✅ Complete | [cc_stk.md](cc_stk.md) |
| `cc_vehicle.md` | `cc_vehicle` | ✅ Complete | [cc_vehicle.md](cc_vehicle.md) |
| `cc_plc.md` | `cc_plc` | ✅ Complete | [cc_plc.md](cc_plc.md) |

---

## Priority 2 - Communication Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_mudp.md` | `cc_mudp` | ✅ Complete | [cc_mudp.md](cc_mudp.md) |
| `cc_coms.md` | `cc_coms` | ✅ Complete | [cc_coms.md](cc_coms.md) |

---

## Priority 3 - System Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_sys.md` | `cc_sys` | ✅ Complete | [cc_sys.md](cc_sys.md) |
| `cc_mem.md` | `cc_mem` | ✅ Complete | [cc_mem.md](cc_mem.md) |
| `cc_gg.md` | `cc_gg` | ✅ Complete | [cc_gg.md](cc_gg.md) |
| `cc_prc.md` | `cc_prc` | ✅ Complete | [cc_prc.md](cc_prc.md) |

---

## Priority 4 - Utility Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_route.md` | `cc_route` | ❌ Pending | - |
| `cc_server.md` | `cc_server` | ❌ Pending | - |
| `cc_tim.md` | `cc_tim` | ❌ Pending | - |
| `cc_std.md` | `cc_std` | ❌ Pending | - |

---

## Progress Summary

| Category | Total | Documented | Pending | Progress |
|----------|-------|------------|---------|----------|
| Priority 1 | 4 | 4 | 0 | **100%** ✅ |
| Priority 2 | 2 | 2 | 0 | **100%** ✅ |
| Priority 3 | 4 | 4 | 0 | **100%** ✅ |
| Priority 4 | 4 | 0 | 4 | 0% |
| **Total** | **14** | **10** | **4** | **71%** |

---

## Notes

- CCSUB classes are static class instances (e.g., `cc_str`, `cc_stk`)
- Classes interact with mapped memory for real-time data access
- Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.)

