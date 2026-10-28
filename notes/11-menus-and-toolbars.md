# 11 - Menus, Toolbars, Status Bar and Timer

## Menu and bars
| Control | Use |
|---|---|
| `MenuStrip` | Main menu at the top (File, Edit, Help) |
| `ContextMenuStrip` | Menu that opens with right-click |
| `ToolStrip` | Toolbar with icon buttons |
| `StatusStrip` | Bar at the bottom to show status (for example "Ready") |

### MenuStrip
1. Drag `MenuStrip` to the form.
2. Type the menu names (`File`, `Open`, `Exit`).
3. Double click an item to create its `Click` handler.

```csharp
private void exitToolStripMenuItem_Click(object sender, EventArgs e)
{
    Application.Exit();
}
```

Useful properties of a menu item:
- `ShortcutKeys` -> for example `Ctrl+O`
- `Checked` -> shows a check mark
- `Enabled` / `Visible`

### ContextMenuStrip
Create it, then set the `ContextMenuStrip` property of any control to use it.

### StatusStrip
```csharp
toolStripStatusLabel1.Text = "File saved";
```

> Tip: items inside these strips are **not** controls. They inherit from `ToolStripItem`.

## Timer
`Timer` runs code again and again every few milliseconds.
It is a **component** (it appears in the component tray, like OpenFileDialog).

```csharp
timer1.Interval = 1000;   // 1000 ms = 1 second
timer1.Tick += (s, e) => lblTime.Text = DateTime.Now.ToString("T");
timer1.Start();
// timer1.Stop();
```

- `Enabled` = `true` starts it too.
- The `Tick` event runs on the **UI thread**, so it is safe to touch controls.
- Do not put heavy work in `Tick`, it will freeze the UI.
