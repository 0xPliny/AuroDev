# ds_invt - Inventory Database Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `ds_invt.cpp`, `ds_invt.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\dsub\` |
| **Module Name** | `ds_invt` |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `ds_invt` module provides **database functions and business logic** for inventory operations in the MHC system. It builds on top of the OSUB `os_invt` generated functions, adding business logic for item-level tracking within loads.

**Critical Operations:**
- Inventory item creation and validation
- Item-location relationship management
- Item status tracking
- Barcode/item code management
- Inventory querying with business rules

**Key Principle:** DSUB functions provide **business logic** on top of OSUB's **data access** functions.

---

## Key Functions

### ds_invt_create

**Signature:**
```cpp
long ds_invt_create(INVT* invt, const char* loadId, const char* itemCode, 
                    long sequenceNo, long quantity, const char* barcode)
```

**Description:**
Creates a new inventory record with business logic validation.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `invt` | `INVT*` | Output - Populated inventory record structure |
| `loadId` | `const char*` | Load identifier |
| `itemCode` | `const char*` | Item code |
| `sequenceNo` | `long` | Sequence number within load |
| `quantity` | `long` | Item quantity |
| `barcode` | `const char*` | Optional barcode/UPC |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - inventory record created |
| `-1` | `GP.BAD` | Failure - validation error or database error |

**Logic Flow:**
1. Validates input parameters
2. Checks if inventory record already exists
3. Initializes `INVT` structure using `INVT_init()`
4. Sets default values:
   - `status` = "ACTIVE"
   - `add_date` = current date/time
5. Copies input parameters to structure
6. Validates load exists using `LOAD_select_LOAD_PR_KEY()`
7. Inserts record using `INVT_insert()`
8. Logs operation

---

### ds_invt_update_location

**Signature:**
```cpp
long ds_invt_update_location(const char* loadId, const char* newLocation)
```

**Description:**
Updates inventory location when load moves (indirect update via load location).

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `loadId` | `const char*` | Load identifier |
| `newLocation` | `const char*` | New location |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - location updated |
| `-1` | `GP.BAD` | Failure - load not found or database error |

---

### ds_invt_get_by_load

**Signature:**
```cpp
long ds_invt_get_by_load(INVT* invt, const char* loadId, long* count)
```

**Description:**
Gets all inventory records for a specific load.

**Parameters:**
| Parameter | Type | Description |
|-----------|------|-------------|
| `invt` | `INVT*` | Output - First inventory record (use loop for all) |
| `loadId` | `const char*` | Load identifier |
| `count` | `long*` | Output - Number of inventory items found |

**Return Values:**
| Return Code | Constant | Description |
|-------------|----------|-------------|
| `0` | `GP.GOOD` | Success - inventory found |
| `-1` | `GP.BAD` | Failure - no inventory found |

---

## Common Usage Patterns

### Pattern 1: Create Inventory for Load

```cpp
INVT invt;
if (ds_invt_create(&invt, "L00001", "ITEM01", 1, 10, "1234567890123") == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Created inventory item for load %s", "L00001");
}
```

### Pattern 2: Get All Items in Load

```cpp
INVT invt;
long count;
if (ds_invt_get_by_load(&invt, "L00001", &count) == GP.GOOD) {
    cs_log_printf(FILELINE, CS_LOG_INFO, "Load %s has %ld items", "L00001", count);
}
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| OSUB Inventory Functions | [os_invt.md](../01_OSUB/os_invt.md) | Generated CRUD functions |
| Load Functions | [ds_load.md](ds_load.md) | Load operations |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation |

