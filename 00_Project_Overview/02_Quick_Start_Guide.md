# Quick Start Guide

**Document Version:** 1.0
**Last Updated:** 2024-12-22
**Author:** PlinyHub HARVEST
**Confidence Score:** 0.90

---

## Overview

This guide provides step-by-step instructions for new developers to get started with the Auro System codebase, including environment setup, build procedures, and initial navigation.

---

## Prerequisites

### Required Software
- **Visual Studio 2017** (or compatible version)
- **Microsoft SQL Server Standard Edition** (local or remote)
- **Kepware KEPServerEX** (for PLC communication)
- **Windows Server** (development VM or physical machine)

### Required Access
- Read access to `D:\ICIS\AuroDev\` (source code)
- Read access to `D:\Auro\` (runtime environment)
- Database access to AuroDev database
- Administrative privileges for registry and environment setup

---

## Initial Setup

### 1. Environment Variables

Run the environment setup batch file:
```batch
C:\Netlogon\Local_Set_Environment.bat
```

Or manually set:
- `ICIS_BYPASS` - Developer bypass mode (set to any value)
- `MEM_HOME` - Root directory of MEM installation
- `PATH` - Includes paths to MEM utilities and binaries

### 2. Registry Setup

Enable developer bypass mode:
1. Open Registry Editor (`regedit`)
2. Navigate to: `HKEY_CURRENT_USER\Environment`
3. Create String Value: `ICIS_BYPASS`
4. Set value to: `1`

### 3. Database Connection

Verify database connectivity:
- Server: SQL Server instance (check `D:\Auro\File\` for connection strings)
- Database: AuroDev database
- Authentication: Windows or SQL Server authentication

---

## Build Process

### Build Order

1. **Build Utilities First:**
   ```batch
   cd D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\util\p_ut_regst
   msbuild p_ut_regst.vcxproj /p:Configuration=Release
   ```

2. **Build Message Compiler:**
   ```batch
   cd D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\util\makemsg
   msbuild makemsg.vcxproj /p:Configuration=Release
   makemsg.exe
   makemsg.exe icis_english
   ```

3. **Build Core Libraries:**
   - `ccsub` (if exists)
   - `csub`
   - `dsub`
   - `osub`
   - `cryptlib`

4. **Build Background Processes:**
   - Build all `p_ar_*`, `p_cc_*`, `p_sy_*`, `p_si_*`, `p_tk_*` projects

5. **Build VB.NET Projects (in order):**
   - `MHC_Interop`
   - `GenScript`
   - `mhcMenu`

### Post-Build Steps

After building `GenScript`:
```batch
xcopy /Y /Q "GenScript\bin\*.dll" "D:\ICIS\AuroDev\Exec\"
```

---

## Running the System

### Warm Start Sequence

1. **Initialize Database:**
   ```batch
   cd D:\Auro\Exec
   p_sy_dbini.exe
   ```

2. **Register System:**
   ```batch
   p_ut_regst.exe
   ```

3. **Start Background Processes (in order):**
   ```batch
   start p_ar_fndwk.exe
   timeout /t 5
   start p_ar_movdp.exe
   start p_ar_stkdp.exe
   start p_ar_agvdp.exe
   start p_ar_rtndp.exe
   start p_ar_srcmp.exe
   start p_ar_agvcp.exe
   start p_ar_rtnxcp.exe
   start p_cc_stkcmx.exe
   start p_cc_mudpcm.exe
   start Kepware_PLC.exe
   start p_sy_stats.exe
   start p_sy_emailer.exe
   start p_sy_erlog.exe
   start p_si_AuroHost.exe
   start p_si_AuroTrack.exe
   ```

4. **Start UI:**
   ```batch
   start mhcMenu.exe
   ```

---

## Code Navigation

### Key Directories

| Directory | Purpose | Location |
|-----------|---------|----------|
| **C++ Source** | Background processes | `D:\ICIS\AuroDev\clogan\AuroDev\MSVC Programs\` |
| **VB.NET Source** | UI dialogs | `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\dilg\mhcMenu\` |
| **Database Routines** | OSUB access | `D:\ICIS\AuroDev\clogan\AuroDev\MSVB Programs\osub\` |
| **Executables** | Runtime binaries | `D:\Auro\Exec\` |
| **Configuration** | Config files | `D:\Auro\File\` |

### Finding Code

1. **Background Process:**
   - Search for `p_ar_movdp.cpp` in `MSVC Programs\area\p_ar_movdp\`

2. **UI Dialog:**
   - Search for `frm_Store.vb` in `MSVB Programs\dilg\mhcMenu\`

3. **Database Table:**
   - Check `D:\Auro\File\database_tables.xml` for table definition
   - Find generated routines in `MSVB Programs\osub\z_*.vb`

4. **Configuration:**
   - Check `D:\Auro\File\` for `.cnf`, `.xml`, `.msg` files

---

## Common Tasks

### Adding a New Dialog

1. Create form in Visual Studio: `frm_NewDialog.vb`
2. Add to `menu.cnf`:
   ```xml
   <Dialog Name="New Dialog">
       <Object>frm_NewDialog</Object>
       <Action Desc="Allow to Add">Add</Action>
   </Dialog>
   ```
3. Run `p_sy_dbini.exe` to update security tables
4. Rebuild `mhcMenu` project

### Adding a Database Table

1. Add table definition to `D:\Auro\File\database_tables.xml`
2. Run `p_ut_makdb.exe` to generate access routines
3. Generated files appear in `MSVB Programs\osub\z_*.vb`
4. Create SQL scripts for table creation

### Debugging a Process

1. Set breakpoints in Visual Studio
2. Attach debugger to running process:
   - Debug → Attach to Process
   - Select process (e.g., `p_ar_movdp.exe`)
3. Or run directly from Visual Studio (F5)

---

## Documentation Navigation

### For New Developers

1. **Start Here:** [Executive Summary](00_Executive_Summary.md)
2. **Understand Architecture:** [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
3. **Learn Workflows:** [Workflow Index](../05_Workflows/00_Workflow_Index.md)
4. **Explore Code:** [Code Reference Index](../03_Code_Reference/00_Code_Index.md)
5. **Understand Data:** [Database Overview](../04_Database_Reference/00_Database_Overview.md)

### For Experienced Developers

- Use [Code Reference Index](../03_Code_Reference/00_Code_Index.md) as navigation
- Reference [Architecture Diagrams](../01_System_Architecture/) for system understanding
- Check [Troubleshooting](../07_Troubleshooting/) for common issues

---

## Troubleshooting

### Build Errors

- **Missing DLLs:** Check `D:\ICIS\AuroDev\Exec\` for required DLLs
- **Database Connection:** Verify SQL Server is running and accessible
- **Registry Errors:** Ensure `ICIS_BYPASS` is set correctly

### Runtime Errors

- **Process Won't Start:** Check logs in `D:\Auro\Logs\`
- **Database Errors:** Verify `p_sy_dbini.exe` ran successfully
- **Communication Errors:** Check Kepware configuration and PLC connectivity

### Common Issues

See [Troubleshooting Guide](../07_Troubleshooting/00_Troubleshooting_Index.md) for detailed solutions.

---

## Related Documents

- [Executive Summary](00_Executive_Summary.md)
- [System Scale and Scope](01_System_Scale_and_Scope.md)
- [Architecture Overview](../01_System_Architecture/00_Architecture_Overview.md)
- [Coding Standards](../02_Coding_Standards/00_Standards_Overview.md)
- [Troubleshooting Guide](../07_Troubleshooting/00_Troubleshooting_Index.md)

---

## Cross-References

| Topic | Document | Section |
|-------|----------|---------|
| Build Process | [Coding Standards](../02_Coding_Standards/00_Standards_Overview.md) | Build Workflow |
| Process Startup | [Process Startup Sequence](../01_System_Architecture/04_Process_Startup_Sequence.md) | Startup Order |
| Database Setup | [Database Overview](../04_Database_Reference/00_Database_Overview.md) | Initialization |
| Configuration | [Configuration Overview](../06_Configuration/00_Configuration_Overview.md) | Setup |
| Troubleshooting | [Troubleshooting Index](../07_Troubleshooting/00_Troubleshooting_Index.md) | Common Issues |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2024-12-22 | Initial creation |

