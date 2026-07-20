# 02-framework-upgrade: Upgrade HASH.csproj to .NET 10.0-windows

Upgrade the target framework from .NET Framework 2.0 to .NET 10.0-windows (Windows Forms enabled).

**Scope**: HASH\HASH.csproj  
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
