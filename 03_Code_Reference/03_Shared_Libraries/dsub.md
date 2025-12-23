# DSUB Library - Database Subroutines

**Document Version:** 2.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Overview

DSUB (Database Subroutines) is the **core database access layer** for the MHC/MEM system. It provides a unified C++ interface for SQL Server database operations, including connection management, transaction control, CRUD operations, and specialized business logic functions.

**Location:** `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\`

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                      Application Layer                          │
│         (Background Processes, Dispatchers, Completers)         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                         DSUB Layer                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │  ds_sql.*   │  │ ds_get_*.*  │  │  Business Logic         │  │
│  │  (Core SQL) │  │  (Getters)  │  │  (ds_movs, ds_group...) │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    ADO Connection Layer                         │
│                  (SQL Server via COM/ADO)                       │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              Microsoft SQL Server Standard Edition              │
└─────────────────────────────────────────────────────────────────┘
```

---

## Core Components

### 1. SQL Connection & Transaction Management (ds_sql.*)

The foundation of all database operations.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_sql.h` | Core SQL interface declarations and `SQL` class |
| `ds_sql_SQL.cpp` | SQL Server implementation |
| `ds_sql_SQL.h` | SQL Server-specific headers |
| `ds_sql_int.h` | Internal SQL interface |

#### SQL Class (`ds_sql_class`)

The global `SQL` object provides database operations:

```cpp
// Global SQL class instance
ds_sql_class SQL;

// Connection
SQL.connect();                    // Connect with default priority
SQL.connect(priority);            // Connect with deadlock priority
SQL.connect(priority, nocrash);   // Connect without crashing on error

// Transaction Control
SQL.start(SQL.READ_ONLY);         // Start read-only transaction
SQL.start(SQL.READ_WRITE);        // Start read-write transaction
SQL.commit();                     // Commit transaction
SQL.rollback();                   // Rollback transaction
SQL.rollback(true);               // Rollback with deadlock handling
SQL.close();                      // Close connection

// Utility Properties
SQL.Where[2048];                  // Where clause buffer
SQL.text[2048];                   // General text buffer
SQL.rtn;                          // Return status
SQL.count;                        // Count variable
SQL.db_update;                    // Update flag
```

#### Return Codes

| Code | Constant | Description |
|------|----------|-------------|
| 0 | `SQL.GOOD` | Success |
| -1 | `SQL.HELD` / `SQL.DBHELD` | Record locked by another process |
| -2 | `SQL.UGLY` / `SQL.NORecord` | Record not found |

#### Deadlock Priority System

DSUB implements a creative priority system for handling deadlock scenarios:

```cpp
SQL.PRIORITY.PEON     // -10 (lowest)
SQL.PRIORITY.LOW      // -5
SQL.PRIORITY.NORMAL   // 0 (default)
SQL.PRIORITY.HIGH     // 5
SQL.PRIORITY.GOBLIN   // 6
SQL.PRIORITY.ORC      // 7
SQL.PRIORITY.TROLL    // 8
SQL.PRIORITY.NAZGUL   // 9
SQL.PRIORITY.SAURON   // 10 (highest - wins all deadlocks)
```

#### SQL Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| 0 | `SQL_SUCCESS` | Operation successful |
| 1403 | `NO_DATA_FOUND` | No records returned |
| -1405 | `NULL_VALUE_RETURNED` | NULL value in result |
| -60 | `DEADLOCK` | Deadlock detected |
| -1002 | `OUTSEQUENCE` | Fetch out of sequence |
| -3114 | `NOTCONNECT` | Not connected to database |
| -28 | `KILLED_ME` | Session killed by DBA |

---

### 2. Move Operations (ds_movs.*)

Functions for managing move records and state transitions.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_movs.h` | Move record operations |
| `ds_movs.cpp` | Move state management |
| `ds_movs_counts.h/cpp` | Move counting functions |
| `ds_movs_over.h/cpp` | Move overflow handling |

#### Core Functions

```cpp
// Update move to next state
long ds_movs_set_next(movs *pMovs, char *pState, 
                      char* pWakeProgram = NULL, 
                      long WakeProgramSize = 0);

// Test for one-off conditions
long ds_movs_OneOff(movs *pMovs, cc_stand* pStd);
```

---

### 3. Location Operations (ds_get_locn.*, ds_locn_counts.*, ds_util_locn.*)

Functions for location management and querying.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_get_locn.h/cpp` | Location lookup functions |
| `ds_locn_counts.h/cpp` | Location counting by stacker |
| `ds_util_locn.h/cpp` | Location utility functions |

#### Core Functions

```cpp
// Find location in aisle
long find_in_aisle(LOAD* load, MOVS* movs);

// Get zone for move
long get_Zone(MOVS* movs, ZOLO* zolo, LOAD* load);

// Update SQL where clause for zone search
long get_Zolo_Search(ds_sql_class* SQL, ZOLO zolo);
```

#### Location Count Class

```cpp
class ds_locn_counts {
public:
    ds_locn_counts();                              // Default constructor
    long count(const bool trans, cc_stacker* SR);  // Count by stacker
    void count_all();                              // Count all locations
};

// Global instance
ds_locn_counts DS_LOCN_COUNT;
```

---

### 4. Stand Operations (ds_stand.*)

Functions for managing stands (stations, buffers).

#### Key Files
| File | Purpose |
|------|---------|
| `ds_stand.h` | Stand operation declarations |
| `ds_stand.cpp` | Stand operations implementation |

#### Core Functions

```cpp
// Actual count at stand
long ds_stand_actual(char* stand, bool tran);

// Enroute count to stand
long ds_stand_enroute(char* stand, const bool tran, const bool all_moves);
long ds_stand_enroute(char* stand, const bool tran, const bool all_moves, 
                      const char* loadid);
long ds_stand_enroute(char* stand, const bool tran, const bool all_moves, 
                      MOVS* movs);

// Check if enroute is OK (not exceeding max)
long ds_stand_enroute_ok(char* stand, const bool tran, const bool all_moves);
long ds_stand_enroute_ok(char* stand, const bool tran, const bool all_moves,
                         MOVS* movs, long* i_max_enroute, long* i_enroute, 
                         long* i_actual);

// Get max enroute for stand
long ds_stand_maxenroute(char* stand, bool tran);
long ds_stand_maxenroute(char* stand, long max, bool tran);

// Get next move at stand
long ds_stand_next(char* stand, bool tran, const char* loadid, 
                   const bool pending, const bool waitmode);

// Stand database management
void ds_stand_add_DB_stand(cc_stand* stP, const long max_enroute);
void ds_stand_delete_DB_stand();
void ds_stand_init_DB_stand();
```

---

### 5. Stacker Operations (ds_sr.*)

Functions for stacker crane (SR) database operations.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_sr.h` | Stacker database operations |
| `ds_sr.cpp` | Stacker operations implementation |

#### Core Functions

```cpp
// Check if enroute to stacker is OK
long ds_sr_enroute_ok(char* sr_name, const bool tran, const bool all_moves, 
                      MOVS* movs);
long ds_sr_enroute_ok(char* sr_name, const bool tran, const bool all_moves, 
                      MOVS* movs, long* i_max_enroute, long* i_enroute);

// Find PD input station for stacker
cc_stand* find_pdin(cc_stacker* SR, const char* stand);

// Count enroute to stacker
long ds_sr_enroute(char* sr_name, const bool tran, const bool all_moves, 
                   MOVS* movs);

// Stacker database management
void ds_sr_add_DB_stacker(cc_stacker* srP, long max_enroute);
void ds_sr_delete_DB_stacker();
void ds_sr_init_DB_stacker();
```

---

### 6. Routing Group Operations (ds_group.*)

Comprehensive class for managing routing groups and stand selection.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_group.h` | Group class definition |
| `ds_group.cpp` | Group operations implementation |

#### Group Class (`ds_group`)

```cpp
class ds_group {
public:
    // Search Direction
    class Search_Direction {
        enum search { None, Forward, Backward };
    } DIRECTION;
    
    // Overflow Method  
    class OverFlow_Method {
        enum overflow { Busy, Offline };
    } OVER_FLOW;
    
    // Search Mode
    class Search_Mode {
        enum modes { FIFO, Bay, Round, Next };
    } MODE;
    
    // Which Station
    class Which_Station {
        enum which_station { Final, Next };
    } WHICH;
    
    // Constructors
    ds_group();
    ds_group(const char* GRP_name);
    ds_group(const long aisle, const char* type);
    
    // Core Methods
    long Find_Stand(MOVS* movs, const bool busy, const bool off);
    void load();
    long get_group(GROUPS* groups, const long aisle, const long level, 
                   const bool do_trans);
    
    // Accessors
    long group_found();
    long group_Member_Count();
    long search_direction();
    void search_direction(const long value);
    long which_station();
    void which_station(const long value);
};

// Global instance
ds_group DS_GRP;
```

---

### 7. Load Creation (ds_create_load.*)

Functions for creating and managing loads.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_create_load.h` | Load creation declarations |
| `ds_create_load.cpp` | Load creation implementation |

#### Core Functions

```cpp
// Create a new load
long ds_create_load(char* loadID, char* loadType, char* loadStatus, 
                    long curAisle, char* currentStation, char* location,
                    char* historyComment, bool alreadyInTrans);

// Get load sub-type from load type
long ds_Get_Load_Sub_Type(char* loadType, char* loadSubType, 
                          bool alreadyInTrans);

// Get load size from sub-type
long ds_Get_Load_Size(char* loadSubType, long& loadSize, bool alreadyInTrans);
```

---

### 8. Host Communication (ds_hostout.*)

Functions for generating host outbound messages.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_hostout.h` | Host message definitions |
| `ds_hostout.cpp` | Host message generation |

#### Core Functions

```cpp
// Transfer report
long ds_hostTranReport(MOVS* myMov, long Priority = 0);

// XML creation
string CreateTranXML(MOVS* myMov);

// QUAD stacker report (4 moves)
long ds_hostTranReport_QUAD(char* pRepType, MOVS* mov1, MOVS* mov2, 
                            MOVS* mov3, MOVS* mov4, long Priority = 0);

// Stacker loaded report
long ds_hostStackerLoaded(MOVS* movs, long Priority = 0);

// Cancel report
long ds_hostCancelReport(map<string, string> HostInMessage, string strReason, 
                         string strStation, bool bACK);
```

#### Message Types

| Constant | Value | Description |
|----------|-------|-------------|
| `CLEQS-REP` | Cell Equipment Status Report | Equipment block status |
| `CLTRN-REP` | Cell Transfer Report | Transfer status |
| `CLTRN-REP-QUAD` | Cell Transfer QUAD | 4-move transfer |
| `CLPLT_REP` | Cell Pallet Report | Pallet request |
| `CLEXT-ANS` | Cell Extinguish Answer | Fire mode response |

#### Report Types (`REP_DIV`)

| Constant | Description |
|----------|-------------|
| `TRN_REQ` | Transfer Request |
| `TRN_LOAD` | Load Report |
| `TRN_CMP` | Complete Report |
| `TRN_CANCEL` | Cancel Report |
| `TRN_CHANGE_REQ` | Destination Change Request |
| `TRN_EMPTY_REQ` | Empty Request |
| `TRN_REMOVE` | Remove Report |
| `TRN_REQ_CANCEL` | Request Cancel Report |
| `TRN_EVENT` | Event Report |

---

### 9. Stored Procedure Execution (ds_exec.*)

Generic stored procedure calling interface.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_exec.h` | Stored procedure interface |
| `ds_exec.cpp` | Stored procedure execution |

#### Parameter Types

```cpp
static class types {
    enum prm_Types {
        END = 0,        // End of parameters
        IN_INT = 1,     // Input integer
        IN_LONG = 2,    // Input long
        IN_DOUBLE = 3,  // Input double
        IN_CHAR = 4,    // Input char
        OUT_INT = 5,    // Output integer
        OUT_LONG = 6,   // Output long
        OUT_DOUBLE = 7, // Output double
        OUT_CHAR = 8,   // Output char
        RETURN = 9      // Return value
    };
} TYPES;
```

#### Core Functions

```cpp
// Execute stored procedure with variadic parameters
long ds_exec_SP(char* spname, ...);

// Execute stored procedure with parameter array
long ds_exec_SP1(char* spname, spP** sp);

// Check if table exists
long ds_exec_TableExists(char* tableName);

// Log database locks
long ds_exec_LogDBLocks(long* rows, long* seq_num);
```

---

### 10. Getter Functions (ds_get_*.*)

Specialized functions for retrieving specific data.

| File | Purpose |
|------|---------|
| `ds_get_MoveId.h/cpp` | Generate unique move IDs |
| `ds_get_MoveNo.h/cpp` | Get move numbers |
| `ds_get_loadid.h/cpp` | Get load IDs |
| `ds_get_loadType.h/cpp` | Get load types |
| `ds_get_load_size.h/cpp` | Get load sizes |
| `ds_get_ldsubtype.h/cpp` | Get load subtypes |
| `ds_get_locn.h/cpp` | Get location data |
| `ds_get_order.h/cpp` | Get order data |
| `ds_get_Pallet.h/cpp` | Get pallet data |
| `ds_get_fifo.h/cpp` | FIFO retrieval |
| `ds_get_itemqty.h/cpp` | Get item quantities |
| `ds_get_Job_ID.h/cpp` | Get job IDs |
| `ds_get_moves.h/cpp` | Get move records |
| `ds_get_schd_number.h/cpp` | Get schedule numbers |
| `ds_get_serial_number.h/cpp` | Get serial numbers |
| `ds_get_store_Aisle.h/cpp` | Get storage aisles |

#### Example: Move ID Generation

```cpp
// Generate a new move ID
long ds_get_MoveId(char* pMoveId, size_t pSizeInBytes);

// Reset move ID sequence
long ds_reset_MoveId(long my_seq);
```

---

### 11. Calculation Functions (ds_calc.*)

Load size and height calculations.

#### Key Files
| File | Purpose |
|------|---------|
| `ds_calc.h` | Calculation declarations |
| `ds_calc.cpp` | Calculation implementation |

#### SIZE_VALUES Structure

```cpp
typedef struct size_values {
    double  Last_Update;           // Last update timestamp
    
    // Film and cardboard sizes
    char    FilmSiz[11];           // Protective film size (inches)
    char    CardBdSiz[11];         // Cardboard size (inches)
    char    JpnShipHi[11];         // Japan shipping height (inches)
    char    CntPalHi[11];          // Central warehouse height (inches)
    char    JmbCardBdSiz[11];      // Jumbo cardboard size (inches)
    int     BaseCardBdCnt;         // Extra cardboard count
    
    // Shipping #2 pallet
    char    Ship2Width[11];
    char    Ship2Length[11];
    char    Ship2MaxHeight[11];
    
    // Process pallet
    char    ProcessWidth[11];
    char    ProcessLength[11];
    char    ProcessShortMax[11];
    char    ProcessMediumMax[11];
    char    ProcessTallMax[11];
    
    // Jumbo pallet
    char    JumboWidth[11];
    char    JumboLength[11];
    char    JumboMaxHeight[11];
} SIZE_VALUES;
```

#### Core Functions

```cpp
// Load element values
long ds_calc_load();

// Calculate load height
double ds_calc_height(INVT* pInvt);

// Test load size
long ds_calc_test_size(INVT* pInvt, long* pSize);

// Get load size and height
long ds_calc_get_size(INVT* pInvt, long* pSize, double* pHeight);
```

---

### 12. AGV/RTNX Operations (ds_agv.*, ds_rtnx.*)

Database operations for AGV and RTNX vehicles.

| File | Purpose |
|------|---------|
| `ds_agv.h/cpp` | AGV database operations |
| `ds_rtnx.h/cpp` | RTNX database operations |

---

### 13. Utility Functions

| File | Purpose |
|------|---------|
| `ds_delete_help.h/cpp` | Delete helper functions |
| `ds_does_load_exist.h/cpp` | Load existence check |
| `ds_hoser.h/cpp` | Error handling utilities |
| `ds_EmailHelp.h/cpp` | Email notification helpers |
| `ds_vlosel_intersect.h/cpp` | View/location intersection |
| `ds_Check_QC_Hold.h/cpp` | QC hold checking |
| `ds_Clear_Pending.cpp` | Clear pending operations |
| `ds_Realloc.h/cpp` | Reallocation functions |
| `ds_Scan_MisMatch_Handle.h/cpp` | Scan mismatch handling |
| `ds_pallet_order_clean.h/cpp` | Pallet order cleanup |
| `ds_stats_info.h/cpp` | Statistics information |

---

## Usage Patterns

### Standard Database Transaction Pattern

```cpp
// Standard transaction pattern
SQL.start(SQL.READ_WRITE);

// Perform operations
int rc = record.select();
if (rc != GP.GOOD) {
    cs_log_printf(CS_LOG_ERROR, "DSUB: Select failed, rc=%d", rc);
    SQL.rollback();
    return GP.BAD;
}

// Update record
record.status = NEW_STATUS;
rc = record.update();
if (rc != GP.GOOD) {
    cs_log_printf(CS_LOG_ERROR, "DSUB: Update failed, rc=%d", rc);
    SQL.rollback();
    return GP.BAD;
}

SQL.commit();
return GP.GOOD;
```

### Read-Only Query Pattern

```cpp
SQL.start(SQL.READ_ONLY);

// Build where clause
sprintf(SQL.Where, "LOAD_ID = '%s'", loadId);

// Execute query
LOAD load;
int rc = TBL_select(&load, SQL.Where);

// No commit/rollback needed for read-only
```

### High-Priority Transaction

```cpp
// Connect with high priority for critical operations
SQL.connect(SQL.PRIORITY.SAURON);

SQL.start(SQL.READ_WRITE);
// Critical operations that must win deadlocks
SQL.commit();
```

---

## Dependencies

### Required Headers
```cpp
#include <global_prm.h>    // Global parameters
#include <cc_gg.h>         // Global variables
#include <cc_str.h>        // String operations
#include <cs_log.h>        // Logging
#include <ds_sql.h>        // SQL interface
```

### Database Table Headers (Generated)
```cpp
#include <movs.h>          // MHC_MOVS table
#include <load.h>          // MHC_LOAD table
#include <locn.h>          // MHC_LOCATION table
#include <invt.h>          // MHC_INVENTORY table
#include <stk.h>           // Stacker tables
#include <sttn.h>          // Station tables
#include <groups.h>        // Routing groups
#include <grpmem.h>        // Group members
```

---

## File Summary

| Category | Files | Description |
|----------|-------|-------------|
| **Core SQL** | 4 | Connection, transaction, error handling |
| **Move Operations** | 4 | Move state management |
| **Location Operations** | 6 | Location queries and counts |
| **Stand Operations** | 2 | Stand management |
| **Stacker Operations** | 2 | Stacker database ops |
| **Group Operations** | 2 | Routing group management |
| **Load Operations** | 2 | Load creation |
| **Host Communication** | 2 | Host message generation |
| **Stored Procedures** | 2 | SP execution interface |
| **Getters** | 30+ | Specialized data retrieval |
| **Calculations** | 2 | Size/height calculations |
| **Utilities** | 12+ | Helper functions |
| **Total** | ~60 files | Complete database layer |

---

## Related Documentation

- [OSUB Library](osub.md) - VB.NET database access (generated classes)
- [CCSUB Library](ccsub.md) - Control subroutines
- [CSUB Library](csub.md) - Common subroutines
- [Database Overview](../../04_Database_Reference/00_Database_Overview.md)
- [global_prm.h Reference](global_prm.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| VB Database Access | [OSUB Library](osub.md) | z_* classes |
| Transaction Management | [Database Patterns](../../02_Coding_Standards/03_Database_Access_Patterns.md) | Transactions |
| Move Processing | [p_ar_movdp](../01_Background_Processes/p_ar_movdp.md) | Move Dispatcher |
| Stacker Control | [p_ar_stkdp](../01_Background_Processes/p_ar_stkdp.md) | Stacker Dispatcher |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2024-12-23 | Comprehensive deep documentation |
| 1.0 | 2024-12-22 | Initial creation |
