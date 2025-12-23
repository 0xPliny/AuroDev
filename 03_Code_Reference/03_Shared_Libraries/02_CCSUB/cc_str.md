# cc_str - String Manipulation Class

**Document Version:** 1.0  
**Last Updated:** 2024-12-23  
**Author:** CmL  
**Confidence Score:** 0.95

---

## Document Metadata

| Field | Value |
|-------|-------|
| **Source File** | `cc_str.cpp`, `cc_str.h` |
| **Location** | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\ccsub\` |
| **Class Name** | `cc_str` |
| **Class Type** | Static class instance |
| **Status** | ✅ Hand-written - DO NOT MODIFY without review |

---

## Overview

The `cc_str` class provides **safe string manipulation functions** to replace standard C library functions (`strcpy`, `strcmp`, `strcat`, etc.). All functions include buffer size checking to prevent buffer overflows, which is critical for system reliability and security.

**Critical Operations:**
- Safe string copying
- String comparison
- String concatenation
- String formatting
- String validation and sanitization

**Key Principle:** **NEVER use standard C string functions** (`strcpy`, `strcmp`, `strcat`, `sprintf`) - always use `cc_str` functions.

---

## Class Declaration

```cpp
class cc_str {
public:
    // Copy operations
    char *copy(char *to_str, size_t sizeInBytes, const char *from_str);
    char *ncopy(char *to_str, size_t sizeInBytes, const char *from_str, long length);
    
    // Comparison operations
    long comp(const char *s1, const char *s2);
    long ncomp(const char *s1, const char *s2, long length);
    
    // Concatenation operations
    char *concat(char *to, size_t sizeInBytes, const char *from);
    char *cat(char *to, size_t sizeInBytes, const char *from);  // Alias for concat
    
    // Formatting operations
    char *format(char *buffer, size_t sizeInBytes, const char *format, ...);
    
    // Case conversion
    char *upper(char *str);
    char *lower(char *str);
    
    // String manipulation
    char *trim(char *str);
    char *clip(char *str);  // Remove trailing spaces
    char *set(char *str, char ch, long count);
    
    // Validation
    GP_BOOL isblank(const char *str);
    GP_BOOL isnumeric(const char *str);
    
    // Conversion
    long atoi(char *in_ptr, long length);
    
    // Tokenization
    char *token(char *str, const char *delimiters, char **saveptr);
    
    // Pattern matching
    long findpat(const char *str, const char *pattern);
    
    // Replacement
    char *replace(char *str, size_t sizeInBytes, const char *old, const char *new_str);
    
    // Sanitization
    char *sanitize(char *str, size_t sizeInBytes);
    char *sqlSafetyCleanUp(char *str, size_t sizeInBytes);
};

// Global static instance
static cc_str cc_str;
```

---

## Function Documentation

### cc_str.copy

**Signature:**
```cpp
char *copy(char *to_str, size_t sizeInBytes, const char *from_str)
```

**Description:**
Safely copies a string from source to destination with buffer size checking. Equivalent to `strncpy` but safer - **always null-terminates** the destination.

**Parameters:**
- `to_str` (char*): Destination buffer
- `sizeInBytes` (size_t): Size of destination buffer in bytes (use `sizeof()`)
- `from_str` (const char*): Source string to copy

**Return Values:**
- Returns pointer to `to_str` on success
- Always null-terminates the destination string
- Truncates if source is longer than buffer

**Logic Flow:**
1. Validates buffer size > 0
2. Copies characters from source to destination
3. Ensures destination is null-terminated
4. Respects buffer size limits (copies at most `sizeInBytes - 1` characters)

**Dependencies:**
- Standard C library: `memcpy`, `strlen`

**Usage Example:**
```cpp
char basketId[BASKET_ID_LEN];
cc_str.copy(basketId, sizeof(basketId), sourceBasketId);

// ✅ CORRECT - Always use sizeof()
char loadId[7];
cc_str.copy(loadId, sizeof(loadId), "L00001");

// ❌ WRONG - NEVER use strcpy
// strcpy(loadId, "L00001");  // FORBIDDEN
```

**Source Files:**
- `cc_str.h` (line ~45)
- `cc_str.cpp` (implementation)

---

### cc_str.ncopy

**Signature:**
```cpp
char *ncopy(char *to_str, size_t sizeInBytes, const char *from_str, long length)
```

**Description:**
Copies a specified number of characters from source to destination. Useful when you need to copy only part of a string.

**Parameters:**
- `to_str` (char*): Destination buffer
- `sizeInBytes` (size_t): Size of destination buffer
- `from_str` (const char*): Source string
- `length` (long): Number of characters to copy

**Return Values:**
- Returns pointer to `to_str`
- Always null-terminates

**Usage Example:**
```cpp
char prefix[10];
cc_str.ncopy(prefix, sizeof(prefix), "ABCDEFGHIJKLMNOP", 5);  // Copies "ABCDE"
```

---

### cc_str.comp

**Signature:**
```cpp
long comp(const char *s1, const char *s2)
```

**Description:**
Compares two strings. Equivalent to `strcmp` but returns MHC standard return codes.

**Parameters:**
- `s1` (const char*): First string
- `s2` (const char*): Second string

**Return Values:**
- `GP_MATCH` (0): Strings are equal
- Non-zero: Strings differ (positive if s1 > s2, negative if s1 < s2)

**Dependencies:**
- Standard C library: `strcmp`

**Usage Example:**
```cpp
if (cc_str.comp(status, "ACTIVE") == GP_MATCH) {
    // Status is active
    return GP.GOOD;
}

// ❌ WRONG - NEVER use strcmp
// if (strcmp(status, "ACTIVE") == 0) { }  // FORBIDDEN
```

**Source Files:**
- `cc_str.h` (line ~39)
- `cc_str.cpp` (implementation)

---

### cc_str.ncomp

**Signature:**
```cpp
long ncomp(const char *s1, const char *s2, long length)
```

**Description:**
Compares the first `length` characters of two strings.

**Parameters:**
- `s1` (const char*): First string
- `s2` (const char*): Second string
- `length` (long): Number of characters to compare

**Return Values:**
- `GP_MATCH` (0): Strings match for `length` characters
- Non-zero: Strings differ

**Usage Example:**
```cpp
if (cc_str.ncomp(command, "STORE", 5) == GP_MATCH) {
    // Command starts with "STORE"
}
```

---

### cc_str.concat / cc_str.cat

**Signature:**
```cpp
char *concat(char *to, size_t sizeInBytes, const char *from)
char *cat(char *to, size_t sizeInBytes, const char *from)  // Alias
```

**Description:**
Safely concatenates a string to the end of another string with buffer size checking. Equivalent to `strncat` but safer.

**Parameters:**
- `to` (char*): Destination buffer (must be null-terminated)
- `sizeInBytes` (size_t): Size of destination buffer
- `from` (const char*): Source string to append

**Return Values:**
- Returns pointer to `to` on success
- Always null-terminates the result
- Truncates if result would exceed buffer size

**Dependencies:**
- Standard C library: `strlen`, `strncat`

**Usage Example:**
```cpp
char message[256];
cc_str.copy(message, sizeof(message), "Error: ");
cc_str.concat(message, sizeof(message), errorText);

// Or using alias
cc_str.cat(message, sizeof(message), " - Code: ");
cc_str.cat(message, sizeof(message), errorCode);

// ❌ WRONG - NEVER use strcat
// strcat(message, errorText);  // FORBIDDEN
```

**Source Files:**
- `cc_str.h` (line ~48)
- `cc_str.cpp` (implementation)

---

### cc_str.format

**Signature:**
```cpp
char *format(char *buffer, size_t sizeInBytes, const char *format, ...)
```

**Description:**
Safely formats a string using printf-style format specifiers. Equivalent to `snprintf` but with MHC return conventions.

**Parameters:**
- `buffer` (char*): Destination buffer
- `sizeInBytes` (size_t): Size of destination buffer
- `format` (const char*): Format string (printf-style)
- `...`: Variable arguments for format specifiers

**Return Values:**
- Returns pointer to `buffer`
- Always null-terminates
- Truncates if formatted string exceeds buffer size

**Dependencies:**
- Standard C library: `vsnprintf`

**Usage Example:**
```cpp
char message[256];
cc_str.format(message, sizeof(message), "Load %s moved from %s to %s", 
              loadId, fromLoc, toLoc);

// ❌ WRONG - NEVER use sprintf
// sprintf(message, "Load %s...", loadId);  // FORBIDDEN
```

---

### cc_str.upper / cc_str.lower

**Signature:**
```cpp
char *upper(char *str)
char *lower(char *str)
```

**Description:**
Converts a string to uppercase or lowercase **in place** (modifies the original string).

**Parameters:**
- `str` (char*): String to convert (modified in place)

**Return Values:**
- Returns pointer to `str`

**Dependencies:**
- Standard C library: `toupper`, `tolower`

**Usage Example:**
```cpp
char status[20];
cc_str.copy(status, sizeof(status), "pending");
cc_str.upper(status);  // Now "PENDING"

char command[20];
cc_str.copy(command, sizeof(command), "STORE");
cc_str.lower(command);  // Now "store"
```

**Source Files:**
- `cc_str.h` (lines ~78-79)
- `cc_str.cpp` (implementation)

---

### cc_str.trim

**Signature:**
```cpp
char *trim(char *str)
```

**Description:**
Removes leading and trailing whitespace from a string **in place**.

**Parameters:**
- `str` (char*): String to trim (modified in place)

**Return Values:**
- Returns pointer to `str`

**Usage Example:**
```cpp
char input[100];
cc_str.copy(input, sizeof(input), "  Load001  ");
cc_str.trim(input);  // Now "Load001"
```

---

### cc_str.clip

**Signature:**
```cpp
char *clip(char *str)
```

**Description:**
Removes trailing spaces from a string **in place**. Similar to `trim` but only removes trailing spaces.

**Parameters:**
- `str` (char*): String to clip (modified in place)

**Return Values:**
- Returns pointer to `str`

**Usage Example:**
```cpp
char location[20];
cc_str.copy(location, sizeof(location), "A01-01-01   ");
cc_str.clip(location);  // Now "A01-01-01"
```

---

### cc_str.set

**Signature:**
```cpp
char *set(char *str, char ch, long count)
```

**Description:**
Sets a string buffer to a specific character for a specified count. Useful for initializing buffers.

**Parameters:**
- `str` (char*): String buffer to set
- `ch` (char): Character to fill with
- `count` (long): Number of characters to set

**Return Values:**
- Returns pointer to `str`
- **Note:** Does NOT null-terminate - caller must do this

**Usage Example:**
```cpp
char buffer[100];
cc_str.set(buffer, ' ', 100);  // Fill with spaces
buffer[99] = '\0';  // Null terminate manually

// Or initialize to zeros
cc_str.set(buffer, '\0', 100);  // Fill with nulls
```

**Source Files:**
- `cc_str.h` (line ~54)
- `cc_str.cpp` (implementation)

---

### cc_str.isblank

**Signature:**
```cpp
GP_BOOL isblank(const char *str)
```

**Description:**
Checks if a string is blank (empty or contains only whitespace characters).

**Parameters:**
- `str` (const char*): String to check

**Return Values:**
- `GP_TRUE` (1): String is blank (empty or whitespace only)
- `GP_FALSE` (0): String contains non-whitespace characters

**Usage Example:**
```cpp
if (cc_str.isblank(userInput)) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "User input is blank");
    return GP.BAD;
}
```

**Source Files:**
- `cc_str.h` (line ~65)
- `cc_str.cpp` (implementation)

---

### cc_str.isnumeric

**Signature:**
```cpp
GP_BOOL isnumeric(const char *str)
```

**Description:**
Checks if a string contains only numeric characters (digits 0-9, optionally with leading minus sign).

**Parameters:**
- `str` (const char*): String to check

**Return Values:**
- `GP_TRUE` (1): String is numeric
- `GP_FALSE` (0): String contains non-numeric characters

**Usage Example:**
```cpp
if (cc_str.isnumeric(priorityStr)) {
    long priority = cc_str.atoi(priorityStr, strlen(priorityStr));
} else {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Invalid priority: %s", priorityStr);
    return GP.BAD;
}
```

---

### cc_str.atoi

**Signature:**
```cpp
long atoi(char *in_ptr, long length)
```

**Description:**
Converts a string to an integer, reading only the specified number of characters.

**Parameters:**
- `in_ptr` (char*): String to convert
- `length` (long): Number of characters to read

**Return Values:**
- Integer value of the string
- Returns 0 if conversion fails

**Dependencies:**
- Standard C library: `atol`, `memcpy`

**Usage Example:**
```cpp
char numStr[10];
cc_str.copy(numStr, sizeof(numStr), "12345");
long value = cc_str.atoi(numStr, 5);  // Returns 12345

// Convert first 3 characters only
long partial = cc_str.atoi(numStr, 3);  // Returns 123
```

**Source Files:**
- `cc_str.h` (line ~83)
- `cc_str.cpp` (lines ~67-88)

---

### cc_str.token

**Signature:**
```cpp
char *token(char *str, const char *delimiters, char **saveptr)
```

**Description:**
Tokenizes a string using specified delimiters. Similar to `strtok` but thread-safe with `saveptr`.

**Parameters:**
- `str` (char*): String to tokenize (NULL for subsequent calls)
- `delimiters` (const char*): Delimiter characters
- `saveptr` (char**): Pointer to save position (must persist between calls)

**Return Values:**
- Returns pointer to next token
- Returns NULL when no more tokens

**Usage Example:**
```cpp
char line[256] = "LOAD001,PD01,STOR";
char *saveptr = NULL;
char *token1 = cc_str.token(line, ",", &saveptr);  // "LOAD001"
char *token2 = cc_str.token(NULL, ",", &saveptr);  // "PD01"
char *token3 = cc_str.token(NULL, ",", &saveptr);  // "STOR"
```

---

### cc_str.findpat

**Signature:**
```cpp
long findpat(const char *str, const char *pattern)
```

**Description:**
Finds a pattern within a string. Returns position of first match.

**Parameters:**
- `str` (const char*): String to search
- `pattern` (const char*): Pattern to find

**Return Values:**
- Returns index of first match (0-based)
- Returns -1 if pattern not found

**Usage Example:**
```cpp
long pos = cc_str.findpat("LOAD001", "LOAD");  // Returns 0
long pos2 = cc_str.findpat("LOAD001", "XYZ");  // Returns -1
```

---

### cc_str.replace

**Signature:**
```cpp
char *replace(char *str, size_t sizeInBytes, const char *old, const char *new_str)
```

**Description:**
Replaces all occurrences of `old` substring with `new_str` substring.

**Parameters:**
- `str` (char*): String to modify
- `sizeInBytes` (size_t): Size of buffer
- `old` (const char*): Substring to replace
- `new_str` (const char*): Replacement substring

**Return Values:**
- Returns pointer to `str`
- Truncates if result exceeds buffer size

**Usage Example:**
```cpp
char message[256];
cc_str.copy(message, sizeof(message), "Error in LOAD001");
cc_str.replace(message, sizeof(message), "LOAD001", "LOAD002");  // "Error in LOAD002"
```

---

### cc_str.sanitize

**Signature:**
```cpp
char *sanitize(char *str, size_t sizeInBytes)
```

**Description:**
Sanitizes input strings by removing or escaping potentially dangerous characters. Used for user input validation.

**Parameters:**
- `str` (char*): String to sanitize (modified in place)
- `sizeInBytes` (size_t): Size of buffer

**Return Values:**
- Returns pointer to `str`

**Usage Example:**
```cpp
char userInput[256];
// Get user input...
cc_str.sanitize(userInput, sizeof(userInput));  // Remove dangerous chars
```

---

### cc_str.sqlSafetyCleanUp

**Signature:**
```cpp
char *sqlSafetyCleanUp(char *str, size_t sizeInBytes)
```

**Description:**
Removes or escapes SQL injection characters from a string. **Critical for database security.**

**Parameters:**
- `str` (char*): String to clean (modified in place)
- `sizeInBytes` (size_t): Size of buffer

**Return Values:**
- Returns pointer to `str`

**Usage Example:**
```cpp
char searchTerm[100];
cc_str.copy(searchTerm, sizeof(searchTerm), userInput);
cc_str.sqlSafetyCleanUp(searchTerm, sizeof(searchTerm));  // Prevent SQL injection
// Now safe to use in SQL WHERE clause
```

**Security Note:** Always use this function before constructing SQL queries with user input.

---

## Common Usage Patterns

### Pattern 1: Safe String Copy

```cpp
char loadId[7];
cc_str.copy(loadId, sizeof(loadId), sourceLoadId);

// Always use sizeof() for buffer size
char location[12];
cc_str.copy(location, sizeof(location), "A01-01-01");
```

### Pattern 2: String Comparison

```cpp
if (cc_str.comp(status, "ACTIVE") == GP_MATCH) {
    // Status is active
}

if (cc_str.comp(command, "STORE") != GP_MATCH) {
    cs_log_printf(FILELINE, CS_LOG_ERROR, "Invalid command: %s", command);
    return GP.BAD;
}
```

### Pattern 3: String Concatenation

```cpp
char message[256];
cc_str.copy(message, sizeof(message), "Error: ");
cc_str.cat(message, sizeof(message), errorText);
cc_str.cat(message, sizeof(message), " - Code: ");
cc_str.cat(message, sizeof(message), errorCodeStr);
```

### Pattern 4: String Formatting

```cpp
char logMsg[256];
cc_str.format(logMsg, sizeof(logMsg), "Load %s moved from %s to %s at %s", 
              loadId, fromLoc, toLoc, timestamp);
cs_log_printf(FILELINE, CS_LOG_INFO, logMsg);
```

### Pattern 5: Input Validation

```cpp
char userInput[100];
// Get user input...

// Validate not blank
if (cc_str.isblank(userInput)) {
    return GP.BAD;
}

// Sanitize for security
cc_str.sanitize(userInput, sizeof(userInput));
cc_str.sqlSafetyCleanUp(userInput, sizeof(userInput));

// Now safe to use
```

---

## Forbidden Functions

**NEVER use these standard C library functions in MHC code:**

| Forbidden Function | Use Instead |
|-------------------|-------------|
| `strcpy()`, `strncpy()` | `cc_str.copy()` |
| `strcmp()`, `strncmp()` | `cc_str.comp()`, `cc_str.ncomp()` |
| `strcat()`, `strncat()` | `cc_str.concat()`, `cc_str.cat()` |
| `sprintf()`, `snprintf()` | `cc_str.format()` |
| `strtok()` | `cc_str.token()` |

**Reason:** Standard C functions do not provide consistent buffer size checking and can lead to buffer overflows, security vulnerabilities, and system crashes.

---

## Related Documentation

| Topic | Document | Section |
|-------|----------|---------|
| CCSUB Overview | [CCSUB Library](../ccsub.md) | cc_str Overview |
| Coding Standards | [C++ Standards](../../02_Coding_Standards/02_CPP_Standards.md) | String Operations |
| Security | [Security Guidelines](../../02_Coding_Standards/04_Security.md) | SQL Injection Prevention |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-23 | Initial comprehensive documentation of all cc_str functions |

