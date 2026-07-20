# 03-windows-forms-api-fixes: Resolve Windows Forms API compatibility issues

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
