# Architecture Overview

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides a high-level overview of the Auro System architecture, including major components, their relationships, and how data flows through the system.

---

## System Architecture Layers

The Auro System follows a layered architecture pattern:

```
┌─────────────────────────────────────────────────────────────┐
│                    USER INTERFACE LAYER                       │
│              mhcMenu.exe (VB.NET Windows Forms)               │
│              100+ dialogs for operations and inquiries         │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Uses
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                  INTEROPERABILITY LAYER                      │
│              CInterop.dll (VB.NET ↔ C++ Bridge)             │
│              MHC Interop (Managed/Unmanaged Marshalling)     │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Uses
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                  DATABASE ACCESS LAYER                       │
│              OSUB API (Generated Database Routines)          │
│              z_*.vb files (z_movs, z_load, z_invt, etc.)    │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Uses
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                  MAPPED MEMORY LAYER                         │
│              Shared Memory Files (.shm)                      │
│              Equipment state, process tables, queues         │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Accessed by
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              BACKGROUND PROCESSES LAYER                      │
│  Dispatchers │ Completers │ Communications │ System Services│
└─────────────────────────────────────────────────────────────┘
                            │
                            │ Communicates with
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    EQUIPMENT LAYER                           │
│  22 Stacker Cranes │ 4 PLCs │ AGVs │ RTNX │ Conveyors       │
└─────────────────────────────────────────────────────────────┘
```

---

## Major Components

### 1. User Interface Layer

**Component:** `mhcMenu.exe` (VB.NET Windows Forms Application)

- **Purpose:** Provides operator interface for system monitoring, control, and maintenance
- **Components:**
  - 100+ dialogs (`frm_*.vb`)
  - Security dialogs (`frmPassword`, `frmUserGroupM`)
  - Inquiry dialogs (`frm_Inventory_Inquiry`, `frm_MoveInq`)
  - Maintenance dialogs (`frm_Move_Maintenance`, `frm_Stand_Maintenance`)
  - System monitoring (`frm_SysMon`, `frm_Stats`)

- **Access Methods:**
  - Direct database access via OSUB routines
  - Mapped memory access via CInterop
  - Real-time equipment status display

### 2. Database Layer

**Component:** Microsoft SQL Server Standard Edition

- **Purpose:** Persistent storage for all system data
- **Key Tables:**
  - `MHC_MOVS` - Move records
  - `MHC_LOAD` - Load records
  - `MHC_LOCATION` - Location records
  - `MHC_INVENTORY` - Inventory records
  - `MHC_ORDER` - Order records
  - `MHC_ERROR` - Error records
  - 60+ additional tables

- **Access Methods:**
  - OSUB API (generated from `database_tables.xml`)
  - Stored procedures
  - Direct SQL queries (for complex operations)

### 3. Mapped Memory Layer

**Component:** Shared Memory Files (`.shm`)

- **Purpose:** High-performance inter-process communication and real-time data sharing
- **Contents:**
  - Equipment state (stackers, stations, PLCs)
  - Process tables
  - Message queues
  - System control structures

- **Location:** `D:\Auro\shmf\`
- **Access:** Multiple processes read/write concurrently
- **Persistence:** Flushed to disk periodically

### 4. Background Processes Layer

#### Dispatchers
- `p_ar_fndwk` - Finds queued moves, updates to pending
- `p_ar_movdp` - Dispatches station-to-station moves
- `p_ar_stkdp` - Dispatches work to 22 stacker cranes
- `p_ar_agvdp` - Dispatches work to AGVs
- `p_ar_rtndp` - Dispatches work to RTNX vehicles

#### Completers
- `p_ar_srcmp` - Monitors stacker completions
- `p_ar_agvcp` - Monitors AGV completions
- `p_ar_rtnxcp` - Monitors RTNX completions

#### Communication Services
- `p_cc_stkcmx` - Stacker communications (MUDP/MASPA/CRC)
- `p_cc_mudpcm` - MUDP communications (AGV/RTNX)
- `Kepware_PLC` - PLC communications via OPC

#### System Services
- `p_sy_dbini` - Database initialization
- `p_sy_stats` - Statistics collection
- `p_sy_emailer` - Email notifications
- `p_sy_erlog` - Error logging
- `p_sy_watch` - Process monitoring

#### Auro-Specific Services
- `p_si_AuroHost` - Auro host integration
- `p_si_AuroTrack` - Auro tracking integration
- `p_si_stkcmx` - Site-specific stacker communications

### 5. Equipment Layer

- **22 Stacker Cranes** (SR01-SR22)
- **4 PLCs** (PLC01-PLC04)
- **AGV/RTNX Vehicles**
- **Conveyor Systems**

---

## Data Flow Patterns

### Inbound Move (Store Operation)

```
Host System
    ↓ (Store Request)
p_si_AuroHost (Host Interface)
    ↓ (Creates MHC_MOVS record)
Database (MHC_MOVS)
    ↓ (Status: QUEUED)
p_ar_fndwk (Find Work)
    ↓ (Status: PENDING)
p_ar_stkdp (Stacker Dispatcher)
    ↓ (Assigns to crane, writes to mapped memory)
Mapped Memory (Stacker Schedule)
    ↓ (Reads schedule)
p_cc_stkcmx (Stacker Communications)
    ↓ (Sends command)
Stacker Crane (SR01-SR22)
    ↓ (Executes store, reports completion)
p_cc_stkcmx (Stacker Communications)
    ↓ (Updates mapped memory)
p_ar_srcmp (Stacker Completer)
    ↓ (Updates MHC_MOVS, MHC_INVENTORY, MHC_LOCATION)
Database (Move Complete)
```

### Outbound Move (Retrieve Operation)

```
Host System
    ↓ (Retrieve Request)
p_si_AuroHost (Host Interface)
    ↓ (Creates MHC_MOVS record)
Database (MHC_MOVS)
    ↓ (Status: QUEUED)
p_ar_fndwk (Find Work)
    ↓ (Status: PENDING)
p_ar_stkdp (Stacker Dispatcher)
    ↓ (Assigns to crane, writes to mapped memory)
Mapped Memory (Stacker Schedule)
    ↓ (Reads schedule)
p_cc_stkcmx (Stacker Communications)
    ↓ (Sends command)
Stacker Crane (SR01-SR22)
    ↓ (Executes retrieve, reports completion)
p_cc_stkcmx (Stacker Communications)
    ↓ (Updates mapped memory)
p_ar_srcmp (Stacker Completer)
    ↓ (Updates MHC_MOVS, MHC_INVENTORY, MHC_LOCATION)
Database (Move Complete)
```

---

## Communication Protocols

### MUDP (Multi-User Data Protocol)
- Used for: AGV and RTNX vehicle communication
- Process: `p_cc_mudpcm`
- Features: Multi-cycle operations, load transfers

### MASPA/CRC
- Used for: Stacker crane communication
- Process: `p_cc_stkcmx`
- Features: Store/retrieve operations, error handling

### OPC (Kepware)
- Used for: PLC communication
- Process: `Kepware_PLC.exe`
- Features: Tag-based communication, multiple PLC brands

---

## Inter-Process Communication

### Mapped Memory
- **Purpose:** Fast data sharing between processes
- **Access:** Multiple processes read/write concurrently
- **Synchronization:** Mutexes and semaphores for thread safety

### Message Queues
- **Purpose:** Asynchronous message passing
- **Implementation:** Within mapped memory files
- **Usage:** Commands, status updates, event notifications

### Database
- **Purpose:** Persistent data storage and coordination
- **Access:** Via OSUB API or direct SQL
- **Transactions:** ACID-compliant operations

---

## Related Documents

- [Component Dependency Map](01_Component_Dependency_Map.md)
- [Data Flow Diagrams](02_Data_Flow_Diagrams.md)
- [Database ERD](03_Database_ERD.md)
- [Process Startup Sequence](04_Process_Startup_Sequence.md)
- [IPC and Shared Memory](05_IPC_and_Shared_Memory.md)
- [MHC-Developers-Guide Architecture](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/01_System_Architecture) - Generic MHC architecture patterns

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Component Dependencies | [Component Dependency Map](01_Component_Dependency_Map.md) | Dependency Graph |
| Data Flow Details | [Data Flow Diagrams](02_Data_Flow_Diagrams.md) | Flow Diagrams |
| Database Schema | [Database ERD](03_Database_ERD.md) | Entity Relationships |
| Startup Process | [Process Startup Sequence](04_Process_Startup_Sequence.md) | Startup Order |
| Shared Memory | [IPC and Shared Memory](05_IPC_and_Shared_Memory.md) | Memory Structure |
| Background Processes | [Code Reference](../03_Code_Reference/01_Background_Processes/) | Process List |
| Database Tables | [Database Overview](../04_Database_Reference/00_Database_Overview.md) | Table List |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

