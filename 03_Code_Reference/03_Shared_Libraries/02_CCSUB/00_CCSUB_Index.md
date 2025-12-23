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
| `cc_stk.md` | `cc_stk` | ❌ Pending | - |
| `cc_vehicle.md` | `cc_vehicle` | ❌ Pending | - |
| `cc_plc.md` | `cc_plc` | ❌ Pending | - |

---

## Priority 2 - Communication Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_mudp.md` | `cc_mudp` | ❌ Pending | - |
| `cc_coms.md` | `cc_coms` | ❌ Pending | - |

---

## Priority 3 - System Modules

| File | Class | Status | Document |
|------|-------|--------|----------|
| `cc_sys.md` | `cc_sys` | ❌ Pending | - |
| `cc_mem.md` | `cc_mem` | ❌ Pending | - |
| `cc_gg.md` | `cc_gg` | ❌ Pending | - |
| `cc_prc.md` | `cc_prc` | ❌ Pending | - |

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
| Priority 1 | 4 | 1 | 3 | 25% |
| Priority 2 | 2 | 0 | 2 | 0% |
| Priority 3 | 4 | 0 | 4 | 0% |
| Priority 4 | 4 | 0 | 4 | 0% |
| **Total** | **14** | **1** | **13** | **7%** |

---

## Notes

- CCSUB classes are static class instances (e.g., `cc_str`, `cc_stk`)
- Classes interact with mapped memory for real-time data access
- Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.)

