# CInterop (C++/VB.NET Interop)

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.85

---

## Overview

The `CInterop` library provides interoperability between VB.NET managed code and C++ unmanaged code.

---

## Purpose

Enables VB.NET dialogs to access C++ functions and mapped memory, handling marshalling between managed and unmanaged code.

---

## Location

- **DLL:** `D:\Auro\Exec\CInterop.dll`
- **Source:** `MSVC Programs\CInterop\`

---

## Key Functions

### VB.NET Access
```vbnet
Dim stackerHandle As IntPtr = VBDLL.GetStackerHandle(aisleNumber)
Dim schedule As Integer = VBDLL.GetStackerSchedule(stackerHandle)
```

---

## Related Documents

- [Shared Libraries Index](00_Shared_Libraries_Index.md)
- [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| IPC | [IPC and Shared Memory](../../01_System_Architecture/05_IPC_and_Shared_Memory.md) | Memory Access |
| VB.NET Standards | [VB.NET Standards](../../02_Coding_Standards/01_VB_NET_Standards.md) | Interop Usage |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

