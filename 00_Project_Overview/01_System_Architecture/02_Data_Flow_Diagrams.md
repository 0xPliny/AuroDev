# Data Flow Diagrams

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides detailed data flow diagrams for critical operations in the Auro System, showing how data moves through processes, databases, and mapped memory.

---

## Inbound Move (Store Operation) Flow

```mermaid
sequenceDiagram
    participant Host as Host System<br/>(WCS/MES)
    participant HostIn as p_si_AuroHost<br/>(Host Interface)
    participant DB as Database<br/>(MHC_MOVS)
    participant FindWork as p_ar_fndwk<br/>(Find Work)
    participant StkDp as p_ar_stkdp<br/>(Stacker Dispatcher)
    participant SM as Mapped Memory<br/>(Stacker Schedule)
    participant StkCm as p_cc_stkcmx<br/>(Stacker Comm)
    participant Crane as Stacker Crane<br/>(SR##)
    participant Srcmp as p_ar_srcmp<br/>(Stacker Completer)
    participant HostOut as p_ar_hostcom<br/>(Host Out)

    Host->>HostIn: Store Request<br/>(Load ID, Item, Qty)
    HostIn->>DB: INSERT MHC_MOVS<br/>(Status: QUEUED)
    FindWork->>DB: SELECT QUEUED moves
    FindWork->>DB: UPDATE Status = PENDING
    FindWork->>StkDp: Wake dispatcher
    StkDp->>DB: SELECT PENDING store moves
    StkDp->>DB: SELECT available location
    StkDp->>DB: UPDATE MHC_MOVS<br/>(Status: SCHEDULED,<br/>Next_Location, Schedule)
    StkDp->>SM: Write command to queue<br/>(Schedule = SR##)
    StkCm->>SM: Read command from queue
    StkCm->>SM: UPDATE Schedule = BUSY
    StkCm->>Crane: Send STORE command<br/>(MUDP/MASPA/CRC)
    Crane->>Crane: Execute store operation
    Crane->>StkCm: Completion message
    StkCm->>SM: UPDATE Schedule = NOT_BUSY
    StkCm->>Srcmp: Notify completion
    Srcmp->>DB: UPDATE MHC_MOVS<br/>(Status: COMPLETE)
    Srcmp->>DB: UPDATE MHC_INVENTORY<br/>(Location)
    Srcmp->>DB: UPDATE MHC_LOCATION<br/>(Load_ID, Status)
    HostOut->>DB: SELECT completed moves
    HostOut->>Host: Send completion message
```

---

## Outbound Move (Retrieve Operation) Flow

```mermaid
sequenceDiagram
    participant Host as Host System<br/>(WCS/MES)
    participant HostIn as p_si_AuroHost<br/>(Host Interface)
    participant DB as Database<br/>(MHC_MOVS, MHC_INVENTORY)
    participant FindWork as p_ar_fndwk<br/>(Find Work)
    participant StkDp as p_ar_stkdp<br/>(Stacker Dispatcher)
    participant SM as Mapped Memory<br/>(Stacker Schedule)
    participant StkCm as p_cc_stkcmx<br/>(Stacker Comm)
    participant Crane as Stacker Crane<br/>(SR##)
    participant Srcmp as p_ar_srcmp<br/>(Stacker Completer)
    participant MovDp as p_ar_movdp<br/>(Move Dispatcher)
    participant Station as Station<br/>(Conveyor)

    Host->>HostIn: Retrieve Request<br/>(Load ID, Destination)
    HostIn->>DB: SELECT MHC_INVENTORY<br/>(Find load location)
    HostIn->>DB: INSERT MHC_MOVS<br/>(Status: QUEUED,<br/>Current_Location, Next_Location)
    FindWork->>DB: SELECT QUEUED moves
    FindWork->>DB: UPDATE Status = PENDING
    FindWork->>StkDp: Wake dispatcher
    StkDp->>DB: SELECT PENDING retrieve moves
    StkDp->>DB: UPDATE MHC_MOVS<br/>(Status: SCHEDULED,<br/>Schedule = SR##)
    StkDp->>SM: Write command to queue
    StkCm->>SM: Read command from queue
    StkCm->>Crane: Send RETRIEVE command
    Crane->>Crane: Execute retrieve operation
    Crane->>StkCm: Completion message
    StkCm->>Srcmp: Notify completion
    Srcmp->>DB: UPDATE MHC_MOVS<br/>(Status: COMPLETE)
    Srcmp->>DB: UPDATE MHC_INVENTORY<br/>(Location = Station)
    Srcmp->>DB: UPDATE MHC_LOCATION<br/>(Load_ID = NULL)
    MovDp->>DB: SELECT completed retrieves<br/>at station
    MovDp->>SM: Write station move command
    MovDp->>Station: Activate conveyor
    Station->>MovDp: Load moved to destination
```

---

## Error Handling Flow

```mermaid
sequenceDiagram
    participant Equipment as Equipment<br/>(Stacker/PLC/AGV)
    participant Comm as Communication<br/>(p_cc_stkcmx/Kepware)
    participant SM as Mapped Memory<br/>(Error Flags)
    participant ErrDev as p_sy_errdev<br/>(Error Device)
    participant DB as Database<br/>(MHC_ERROR)
    participant ErLog as p_sy_erlog<br/>(Error Logging)
    participant Emailer as p_sy_emailer<br/>(Emailer)
    participant UI as mhcMenu.exe<br/>(UI Dialog)

    Equipment->>Comm: Error detected<br/>(Error Code)
    Comm->>SM: SET Error Flag = TRUE<br/>SET Error Code
    ErrDev->>SM: Monitor error flags
    ErrDev->>DB: INSERT MHC_ERROR<br/>(Error Code, Device, Timestamp)
    ErrDev->>ErLog: Log error event
    ErLog->>DB: INSERT MHC_ERROR_HISTORY
    Emailer->>DB: SELECT error subscriptions
    Emailer->>Emailer: Send email notification
    UI->>DB: SELECT MHC_ERROR<br/>(Display errors)
    UI->>UI: Show error dialog<br/>(frm_ErrorsMini)
    UI->>DB: UPDATE MHC_ERROR<br/>(Acknowledged by user)
    Equipment->>Comm: Error cleared
    Comm->>SM: SET Error Flag = FALSE
    ErrDev->>DB: UPDATE MHC_ERROR<br/>(Status: CLEARED)
```

---

## Host Communication Flow

```mermaid
sequenceDiagram
    participant Host as Host System<br/>(WCS/MES)
    participant HostIn as p_si_AuroHost<br/>(Host In)
    participant DB as Database<br/>(MHC_HOST_IN)
    participant HostCom as p_cc_hostcom<br/>(Host Communications)
    participant Process as Background Process<br/>(Dispatcher/Completer)
    participant HostOut as p_ar_hostcom<br/>(Host Out)
    participant DBOut as Database<br/>(MHC_HOST_OUT)

    Host->>HostIn: Inbound message<br/>(Store/Retrieve/Order)
    HostIn->>DB: INSERT MHC_HOST_IN<br/>(Message, Status: RECEIVED)
    HostIn->>DB: Process message<br/>(Create MHC_MOVS/MHC_ORDER)
    HostCom->>DB: SELECT MHC_HOST_IN<br/>(Status: RECEIVED)
    HostCom->>Process: Trigger processing
    Process->>DB: Update move/order status
    Process->>DBOut: INSERT MHC_HOST_OUT<br/>(Completion message)
    HostOut->>DBOut: SELECT MHC_HOST_OUT<br/>(Status: PENDING)
    HostOut->>Host: Send outbound message
    HostOut->>DBOut: UPDATE Status = SENT
```

---

## Statistics Collection Flow

```mermaid
sequenceDiagram
    participant Process as Background Process<br/>(Dispatcher/Completer)
    participant SM as Mapped Memory<br/>(Statistics Counters)
    participant Stats as p_sy_stats<br/>(Statistics Service)
    participant DB as Database<br/>(MHC_STATISTICS)
    participant Rollup as SQL Job<br/>(Roll-up Procedure)
    participant DBHourly as Database<br/>(STATISTICS_HR)
    participant DBDaily as Database<br/>(STATISTICS_DAY)

    Process->>SM: Increment counter<br/>(CountStores++, CountRetrieves++)
    Stats->>SM: Read statistics<br/>(Every 30 minutes)
    Stats->>DB: INSERT MHC_STATISTICS<br/>(Half-hourly data)
    Rollup->>DB: SELECT half-hourly data
    Rollup->>DBHourly: INSERT aggregated<br/>(Hourly roll-up)
    Rollup->>DBHourly: SELECT hourly data
    Rollup->>DBDaily: INSERT aggregated<br/>(Daily roll-up)
```

---

## Related Documents

- [Architecture Overview](00_Architecture_Overview.md)
- [Component Dependency Map](01_Component_Dependency_Map.md)
- [Process Startup Sequence](04_Process_Startup_Sequence.md)
- [Inbound Move Workflow](../05_Workflows/01_Inbound_Move.md)
- [Outbound Move Workflow](../05_Workflows/02_Outbound_Move.md)
- [Equipment Fault Handling](../05_Workflows/03_Equipment_Fault_Handling.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Inbound Move Details | [Inbound Move Workflow](../05_Workflows/01_Inbound_Move.md) | Step-by-Step |
| Outbound Move Details | [Outbound Move Workflow](../05_Workflows/02_Outbound_Move.md) | Step-by-Step |
| Error Handling Details | [Equipment Fault Handling](../05_Workflows/03_Equipment_Fault_Handling.md) | Error Flow |
| Process Details | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process Documentation |
| Database Tables | [Database Reference](../04_Database_Reference/00_Database_Overview.md) | Table Documentation |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

