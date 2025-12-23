# ICISDefines - Categorized Documentation Index

**Document Version:** 1.0  
**Last Updated:** 2024-12-23

---

## Overview

This directory contains categorized documentation for `ICISDefines.vb`, breaking down the comprehensive constants and definitions into logical categories for easier reference.

**Source File:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\ICISDefines.vb`

**Important:** ICISDefines.vb is **hand-written** VB.NET constants file. Must be kept synchronized with `global_prm.h` in C++.

---

## Categorized Documentation Files

| File | Category | Status | Document |
|------|----------|--------|----------|
| `System_Constants.md` | System Type, Date/Time, Global Variables, MAX | ✅ Complete | [System_Constants.md](System_Constants.md) |
| `Equipment_Type_Codes.md` | Device Types, Equipment Classifications | ✅ Complete | [Equipment_Type_Codes.md](Equipment_Type_Codes.md) |
| `Status_Codes.md` | Communication, Error Status, General Status | ✅ Complete | [Status_Codes.md](Status_Codes.md) |
| `Move_Constants.md` | Move Status, Actions, Types | ✅ Complete | [Move_Constants.md](Move_Constants.md) |
| `Vehicle_Constants.md` | Vehicle Functions, Schedule, Completion | ✅ Complete | [Vehicle_Constants.md](Vehicle_Constants.md) |
| `Stacker_Constants.md` | Stacker Functions, Schedule, Modes | ✅ Complete | [Stacker_Constants.md](Stacker_Constants.md) |
| `Station_Constants.md` | Station Types, Station Modes | ✅ Complete | [Station_Constants.md](Station_Constants.md) |
| `Location_Constants.md` | Location Status, Pending, Locked | ✅ Complete | [Location_Constants.md](Location_Constants.md) |
| `Load_Inventory_Constants.md` | Load Status, Inventory Status, Location Codes | ✅ Complete | [Load_Inventory_Constants.md](Load_Inventory_Constants.md) |
| `Error_Codes.md` | Station Error Codes, System Errors | ✅ Complete | [Error_Codes.md](Error_Codes.md) |

---

## Progress Summary

| Category | Total | Documented | Progress |
|----------|-------|------------|----------|
| All Categories | 10 | 10 | **100%** ✅ |

---

## Cross-Reference

| Topic | Document | Section |
|-------|----------|---------|
| C++ Equivalent | [global_prm.md](../global_prm.md) | Must match ICISDefines |
| Original File | [ICISDefines.md](../ICISDefines.md) | Complete reference |

---

## Notes

- All constants must match between `ICISDefines.vb` and `global_prm.h`
- Numeric values must be identical
- Enumeration order must be preserved
- Changes require updates to both files

