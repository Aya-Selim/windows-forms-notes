# 05 - GDI+

- **GDI+** is used for drawing and painting (shapes, text, images).
- Coordinates:
  - `Width` = **X** axis
  - `Height` = **Y** axis
- The point (0, 0) is the **top-left** corner.

```csharp
protected override void OnPaint(PaintEventArgs e)
{
    base.OnPaint(e);
    e.Graphics.DrawRectangle(Pens.Blue, 20, 20, 100, 50);
}
```