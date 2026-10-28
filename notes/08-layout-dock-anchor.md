# 08 - Layout, Containers, Dock and Anchor

## Container controls
A **container** holds other controls. Moving or hiding the container also moves or hides the controls inside it.

| Container | Use |
|---|---|
| `Panel` | Group controls together (can scroll with `AutoScroll`) |
| `GroupBox` | Like a Panel, but with a title |
| `TabControl` | Many pages (tabs) in one place |
| `FlowLayoutPanel` | Places controls one after another (horizontal or vertical) |
| `TableLayoutPanel` | Places controls in a grid of rows and columns |
| `SplitContainer` | Two areas that the user can resize |

## Why layout matters
When the user resizes the form (or uses a different screen size or DPI), a fixed layout can look broken. Use **Anchor**, **Dock** and layout panels to build a UI that adapts.

## Anchor
Anchor keeps a **fixed distance** from the chosen edges of the container.

- Default: `Top, Left`. The control stays at the top-left.
- `Left, Right`: the control **stretches** when the form gets wider.
- `Bottom, Right`: the control stays at the bottom-right corner (good for OK / Cancel buttons).

```csharp
txtSearch.Anchor = AnchorStyles.Top | AnchorStyles.Left | AnchorStyles.Right;
```

## Dock
Dock **sticks** the control to an edge of its container.

| Value | Result |
|---|---|
| `Top` / `Bottom` | Sticks to that edge, full width |
| `Left` / `Right` | Sticks to that edge, full height |
| `Fill` | Fills all the remaining space |
| `None` | No docking |

```csharp
dataGridView1.Dock = DockStyle.Fill;
```

## Anchor vs Dock
| | Anchor | Dock |
|---|---|---|
| Idea | Keep distance from edges | Stick to an edge |
| Control keeps its own size? | Yes (unless anchored on both sides) | No, it is resized |
| Good for | Buttons, text boxes in a form | Main areas (grid, panels, menus) |

## Tips
- When you use several docked controls, the **order** matters. Use "Bring to Front / Send to Back" to change it.
- Use `Padding` (space inside a container) and `Margin` (space outside a control) to make a clean UI.
- `AutoSize` and `MinimumSize` help the form look right at small sizes.
- For complex forms, use `TableLayoutPanel` instead of placing everything by hand.
