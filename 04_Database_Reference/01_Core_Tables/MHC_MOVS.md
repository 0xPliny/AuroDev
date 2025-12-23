# MHC_MOVS

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

The `MHC_MOVS` table stores all move records in the Auro System. A move represents a load movement operation (store, retrieve, or transfer) from one location to another.

---

## Purpose

Tracks load movement requests and their execution status throughout the system, from initial request through completion.

---

## Schema

| Column | Type | Nullable | Default | Description |
|--------|------|----------|---------|-------------|
| Move_ID | int | No | IDENTITY | Primary key, auto-increment |
| Load_ID | varchar(50) | No | | Foreign key to MHC_LOAD |
| Move_Type | varchar(20) | No | | STORE, RETRIEVE, TRANSFER |
| Move_Status | varchar(20) | No | | QUEUED, PENDING, SCHEDULED, BUSY, COMPLETE, ERROR |
| Current_Location | varchar(50) | No | | Current location of load |
| Next_Location | varchar(50) | Yes | NULL | Target location for move |
| Schedule | varchar(20) | Yes | NULL | Equipment assigned (SR##, AGV##, etc.) |
| Scheduled_Date | datetime | Yes | NULL | When move was scheduled |
| Complete_Date | datetime | Yes | NULL | When move completed |
| Move_Current_Action | varchar(20) | Yes | NULL | Current action being performed |
| Add_User | varchar(50) | No | | User who created move |
| Add_Date | datetime | No | GETDATE() | When move was created |
| Modify_User | varchar(50) | Yes | NULL | User who last modified |
| Modify_Date | datetime | Yes | NULL | Last modification date |

---

## Primary Key

- `Move_ID` (int, IDENTITY)

---

## Foreign Keys

| Column | References | On Delete |
|--------|------------|-----------|
| Load_ID | MHC_LOAD.Load_ID | RESTRICT |
| Current_Location | MHC_LOCATION.Location | RESTRICT |
| Next_Location | MHC_LOCATION.Location | RESTRICT |
| Move_Type | MHC_MOVS_TYPE.Move_Type | RESTRICT |
| Move_Status | MHC_MOVS_STATUS.Move_Status | RESTRICT |
| Move_Current_Action | MHC_MOVS_ACTION.Move_Current_Action | RESTRICT |

---

## Indexes

| Index Name | Columns | Type |
|------------|---------|------|
| PK_MHC_MOVS | Move_ID | Clustered |
| IX_MHC_MOVS_Load_ID | Load_ID | Non-clustered |
| IX_MHC_MOVS_Status | Move_Status | Non-clustered |
| IX_MHC_MOVS_Schedule | Schedule | Non-clustered |
| IX_MHC_MOVS_Current_Location | Current_Location | Non-clustered |

---

## Common Queries

### Select Pending Moves
```sql
SELECT * FROM MHC_MOVS 
WHERE Move_Status = 'PENDING' 
ORDER BY Scheduled_Date
```

### Select Moves for Stacker
```sql
SELECT * FROM MHC_MOVS 
WHERE Schedule = 'SR01' 
AND Move_Status IN ('SCHEDULED', 'BUSY')
```

### Select Completed Moves
```sql
SELECT * FROM MHC_MOVS 
WHERE Move_Status = 'COMPLETE' 
AND Complete_Date >= DATEADD(day, -1, GETDATE())
```

---

## Related Tables

- **MHC_LOAD** - Load information
- **MHC_LOCATION** - Location information
- **MHC_INVENTORY** - Inventory records
- **MHC_MOVS_LOG** - Move history log
- **MHC_MOVS_STATUS** - Status lookup
- **MHC_MOVS_TYPE** - Type lookup
- **MHC_MOVS_ACTION** - Action lookup

---

## Usage in Processes

- **p_ar_fndwk** - Finds QUEUED moves, updates to PENDING
- **p_ar_stkdp** - Assigns moves to stackers, updates Schedule
- **p_ar_movdp** - Processes station moves
- **p_ar_srcmp** - Updates moves to COMPLETE on stacker completion
- **p_ar_agvcp** - Updates moves to COMPLETE on AGV completion
- **p_ar_rtnxcp** - Updates moves to COMPLETE on RTNX completion

---

## Related Documents

- [Database Overview](../00_Database_Overview.md)
- [MHC_LOAD](MHC_LOAD.md)
- [MHC_LOCATION](MHC_LOCATION.md)
- [MHC_MOVS_LOG](../03_Log_Tables/MHC_MOVS_LOG.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Load Table | [MHC_LOAD](MHC_LOAD.md) | Schema |
| Location Table | [MHC_LOCATION](MHC_LOCATION.md) | Schema |
| Move Workflow | [Inbound Move Workflow](../../05_Workflows/01_Inbound_Move.md) | Move Flow |
| Move Dispatcher | [p_ar_stkdp](../../03_Code_Reference/01_Background_Processes/p_ar_stkdp.md) | Process |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

