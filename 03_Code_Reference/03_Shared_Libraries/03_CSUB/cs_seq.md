# cs_seq - Sequence Number Functions

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.90

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cs_seq.cpp` / `CS_SEQ.H` |
| **Library** | CSUB (Common Subroutines) |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\Base\trunk\MSVC Programs\ccsub\` |
| **Status** | ✅ Active - Sequence Generation |

---

## Overview

The `cs_seq` module provides sequence number generation functions for the MHC/MEM system. It generates unique sequential numbers for moves, loads, orders, and other entities requiring unique identifiers.

**Key Responsibilities:**
- Generate unique sequence numbers
- Manage sequence counters
- Handle rollover conditions
- Thread-safe number generation

---

## Key Functions

### cs_seq_init

**Signature:**
```cpp
long cs_seq_init(const char* seqName, long startValue, long maxValue);
```

**Purpose:** Initialize a named sequence counter.

| Parameter | Type | Description |
|-----------|------|-------------|
| `seqName` | `const char*` | Sequence name |
| `startValue` | `long` | Initial value |
| `maxValue` | `long` | Maximum before rollover |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Sequence initialized |
| `GP.BAD` | Initialization failed |

---

### cs_seq_next

**Signature:**
```cpp
long cs_seq_next(const char* seqName);
```

**Purpose:** Get next sequence number.

| Parameter | Type | Description |
|-----------|------|-------------|
| `seqName` | `const char*` | Sequence name |

| Return Value | Description |
|--------------|-------------|
| `> 0` | Next sequence number |
| `-1` | Error |

---

### cs_seq_current

**Signature:**
```cpp
long cs_seq_current(const char* seqName);
```

**Purpose:** Get current sequence value without incrementing.

| Parameter | Type | Description |
|-----------|------|-------------|
| `seqName` | `const char*` | Sequence name |

| Return Value | Description |
|--------------|-------------|
| `> 0` | Current sequence number |
| `-1` | Error or not found |

---

### cs_seq_reset

**Signature:**
```cpp
long cs_seq_reset(const char* seqName, long newValue);
```

**Purpose:** Reset sequence to a specific value.

| Parameter | Type | Description |
|-----------|------|-------------|
| `seqName` | `const char*` | Sequence name |
| `newValue` | `long` | New value |

| Return Value | Description |
|--------------|-------------|
| `GP.GOOD` | Sequence reset |
| `GP.BAD` | Error |

---

## Standard Sequences

| Sequence Name | Purpose | Range |
|---------------|---------|-------|
| `SEQ_MOVE_ID` | Move record IDs | 1 - 999999999 |
| `SEQ_LOAD_ID` | Load identifiers | 1 - 99999999 |
| `SEQ_ORDER_NO` | Order numbers | 1 - 99999999 |
| `SEQ_MSG_NO` | Message sequence | 1 - 999999 |

---

## Common Usage Patterns

### Pattern 1: Generate New Move ID

```cpp
// Get next move ID
long moveId = cs_seq_next("SEQ_MOVE_ID");
if (moveId > 0) {
    move.move_id = moveId;
    os_movs.insert(&move);
}
```

### Pattern 2: Generate Formatted Load ID

```cpp
// Get sequence number
long seq = cs_seq_next("SEQ_LOAD_ID");

// Format as load ID (e.g., "LD00123456")
char loadId[GP_MAX_LOADID + 1];
sprintf(loadId, "LD%08ld", seq);
```

### Pattern 3: Message Sequence for Host Communication

```cpp
// Get message sequence for host message
long msgSeq = cs_seq_next("SEQ_MSG_NO");

// Build host message with sequence
sprintf(hostMsg, "MSG%06ld|%s", msgSeq, messageData);
```

### Pattern 4: Initialize Sequences at Startup

```cpp
// Initialize standard sequences
cs_seq_init("SEQ_MOVE_ID", 1, 999999999);
cs_seq_init("SEQ_LOAD_ID", 1, 99999999);
cs_seq_init("SEQ_ORDER_NO", 1, 99999999);
cs_seq_init("SEQ_MSG_NO", 1, 999999);

cs_log_printf(FILELINE, GP_LOGLVL_2, "Sequence counters initialized");
```

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| Move Operations | [ds_movs.md](../04_DSUB/ds_movs.md) | Move ID generation |
| Load Operations | [ds_load.md](../04_DSUB/ds_load.md) | Load ID generation |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial documentation |

