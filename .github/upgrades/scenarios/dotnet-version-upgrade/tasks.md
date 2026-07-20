# Upgrade Tasks

## Status Summary

| Status | Count | Task IDs |
|--------|-------|----------|
| Pending | 4 | 02, 03, 04, 05 |
| In Progress | 0 | — |
| Completed | 1 | 01 |
| Skipped | 0 | — |
| Failed | 0 | — |

**Total**: 5 tasks

## Task Hierarchy

### Phase 1: Preparatory Changes
- ✅ **01-project-sdk-conversion** *(completed)* — Convert HASH.csproj to SDK-style format

### Phase 2: Framework Upgrade
- 🔄 **02-framework-upgrade** *(pending)* — Upgrade HASH.csproj to .NET 10.0-windows

### Phase 3: API Compatibility Fixes
- 🔘 **03-windows-forms-api-fixes** *(pending)* — Resolve Windows Forms API compatibility issues

### Phase 4: System.Drawing Migration
- 🔘 **04-system-drawing-fixes** *(pending)* — Resolve System.Drawing API compatibility

### Phase 5: Validation
- 🔘 **05-final-validation** *(pending)* — Full solution build, test, and sign-off

## Next Task

**02-framework-upgrade** — Upgrade HASH.csproj to .NET 10.0-windows

See `tasks/02-framework-upgrade/task.md` for details, or run `start_task("02-framework-upgrade")` to begin.
