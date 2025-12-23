# p_si_AuroHost (Auro Host Integration)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

The Auro Host Integration process (`p_si_AuroHost`) handles Auro-specific host communication protocol, processing inbound and outbound messages from the host system (WCS/MES).

---

## Purpose

Manages Auro-specific host communication protocol, processes host messages, creates move records, and sends completion notifications.

---

## Location

- **Source:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\simu\p_si_AuroHost\p_si_AuroHost.cpp`
- **Executable:** `D:\Auro\Exec\p_si_AuroHost.exe`

---

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| Database | Database | MHC_HOST_IN, MHC_HOST_OUT, MHC_MOVS tables |
| Host Protocol | Protocol Library | Auro-specific host protocol |

---

## Key Functions

### ProcessHostIn
**Purpose:** Processes inbound host messages
**Action:** Creates MHC_MOVS records, processes store/retrieve requests

### ProcessHostOut
**Purpose:** Processes outbound host messages
**Action:** Sends completion notifications to host

---

## Database Tables Accessed

| Table | Operation | Purpose |
|-------|-----------|---------|
| MHC_HOST_IN | INSERT, UPDATE | Inbound messages |
| MHC_HOST_OUT | INSERT, UPDATE | Outbound messages |
| MHC_MOVS | INSERT | Create move records |
| MHC_LOAD | INSERT | Create load records |
| MHC_INVENTORY | INSERT | Create inventory records |

---

## Shared Memory Usage

| Memory Block | Read/Write | Purpose |
|--------------|------------|---------|
| Process Table | R/W | Update heartbeat |

---

## Auro-Specific Features

- Custom message format
- Auro-specific message processing logic
- Site-specific host integration

---

## Related Documents

- [Code Reference Index](../00_Code_Index.md)
- [Auro Specific Overview](../../08_Project_Specific/00_Auro_Specific_Overview.md)
- [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Auro Features | [Auro Specific Overview](../../08_Project_Specific/00_Auro_Specific_Overview.md) | Custom Features |
| Host Communication | [Host Communication Flow](../../01_System_Architecture/02_Data_Flow_Diagrams.md) | Host Flow |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

