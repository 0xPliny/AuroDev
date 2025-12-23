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
| `ds_load.md` | `ds_load` | ❌ Pending | - |
| `ds_invt.md` | `ds_invt` | ❌ Pending | - |
| `ds_locn.md` | `ds_locn` | ❌ Pending | - |
| `ds_get_locn.md` | `ds_get_locn` | ❌ Pending | - |

---

## Priority 2 - SQL Interface

| File | Module | Status | Document |
|------|--------|--------|----------|
| `ds_sql.md` | `ds_sql` | ❌ Pending | - |

---

## Priority 3 - Supporting Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `ds_sys.md` | `ds_sys` | ❌ Pending | - |
| `ds_stk.md` | `ds_stk` | ❌ Pending | - |
| `ds_sttn.md` | `ds_sttn` | ❌ Pending | - |
| `ds_errors.md` | `ds_errors` | ❌ Pending | - |

---

## Progress Summary

| Category | Total | Documented | Pending | Progress |
|----------|-------|------------|---------|----------|
| Priority 1 | 5 | 1 | 4 | 20% |
| Priority 2 | 1 | 0 | 1 | 0% |
| Priority 3 | 4 | 0 | 4 | 0% |
| **Total** | **10** | **1** | **9** | **10%** |

---

## Notes

- DSUB functions provide business logic on top of OSUB database access
- Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.)
- DSUB functions typically call OSUB functions internally
- Business logic includes validation, calculations, and complex queries

