# .NET Version Upgrade Scenario

## Preferences
- **Flow Mode**: Automatic
- **Target Framework**: net10.0

## Source Control
- **Source Branch**: master
- **Working Branch**: upgrade-dotnet-10
- **Commit Strategy**: After Each Task
- **Branch Sync**: Auto (Merge)

## Strategy
**Selected**: All-at-Once  
**Rationale**: Single project with clear Windows Forms dependency. Atomic upgrade from .NET Framework 2.0 to .NET 10.0-windows with SDK-style conversion. No inter-project dependencies to coordinate.

### Execution Constraints
- SDK-style conversion must complete before framework upgrade
- Windows Desktop workload support required (net10.0-windows target)
- All Windows Forms APIs and System.Drawing references will be re-examined post-upgrade
- Build validation required after each major change group
- Full solution build must pass before considering upgrade complete

## Key Decisions Log

- **Framework Selection**: .NET 10 (LTS) selected — Long-term support until November 2028
- **Strategy**: All-at-Once — Single project with no inter-project dependencies
- **Project Format**: Requires SDK-style conversion before framework upgrade
- **Target**: net10.0-windows to enable Windows Forms support
