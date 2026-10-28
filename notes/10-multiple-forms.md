# 10 - Multiple Forms

Most apps have more than one form. This file explains how to open them and how to send data between them.

## Open another form
```csharp
var form2 = new Form2();
form2.Show();          // non-modal: user can use both forms
form2.ShowDialog();    // modal: user must close form2 first
```

## Send data to another form

### Option 1: Constructor
Good when the data is **required**.
```csharp
public partial class Form2 : Form
{
    public Form2(string userName)
    {
        InitializeComponent();
        lblWelcome.Text = $"Welcome {userName}";
    }
}

new Form2("Aya").ShowDialog();
```

### Option 2: Public property
```csharp
public string UserName { get; set; }

var form2 = new Form2 { UserName = "Aya" };
form2.ShowDialog();
```

## Get data back from a dialog form
```csharp
// In Form2: expose the result
public string SelectedName => txtName.Text;

// In Form1:
using var form2 = new Form2();
if (form2.ShowDialog() == DialogResult.OK)
{
    lblResult.Text = form2.SelectedName;
}
```

## Option 3: Events (best for loose coupling)
Form2 raises an event. Form1 listens. Form2 does not need to know anything about Form1.

```csharp
// Form2
public event EventHandler<string> NameSaved;

private void btnSave_Click(object sender, EventArgs e)
{
    NameSaved?.Invoke(this, txtName.Text);
}

// Form1
var form2 = new Form2();
form2.NameSaved += (s, name) => lblResult.Text = name;
form2.Show();
```

## Do not do this
Do not make controls `public` so another form can change them directly. This creates tight coupling and the code becomes hard to maintain. Use properties, methods or events.

## MDI (Multiple Document Interface)
One **parent** form holds many **child** forms inside it (like old Word or Excel).

```csharp
// Parent form
this.IsMdiContainer = true;

// Open a child
var child = new ChildForm();
child.MdiParent = this;
child.Show();
```

- `LayoutMdi(MdiLayout.Cascade)` or `TileVertical` arranges the children.
- MDI is old. Modern apps often use tabs or one main form with panels (user controls), but it is still part of many courses.

## UserControl (reusable parts)
If you use the same group of controls in many forms, create a **UserControl**.
It is like a small form that you can drag from the Toolbox into other forms.
