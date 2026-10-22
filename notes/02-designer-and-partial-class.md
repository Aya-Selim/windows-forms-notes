# 02 - Designer and Partial Class

## Two views
Every form has two views:
- **Design view** - drag and drop controls. Shortcut: `Shift + F7`
- **Code view** - the C# code. Shortcut: `F7`

## The designer writes code for you
Everything you do in the designer is saved as C# code in the background.
**Do not edit the designer code by hand.** It is generated and can be overwritten.

## Partial class
`partial` lets us split one class into **more than one file**.

Why is it useful? Visual Studio puts its generated code in one file, and you put your code in another:

- `Form1.cs` -> your code
- `Form1.Designer.cs` -> generated code (do not touch)

Both files together make the complete `Form1` class.

```csharp
// Form1.cs
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
    }
}
```

## InitializeComponent()
- It is inside `Form1.Designer.cs`.
- It applies every change you made in the designer (create controls, set properties, add them to the form).
- Always keep this call in the constructor.

## Program.cs
`Program.cs` creates the startup form and starts the application.

```csharp
static void Main()
{
    ApplicationConfiguration.Initialize();
    Application.Run(new Form1()); // Form1 is the startup form
}
```

## Closing the application
- `Application.Exit()` closes the whole application.
- `Close()` on the startup form also ends the app.
- Dispose the form when you no longer need it: `form.Dispose()`.