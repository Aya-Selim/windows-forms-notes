# 16 - Resources, Settings and Debugging

## Resources
A **resource** is a file or value that is embedded in your app (images, icons, text).

- Open **Properties -> Resources.resx** in Solution Explorer and add images or strings.
- Use them in code:

```csharp
pictureBox1.Image = Properties.Resources.Logo;
```

- **Localization**: set the form's `Localizable = true` and change `Language`. Visual Studio creates a `.resx` file for each language, so you can support Arabic and English.
- For right-to-left languages, use `RightToLeft = Yes` and `RightToLeftLayout = true`.

## Application settings
Store user or app settings (last folder, theme, window size).

1. Open **Properties -> Settings.settings**.
2. Add a setting (name, type, scope).
3. Use it:

```csharp
Properties.Settings.Default.LastFolder = folderPath;
Properties.Settings.Default.Save();

string folder = Properties.Settings.Default.LastFolder;
```

| Scope | Meaning |
|---|---|
| `User` | Can change at run time and save |
| `Application` | Read-only at run time |

> Do **not** store passwords or secrets here.

## Debugging tips
- Put a **breakpoint** (`F9`) inside the event handler. Use `F10` (step over) and `F11` (step into).
- Use the **Watch** and **Locals** windows to see values.
- Use `Debug.WriteLine("text")` instead of `MessageBox` for debug messages. It only runs in Debug mode.
- If the designer shows an error, check that your code in the constructor or `Load` does not use things (like a database) that are not available at **design time**. You can check `DesignMode`:

```csharp
if (!DesignMode)
{
    LoadData();
}
```

## Handling unhandled exceptions
Add this in `Program.cs` to avoid a crash with no message:

```csharp
Application.ThreadException += (s, e) =>
    MessageBox.Show(e.Exception.Message, "Unexpected error");

Application.SetUnhandledExceptionMode(UnhandledExceptionMode.CatchException);
```
Also log the exception to a file (for example with Serilog) so you can fix it later.

## Publish the app
Right-click the project -> **Publish**, or use the command line:

```bash
dotnet publish -c Release -r win-x64 --self-contained false
```
The output folder contains the `.exe` you can give to users (they need the .NET runtime unless you use `--self-contained true`).
