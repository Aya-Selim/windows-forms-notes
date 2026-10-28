# 09 - Form Properties and Lifecycle

## Important form properties
| Property | Meaning |
|---|---|
| `Text` | Title in the title bar |
| `StartPosition` | Where the form appears (`CenterScreen`, `CenterParent`...) |
| `FormBorderStyle` | Border type (`Sizable`, `FixedDialog`, `None`) |
| `MaximizeBox` / `MinimizeBox` | Show or hide these buttons |
| `WindowState` | `Normal`, `Maximized`, `Minimized` |
| `TopMost` | Stay on top of other windows |
| `ShowInTaskbar` | Show the form in the Windows taskbar |
| `Icon` | Icon of the form |
| `Opacity` | Transparency (0.0 to 1.0) |
| `AcceptButton` | Button that runs when the user presses **Enter** |
| `CancelButton` | Button that runs when the user presses **Esc** |
| `KeyPreview` | If `true`, the form receives keyboard events **before** the controls |

## Lifecycle (order of events)
1. **Constructor** - the form object is created. `InitializeComponent()` runs here.
2. **Load** - before the form is shown for the first time.
3. **Shown** - after the form is shown to the user.
4. The user works with the form.
5. **FormClosing** - the form is about to close. You can cancel it.
6. **FormClosed** - the form is closed.

## Load vs Shown
- `Load`: good for loading initial data and setting up controls.
- `Shown`: good for work that needs the form to be visible (for example starting a long task after the UI appears).

## Cancel closing
```csharp
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    var result = MessageBox.Show("Do you want to exit?", "Confirm",
        MessageBoxButtons.YesNo, MessageBoxIcon.Question);

    if (result == DialogResult.No)
        e.Cancel = true;   // the form stays open
}
```

`e.CloseReason` tells you why the form is closing (user click, Windows shutdown, `Application.Exit()`...).

## Close vs Hide vs Dispose
| Method | What it does |
|---|---|
| `Close()` | Closes the form and releases its resources |
| `Hide()` | Only hides it. The form is still in memory and can be shown again |
| `Dispose()` | Releases resources. (`Close()` calls it for forms opened with `Show()`) |

> A form opened with `ShowDialog()` is **not** disposed when closed. You must dispose it yourself, for example with `using`.
