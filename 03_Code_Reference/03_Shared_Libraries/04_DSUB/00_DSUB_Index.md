# DSUB Library - Individual File Documentation Index

**Document Version:** 1.0  
**Last Updated:** 2024-12-23

---

## Overview

This directory contains individual file-level documentation for each DSUB (Database Subroutines) module. DSUB provides database access functions and business logic for database operations.

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\`

**Important:** DSUB files are **hand-written** C++ database functions with business logic.

---

## Priority 1 - Most Used Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `ds_movs.md` | `ds_movs` | ✅ Complete | [ds_movs.md](ds_movs.md) |
| `ds_load.md` | `ds_load` | ✅ Complete | [ds_load.md](ds_load.md) |
| `ds_invt.md` | `ds_invt` | ✅ Complete | [ds_invt.md](ds_invt.md) |
| `ds_locn.md` | `ds_locn` | ✅ Complete | [ds_locn.md](ds_locn.md) |
| `ds_get_locn.md` | `ds_get_locn` | ✅ Complete | [ds_get_locn.md](ds_get_locn.md) |

---

## Priority 2 - SQL Interface

| File | Module | Status | Document |
|------|--------|--------|----------|
| `ds_sql.md` | `ds_sql` | ✅ Complete | [ds_sql.md](ds_sql.md) |

---

## Priority 3 - Supporting Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `ds_sys.md` | `ds_sys` | ✅ Complete | [ds_sys.md](ds_sys.md) |
| `ds_stk.md` | `ds_stk` | ✅ Complete | [ds_stk.md](ds_stk.md) |
| `ds_sttn.md` | `ds_sttn` | ✅ Complete | [ds_sttn.md](ds_sttn.md) |
| `ds_errors.md` | `ds_errors` | ✅ Complete | [ds_errors.md](ds_errors.md) |

---

## Progress Summary

| Category | Total | Documented | Pending | Progress |
|----------|-------|------------|---------|----------|
| Priority 1 | 5 | 5 | 0 | **100%** ✅ |
| Priority 2 | 1 | 1 | 0 | **100%** ✅ |
| Priority 3 | 4 | 4 | 0 | **100%** ✅ |
| **Total** | **10** | **10** | **0** | **100%** ✅ |

---

## Notes

- DSUB functions provide business logic on top of OSUB database access
- Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.)
- DSUB functions typically call OSUB functions internally
- Business logic includes validation, calculations, and complex queries

