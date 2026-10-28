# 13 - Validation

**Validation** checks that the user's input is correct before you use it.

## Why?
- Protect your data and your database.
- Give the user a clear message about what is wrong.

## ErrorProvider
`ErrorProvider` is a component. It shows a small red icon next to a control, with a message on hover.

```csharp
errorProvider1.SetError(txtEmail, "Email is required");   // show error
errorProvider1.SetError(txtEmail, "");                    // clear error
```

## Validating and Validated events
`Validating` runs when the control **loses focus**. You can stop the user from leaving the control.

```csharp
private void txtEmail_Validating(object sender, CancelEventArgs e)
{
    if (string.IsNullOrWhiteSpace(txtEmail.Text) || !txtEmail.Text.Contains('@'))
    {
        e.Cancel = true;    // focus stays on this control
        errorProvider1.SetError(txtEmail, "Enter a valid email");
    }
    else
    {
        errorProvider1.SetError(txtEmail, "");
    }
}
```

- `Validated` runs **after** validation succeeds.
- `CausesValidation` must be `true` on the control that the user moves to (default is `true`). Set it to `false` on a **Cancel** button so the user can leave even if the data is invalid.

## Validate all controls before saving
```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    if (!ValidateChildren())
        return;     // at least one control is invalid

    // save data
}
```

## Other ways to restrict input
- `MaskedTextBox`: forces a format (phone, date).
- `NumericUpDown`: only numbers, with `Minimum` and `Maximum`.
- `TextBox.MaxLength`: limit the text length.
- `KeyPress` event: block unwanted characters (see file 06).
- `Regex`: check patterns such as email or phone number.

```csharp
bool valid = Regex.IsMatch(email, @"^[^@\s]+@[^@\s]+\.[^@\s]+$");
```

## Best practice
Do the **real** validation in your business layer or service too, not only in the UI. UI validation is for a good user experience. Business validation protects the system.
