# Database Overview

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This document provides an overview of the Auro System database schema, including all tables, views, and their relationships.

---

## Database Information

- **Database Type:** Microsoft SQL Server Standard Edition
- **Database Name:** AuroDev (development), Auro (production)
- **Collation:** SQL_Latin1_General_CP1_CI_AS
- **Schema Definition:** `D:\Auro\File\database_tables.xml`

---

## Table Categories

### Core Tables (15+)
- `MHC_MOVS` - Move records
- `MHC_LOAD` - Load records
- `MHC_LOCATION` - Location records
- `MHC_INVENTORY` - Inventory records
- `MHC_ORDER` - Order records
- `MHC_ORDER_LINE` - Order line items
- `MHC_PALLET` - Pallet records
- Status and type lookup tables

### Equipment Tables (10+)
- `MHC_SM2DB_STACKER` - Stacker state (22 cranes)
- `MHC_SM2DB_STATION` - Station state
- `MHC_SM2DB_COMMS` - Communication state
- `MHC_SM2DB_ELT` - Equipment list
- `MHC_SM2DB_PROCESS` - Process table
- `MHC_SM2DB_QUEUES` - Queue table
- `MHC_SM2DB_SEMAPHORES` - Semaphore table
- `MHC_SM2DB_SYS` - System control

### Error Tables (8+)
- `MHC_ERROR` - Current errors
- `MHC_ERROR_HISTORY` - Error history
- `MHC_ERROR_QUEUE` - Error queue
- `MHC_ERROR_DESCRIPTION` - Error descriptions
- `MHC_ERROR_MESSAGE` - Error messages
- `MHC_ERROR_DETAILS` - Error details
- `MHC_ERROR_DISPLAY` - Error display
- `MHC_ERROR_USERS` - Error user assignments

### Security Tables (6+)
- `MHC_SEC_USERS` - Users
- `MHC_SEC_GROUP` - Groups
- `MHC_SEC_USER_GROUP` - User-group assignments
- `MHC_SEC_MENU_ITEM` - Menu items/dialogs
- `MHC_SEC_OBJECT_ACTIONS` - Object actions
- `MHC_SEC_USER_PERMISSIONS` - User permissions

### Statistics Tables (3+)
- `MHC_STATISTICS` - Statistics data
- `MHC_STATISTICS_CONTROL` - Statistics control

### Log Tables (15+)
- `MHC_MOVS_LOG` - Move history
- `MHC_LOAD_LOG` - Load history
- `MHC_INVENTORY_LOG` - Inventory history
- `MHC_LOCATION_LOG` - Location history
- `MHC_ORDER_LOG` - Order history
- `MHC_PALLET_LOG` - Pallet history
- `MHC_ERROR_QUEUE_LOG` - Error queue history
- `MHC_ELEMENTS_LOG` - Elements history
- Additional log tables for other entities

### Views (20+)
- `VIEW_LOAD_INVT` - Load and inventory
- `VIEW_LOCN_LOAD` - Location and load
- `VIEW_MOV_LOAD_INVT` - Move, load, and inventory
- `VIEW_RACK_INQ` - Rack inquiry
- `VIEW_RACKLOCN` - Rack location
- `VIEW_SEC_MENU` - Security menu
- `VIEW_SEC_PERM` - Security permissions
- Additional views for reporting

---

## Key Relationships

### Move → Load → Inventory → Location
- `MHC_MOVS` references `MHC_LOAD` via `Load_ID`
- `MHC_LOAD` has many `MHC_INVENTORY` records
- `MHC_INVENTORY` references `MHC_LOCATION` via `Location`
- `MHC_MOVS` references `MHC_LOCATION` via `Current_Location` and `Next_Location`

### Order → Pallet → Load
- `MHC_ORDER` has many `MHC_ORDER_LINE` records
- `MHC_ORDER` has many `MHC_PALLET` records
- `MHC_PALLET` has many `MHC_LOAD` records

### Error Relationships
- `MHC_ERROR` references `MHC_Device` via `Device`
- `MHC_ERROR` has many `MHC_ERROR_HISTORY` records
- `MHC_ERROR` references `MHC_ERROR_DESCRIPTION` via `Error_Code`

---

## Table Documentation

### Core Tables
- [MHC_MOVS](01_Core_Tables/MHC_MOVS.md)
- [MHC_LOAD](01_Core_Tables/MHC_LOAD.md)
- [MHC_LOCATION](01_Core_Tables/MHC_LOCATION.md)
- [MHC_INVENTORY](01_Core_Tables/MHC_INVENTORY.md)
- [MHC_ORDER](01_Core_Tables/MHC_ORDER.md)
- [MHC_PALLET](01_Core_Tables/MHC_PALLET.md)

### Equipment Tables
- [MHC_SM2DB_STACKER](02_Equipment_Tables/MHC_SM2DB_STACKER.md)
- [MHC_SM2DB_STATION](02_Equipment_Tables/MHC_SM2DB_STATION.md)
- [MHC_SM2DB_COMMS](02_Equipment_Tables/MHC_SM2DB_COMMS.md)

### Log Tables
- [MHC_MOVS_LOG](03_Log_Tables/MHC_MOVS_LOG.md)
- [MHC_LOAD_LOG](03_Log_Tables/MHC_LOAD_LOG.md)
- [MHC_INVENTORY_LOG](03_Log_Tables/MHC_INVENTORY_LOG.md)

---

## Related Documents

- [Database ERD](../01_System_Architecture/03_Database_ERD.md)
- [Database Access Patterns](../02_Coding_Standards/03_Database_Access_Patterns.md)
- [MHC-Developers-Guide Database](https://github.com/0xPliny/MHC-Developers-Guide/tree/main/04_Database_Reference)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| ERD | [Database ERD](../01_System_Architecture/03_Database_ERD.md) | Entity Relationships |
| Access Patterns | [Database Access Patterns](../02_Coding_Standards/03_Database_Access_Patterns.md) | OSUB Usage |
| Workflows | [Workflow Index](../05_Workflows/00_Workflow_Index.md) | Database Usage |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

