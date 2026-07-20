# 04-system-drawing-fixes: Resolve System.Drawing API compatibility

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
