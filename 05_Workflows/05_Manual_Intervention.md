# Manual Intervention

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes manual intervention procedures for operators to resolve problems and perform manual operations.

---

## Manual Operations

### Move Maintenance
- **Dialog:** `frm_Move_Maintenance.vb`
- **Purpose:** Manually create, modify, or cancel moves
- **Actions:**
  - Create new move record
  - Modify move status
  - Cancel pending moves
  - Force complete moves

### Station Recovery
- **Dialog:** `frm_Sttn_Recovery.vb`
- **Purpose:** Recover from station errors
- **Actions:**
  - Clear station error flags
  - Reset station status
  - Manually update load presence

### MEI Recovery
- **Dialog:** `frm_MEI_Recovery.vb`
- **Purpose:** Recover from material handling errors
- **Actions:**
  - Clear equipment error flags
  - Reset equipment status
  - Manually complete moves

### Manual Store
- **Dialog:** `frm_Store.vb`
- **Purpose:** Manually create store operations
- **Actions:**
  - Enter load information
  - Select destination location
  - Create move record

### Manual Retrieve
- **Dialog:** `frm_Manual_Retrieve.vb`
- **Purpose:** Manually create retrieve operations
- **Actions:**
  - Enter load ID or location
  - Select destination station
  - Create move record

---

## Related Documents

- [Workflow Index](00_Workflow_Index.md)
- [UI Dialogs](../03_Code_Reference/02_UI_Dialogs/)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Move Maintenance | [frm_Move_Maintenance](../03_Code_Reference/02_UI_Dialogs/frm_Move_Maintenance.md) | Dialog |
| Station Recovery | [frm_Sttn_Recovery](../03_Code_Reference/02_UI_Dialogs/frm_Sttn_Recovery.md) | Dialog |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Manual Procedures |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

