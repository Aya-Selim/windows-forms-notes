# 03 - Controls and Properties

## What is a control?
- A **control** is a class you can drag and drop from the **Toolbox**.
- Any class that has a GUI and supports drag and drop **inherits from**
  `System.Windows.Forms.Control`.
- Like any class, a control has **properties**, **events** and **methods**.

## Form and controls
- A form is a **container** for controls.
- The relation between a form and a button is **composition**
  (the form owns its controls).

## Properties window
- Open it with `F4`.
- A form has public properties. When you change one, the designer writes
  the code in `InitializeComponent()` automatically.
- You can sort properties **A-Z** or by **Categories**.

## Useful tips
- **Rename every control** after you add it (`btnSave`, `txtName`, `lblTitle`).
  Good names make code easier to read.
- If a control does not appear in the Toolbox:
  right click Toolbox -> **Choose General** -> load the DLL.
- **Anchor**: keeps a control at a fixed distance from the form edges.
  Anchor to **left and right** to stretch the control when the form is resized.