# Equipment Fault Handling

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document describes the workflow for detecting, logging, notifying, and recovering from equipment faults in the Auro System.

---

## Workflow Steps

### Step 1: Error Detection
- **Source:** Equipment (Stacker, PLC, AGV, etc.)
- **Action:** Equipment detects error condition
- **Communication:** Sends error code via communication protocol

### Step 2: Communication Process Receives Error
- **Process:** `p_cc_stkcmx`, `Kepware_PLC`, `p_cc_mudpcm`
- **Action:**
  - Receives error message from equipment
  - Updates mapped memory: Error Flag=TRUE, Error Code
  - Logs error to communication log

### Step 3: Error Device Monitor
- **Process:** `p_sy_errdev` (Error Device Monitor)
- **Action:**
  - Monitors error flags in mapped memory
  - Detects new error condition
  - Creates `MHC_ERROR` record
  - Fields: Error_ID, Device, Error_Code, Timestamp, Status='ACTIVE'

### Step 4: Error Logging
- **Process:** `p_sy_erlog` (Error Logging)
- **Action:**
  - Writes error to log file (`D:\Auro\Logs\error.log`)
  - Inserts into `MHC_ERROR_HISTORY` table
  - Formats error message using `MHC_ERROR_DESCRIPTION`

### Step 5: Email Notification
- **Process:** `p_sy_emailer` (Email Service)
- **Action:**
  - Queries `MHC_EML_ERROR_SUBSCRIPTION` for subscribers
  - Creates email message with error details
  - Inserts into `MHC_EML_EMAIL_QUEUE`
  - Sends email notification

### Step 6: UI Display
- **Process:** `mhcMenu.exe`
- **Action:**
  - Queries `MHC_ERROR` for active errors
  - Displays error in `frm_ErrorsMini` dialog
  - Shows error code, description, device, timestamp
  - Operator can acknowledge error

### Step 7: Error Recovery
- **Automatic Recovery:**
  - Process attempts automatic recovery
  - Retries failed operations
  - Resets error flags if recovery successful
- **Manual Recovery:**
  - Operator uses recovery dialogs (`frm_MEI_Recovery`, `frm_Sttn_Recovery`)
  - Manually clears error conditions
  - Updates error status

### Step 8: Error Clearance
- **Process:** `p_sy_errdev`
- **Action:**
  - Monitors error flags in mapped memory
  - Detects error cleared (Error Flag=FALSE)
  - Updates `MHC_ERROR`: Status='CLEARED', Clear_Date
  - Moves to `MHC_ERROR_HISTORY`

---

## Related Documents

- [Workflow Index](00_Workflow_Index.md)
- [Error Handling Patterns](../02_Coding_Standards/04_Error_Handling_Patterns.md)
- [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Error Handling | [Error Handling Patterns](../02_Coding_Standards/04_Error_Handling_Patterns.md) | Patterns |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Error Solutions |
| Error Table | [MHC_ERROR](../04_Database_Reference/01_Core_Tables/MHC_ERROR.md) | Schema |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

