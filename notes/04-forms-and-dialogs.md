# 04 - Forms and Dialogs

## Show vs ShowDialog

| Method | Behavior |
|---|---|
| `Show()` | Opens the form. Code continues to the next line right away. You can go back to the other form. |
| `ShowDialog()` | Opens a **modal** form. Code **stops** until the form is closed. You cannot go back to the previous form. |

```csharp
var form2 = new Form2();

form2.Show();        // next line runs immediately

form2.ShowDialog();  // next line runs only after form2 is closed
```

## Load event
- The `Load` event runs **after the constructor** and **before the form is shown**.
- Good place for loading initial data.

Order of execution:
1. Constructor
2. `Load` event
3. Form is displayed

## Dialog controls (OpenFileDialog)

Ready-made windows for common tasks (open file, save file, choose color, choose font).
- They are **components**, not visible controls. They appear in the **component tray**.
- Use `ShowDialog()` and check that the result is `DialogResult.OK`.

| Dialog | Purpose |
|---|---|
| `OpenFileDialog` | Choose a file to open |
| `SaveFileDialog` | Choose where to save a file |
| `FolderBrowserDialog` | Choose a folder |
| `ColorDialog` | Choose a color |
| `FontDialog` | Choose a font |



### DialogResult
`OK`, `Cancel`, `Yes`, `No`, `Retry`, `Abort`, `Ignore`.
Always check the result before using the value. The user may press Cancel.

### Useful properties
- `Filter` and `FilterIndex` (index starts from 1)
- `Multiselect` and `FileNames` (select many files)
- `InitialDirectory`, `Title`
- `FileName` = full path, `SafeFileName` = name only
- `OverwritePrompt`, `DefaultExt` (SaveFileDialog)

### MessageBox
A static class to show a message and get the user's choice.

```csharp
var result = MessageBox.Show("Delete this item?", "Confirm",
    MessageBoxButtons.YesNo, MessageBoxIcon.Question);
if (result == DialogResult.Yes) { /* delete */ }
```

### Custom dialog
- Open your own form with `ShowDialog()`.
- Set the button's `DialogResult` property to close the form with a result.
- `AcceptButton` = Enter key, `CancelButton` = Esc key.

### Tip
Wrap dialogs in `using` so they are disposed automatically.

```csharp
using var dialog = new OpenFileDialog();
if (dialog.ShowDialog() == DialogResult.OK)
{
    string path = dialog.FileName;
}
```