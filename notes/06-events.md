# 06 - Events and Event Handlers

## What is an event?
An **event** is a message that says "something happened".
Examples: the user clicks a button, types text, moves the mouse, or closes the form.

Windows Forms is **event-driven**. Your program waits, and your code runs only when an event happens.

## Event handler
An **event handler** is a method that runs when the event happens.

```csharp
// Form1.cs (your code)
private void btnSave_Click(object sender, EventArgs e)
{
    MessageBox.Show("Saved!");
}
```

The designer connects the event to the handler in `Form1.Designer.cs`:

```csharp
this.btnSave.Click += new System.EventHandler(this.btnSave_Click);
```

- `+=` means "subscribe": when `Click` happens, call `btnSave_Click`.
- To create a handler: double click the control, or open the **Properties** window -> click the lightning icon -> double click the event.

## The two parameters
| Parameter | Meaning |
|---|---|
| `sender` | The control that raised the event |
| `e` | Extra data about the event (key pressed, mouse position...) |

One handler can serve many controls. Use `sender` to know which one was clicked:

```csharp
private void AnyButton_Click(object sender, EventArgs e)
{
    var button = (Button)sender;
    MessageBox.Show($"You clicked {button.Text}");
}
```

## Important events
| Event | When it happens |
|---|---|
| `Click` | The control is clicked |
| `TextChanged` | The text changed |
| `KeyDown` / `KeyPress` / `KeyUp` | A keyboard key is pressed |
| `MouseMove` / `MouseClick` | The mouse moves or clicks |
| `Load` | The form is loading |
| `FormClosing` | The form is about to close (you can cancel it) |
| `FormClosed` | The form is closed |
| `SelectedIndexChanged` | Selection changed in ComboBox or ListBox |
| `Validating` | The control loses focus and must be checked |

## Example: only allow numbers in a TextBox
```csharp
private void txtAge_KeyPress(object sender, KeyPressEventArgs e)
{
    if (!char.IsDigit(e.KeyChar) && !char.IsControl(e.KeyChar))
        e.Handled = true; // ignore this key
}
```
`e.Handled = true` tells Windows Forms: "I handled this key, do not process it again."

## Tips
- Keep handlers **short**. Do not put business logic inside them. Call a method or a service instead.
- If you delete a handler method by hand, the designer code still points to it and the build fails. Remove the event from the Properties window instead.
