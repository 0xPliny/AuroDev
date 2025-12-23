# CSUB Library - Individual File Documentation Index

**Document Version:** 1.0  
**Last Updated:** 2024-12-23

---

## Overview

This directory contains individual file-level documentation for each CSUB (Common Subroutines) module. CSUB provides fundamental utility functions used throughout the MHC/MEM system.

**Source Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\csub\`

**Important:** CSUB files are **hand-written** C++ utility functions.

---

## Priority 1 - Most Used Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `cs_log_printf.md` | `cs_log` | ✅ Complete | [cs_log_printf.md](cs_log_printf.md) |
| `cs_msg.md` | `cs_msg` | ✅ Complete | [cs_msg.md](cs_msg.md) |
| `cs_reg.md` | `cs_reg` | ✅ Complete | [cs_reg.md](cs_reg.md) |
| `cs_dtm.md` | `cs_dtm` | ✅ Complete | [cs_dtm.md](cs_dtm.md) |

---

## Priority 2 - Configuration Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `cs_elt.md` | `cs_elt` | ❌ Pending | - |
| `cs_err.md` | `cs_err` | ❌ Pending | - |

---

## Priority 3 - Utility Modules

| File | Module | Status | Document |
|------|--------|--------|----------|
| `cs_cch.md` | `cs_cch` | ❌ Pending | - |
| `cs_mpm.md` | `cs_mpm` | ❌ Pending | - |
| `cs_tmr.md` | `cs_tmr` | ❌ Pending | - |
| `cs_txt.md` | `cs_txt` | ❌ Pending | - |
| `cs_sem.md` | `cs_sem` | ❌ Pending | - |
| `cs_seq.md` | `cs_seq` | ❌ Pending | - |

---

## Progress Summary

| Category | Total | Documented | Pending | Progress |
|----------|-------|------------|---------|----------|
| Priority 1 | 4 | 4 | 0 | **100%** ✅ |
| Priority 2 | 2 | 0 | 2 | 0% |
| Priority 3 | 6 | 0 | 6 | 0% |
| **Total** | **12** | **4** | **8** | **33%** |

---

## Notes

- CSUB functions are fundamental utilities used by all MHC processes
- Functions return standard MHC return codes (`GP.GOOD`, `GP.BAD`, etc.)
- Many functions depend on configuration files (e.g., `log.cnf`, `DBINI.CNF`)

