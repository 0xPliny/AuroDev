# Workflow Index

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides an index of all critical workflows documented in the Auro System.

---

## Workflow Documents

### 1. Inbound Move (Store Operation)
- **Document:** [01_Inbound_Move.md](01_Inbound_Move.md)
- **Purpose:** Complete flow of a store operation from host request to storage completion
- **Key Processes:** p_si_AuroHost, p_ar_fndwk, p_ar_stkdp, p_cc_stkcmx, p_ar_srcmp

### 2. Outbound Move (Retrieve Operation)
- **Document:** [02_Outbound_Move.md](02_Outbound_Move.md)
- **Purpose:** Complete flow of a retrieve operation from host request to load delivery
- **Key Processes:** p_si_AuroHost, p_ar_fndwk, p_ar_stkdp, p_cc_stkcmx, p_ar_srcmp, p_ar_movdp

### 3. Equipment Fault Handling
- **Document:** [03_Equipment_Fault_Handling.md](03_Equipment_Fault_Handling.md)
- **Purpose:** Error detection, logging, notification, and recovery procedures
- **Key Processes:** p_cc_stkcmx, p_sy_errdev, p_sy_erlog, p_sy_emailer

### 4. System Startup/Shutdown
- **Document:** [04_System_Startup_Shutdown.md](04_System_Startup_Shutdown.md)
- **Purpose:** System initialization and shutdown procedures
- **Key Processes:** p_sy_gcini, p_sy_dbini, all background processes

### 5. Manual Intervention
- **Document:** [05_Manual_Intervention.md](05_Manual_Intervention.md)
- **Purpose:** Operator actions for problem resolution and manual operations
- **Key Dialogs:** frm_Move_Maintenance, frm_MEI_Recovery, frm_Sttn_Recovery

---

## Related Documents

- [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
- [Data Flow Diagrams](../01_System_Architecture/02_Data_Flow_Diagrams.md)
- [Process Startup Sequence](../01_System_Architecture/04_Process_Startup_Sequence.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Inbound Move | [01_Inbound_Move.md](01_Inbound_Move.md) | Complete Flow |
| Outbound Move | [02_Outbound_Move.md](02_Outbound_Move.md) | Complete Flow |
| Error Handling | [03_Equipment_Fault_Handling.md](03_Equipment_Fault_Handling.md) | Error Flow |
| Startup | [04_System_Startup_Shutdown.md](04_System_Startup_Shutdown.md) | Startup Steps |
| Manual Operations | [05_Manual_Intervention.md](05_Manual_Intervention.md) | Operator Actions |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

