# 07 - Common Controls

All controls inherit from `System.Windows.Forms.Control`, so they share many properties and events.

## Common controls
| Control | Use |
|---|---|
| `Label` | Show fixed text |
| `TextBox` | User types text (`Multiline`, `PasswordChar`, `ReadOnly`) |
| `Button` | Run an action when clicked |
| `CheckBox` | True / false choice (`Checked`) |
| `RadioButton` | Choose **one** option from a group |
| `ComboBox` | Drop-down list |
| `ListBox` | List of items |
| `DateTimePicker` | Choose a date or time |
| `NumericUpDown` | Choose a number with up/down arrows |
| `PictureBox` | Show an image |
| `ProgressBar` | Show progress of a task |

## Properties that most controls have
| Property | Meaning |
|---|---|
| `Name` | Name in code (`btnSave`) |
| `Text` | Text shown to the user |
| `Enabled` | If `false`, the user cannot use it (it looks gray) |
| `Visible` | If `false`, the control is hidden |
| `Location` | Position (X, Y) inside its container |
| `Size` | Width and height |
| `BackColor` / `ForeColor` | Background and text color |
| `Font` | Font of the text |
| `TabIndex` | Order when the user presses Tab |
| `Tag` | Free place to store any object (extra data) |
| `Dock` / `Anchor` | Layout (see file 08) |

## Naming convention
Use a short prefix + a clear name:

| Control | Prefix | Example |
|---|---|---|
| Button | `btn` | `btnSave` |
| TextBox | `txt` | `txtName` |
| Label | `lbl` | `lblTitle` |
| ComboBox | `cmb` | `cmbCountry` |
| CheckBox | `chk` | `chkActive` |
| RadioButton | `rdo` | `rdoMale` |
| DataGridView | `dgv` | `dgvCustomers` |

## Examples

### ComboBox
```csharp
cmbCountry.Items.Add("Egypt");
cmbCountry.Items.Add("Saudi Arabia");
cmbCountry.SelectedIndex = 0;                 // select first item

string selected = cmbCountry.SelectedItem?.ToString();
```

### Better: fill it from data
```csharp
cmbCountry.DataSource = countries;            // a List<Country>
cmbCountry.DisplayMember = "Name";            // what the user sees
cmbCountry.ValueMember = "Id";                // the hidden value
int id = (int)cmbCountry.SelectedValue;
```

### RadioButton
Radio buttons in the **same container** work as one group. Put each group inside a `GroupBox` or `Panel`.

### ListBox
```csharp
listBox1.Items.Add("Item 1");
listBox1.Items.RemoveAt(0);
```
