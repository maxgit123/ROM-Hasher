# 01-project-sdk-conversion: Convert HASH.csproj to SDK-style format

The HASH project currently uses legacy .NET Framework project format (.csproj). SDK-style format is required for .NET 5+ and significantly improves tooling, dependency management, and cross-platform support.

**Scope**: HASH\HASH.csproj project file  
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
