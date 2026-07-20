# Upgrade Plan

## Strategy

**Approach**: All-at-Once  
**Rationale**: Single project with clear Windows Forms dependency. Atomic upgrade from .NET Framework 2.0 to .NET 10.0-windows with SDK-style conversion. No inter-project dependencies to coordinate.

### Execution Constraints

- SDK-style conversion must complete before framework upgrade
- Windows Desktop workload support required (net10.0-windows target)
- All Windows Forms APIs and System.Drawing references will be re-examined post-upgrade
- Build validation required after each major change group
- Full solution build must pass before considering upgrade complete

---

## Phases

### Phase 1: Preparatory Changes

### 01-project-sdk-conversion: Convert HASH.csproj to SDK-style format

The HASH project currently uses legacy .NET Framework project format (.csproj). SDK-style format is required for .NET 5+ and significantly improves tooling, dependency management, and cross-platform support.

**Scope**: HASH\\HASH.csproj project file  
**Current format**: Classic (ToolsVersion-based) csproj  
**Target format**: SDK-style (modern MSBuild format)

This is a prerequisite for upgrading to .NET 10. The legacy format does not support modern .NET frameworks.

**Key considerations**:
- Project currently has no NuGet package references (uses no packages.config)
- 51 code files will be retained as-is; only the project file structure changes
- Build configuration and properties will be modernized
- Assembly name, root namespace, and other build outputs will be preserved

**Done when**:
- [ ] HASH.csproj converted to SDK-style
- [ ] Project loads without errors in Visual Studio
- [ ] All 51 code files are accounted for in the new project structure
- [ ] csproj file validates against SDK schema

---

### Phase 2: Framework Upgrade

### 02-framework-upgrade: Upgrade HASH.csproj to .NET 10.0-windows

Upgrade the target framework from .NET Framework 2.0 to .NET 10.0-windows (Windows Forms enabled).

**Scope**: HASH\\HASH.csproj  
**Target framework**: net10.0-windows  
**Key dependencies**: 
- Windows Forms APIs (2,881 references — 90.8% of detected APIs)
- System.Drawing APIs (281 references — 8.9% of detected APIs)
- 3 legacy Windows Forms controls (StatusBar, DataGrid, ContextMenu)

The assessment identified that the project is heavily Windows Forms-based with some System.Drawing usage. The net10.0-windows target provides both Windows Forms support and the required runtime environment.

**Known issues to expect**:
- **Binary incompatible APIs**: 2,881 — Will require recompilation and possible code fixes
- **Source incompatible APIs**: 281 — May require API call updates
- **Legacy controls**: StatusBar, DataGrid, ContextMenu have been removed; need migration to ToolStrip, MenuStrip, ContextMenuStrip, DataGridView
- **Binding redirects**: AutoGenerateBindingRedirects not currently configured

**Research before starting**:
- Identify all System.Drawing.Font and System.Drawing.Image usages
- Catalog all legacy control instances (search for StatusBar, DataGrid, MainMenu, MenuItem, ToolBar, ContextMenu)
- Determine if System.Drawing.Common NuGet package is needed
- Check for any manual assembly binding redirects that need to be carried forward

**Done when**:
- [ ] TargetFramework set to net10.0-windows in project file
- [ ] UseWindowsDesktop property set to true (if not auto-included)
- [ ] Solution builds without framework-related errors
- [ ] No unresolved assembly binding issues
- [ ] Project loads and builds in Visual Studio

---

### Phase 3: API Compatibility Fixes

### 03-windows-forms-api-fixes: Resolve Windows Forms API compatibility issues

Fix binary and source incompatible Windows Forms APIs in the codebase.

**Scope**: All 51 code files in HASH project  
**API issues**: 
- 2,881 binary incompatible Windows Forms references
- 3 legacy control instances needing replacement

Most of these are framework infrastructure references that will resolve during recompilation. However, deprecated control types (StatusBar, DataGrid, ContextMenu, MainMenu, MenuItem, ToolBar) must be manually replaced.

**Replacements needed**:
- StatusBar → None (remove or integrate status into form)
- DataGrid → DataGridView
- ContextMenu → ContextMenuStrip
- MainMenu → MenuStrip
- MenuItem → ToolStripMenuItem
- ToolBar → ToolStrip

**Research before starting**:
- Search codebase for all instances of deprecated control types
- Catalog API usage patterns for each deprecated control
- Determine functional equivalents in modern Windows Forms

**Done when**:
- [ ] All legacy control types replaced with modern equivalents
- [ ] Code compiles without Windows Forms API errors
- [ ] Application builds and runs
- [ ] No CS0246 (missing type) or CS0115 (override issues) related to Windows Forms

---

### Phase 4: System.Drawing Migration

### 04-system-drawing-fixes: Resolve System.Drawing API compatibility

Address System.Drawing.Common API compatibility issues.

**Scope**: All code using System.Drawing (281 identified references)  
**Technologies affected**: GDI+ / System.Drawing 2D graphics, imaging, and printing APIs

In .NET 5+, System.Drawing is available only on Windows via the System.Drawing.Common NuGet package. Most APIs will continue to work when added as a dependency.

**Research before starting**:
- Identify all System.Drawing.Font, System.Drawing.Bitmap, System.Drawing.Image, System.Drawing.Graphics usages
- Verify that System.Drawing.Common NuGet package satisfies all graphics needs
- If cross-platform graphics are needed in future, note SkiaSharp or ImageSharp as alternatives

**Done when**:
- [ ] System.Drawing.Common NuGet package added if needed
- [ ] All graphics code compiles without errors
- [ ] Graphics rendering tested at runtime (fonts, bitmaps, images render correctly)

---

### Phase 5: Validation

### 05-final-validation: Full solution build, test, and sign-off

Comprehensive validation that the upgrade is complete and functional.

**Scope**: Entire HASH project and solution  
**Validation steps**:
- Full solution build with no warnings or errors
- Application startup and basic form rendering
- Windows Forms interaction (button clicks, form events, control behavior)
- Any runtime behavioral changes related to .NET 10

**Done when**:
- [ ] `dotnet build` succeeds with zero warnings
- [ ] Project builds in Visual Studio without errors
- [ ] Application executable launches without exceptions
- [ ] All form controls render and respond to user input
- [ ] No compatibility or runtime errors observed

---

## Notes

This upgrade moves the project from an 18-year-old .NET Framework 2.0 to the latest .NET 10 LTS release. The major changes include:

1. **Project file format** — SDK-style (modern, cleaner)
2. **Runtime** — .NET Core 10 (cross-platform runtime, modern GC, performance improvements)
3. **API set** — Windows Forms still fully supported on Windows
4. **Build tooling** — Modern MSBuild, NuGet 5.0+, better diagnostics
5. **Language** — Access to modern C# features (nullable refs, records, patterns, etc.)

The assessment identified 3,172+ lines of code requiring review/modification, primarily due to Windows Forms binary compatibility updates and legacy control replacement.
