# 12 - Data Binding, DataGridView and Databases

## What is data binding?
**Data binding** connects a control to data, so you do not copy values by hand.

## DataGridView
A table control to show and edit data.

```csharp
dgvCustomers.DataSource = customers;   // List<Customer>
```

- Each **property** becomes a **column**, each **object** becomes a **row**.
- Useful properties: `AutoGenerateColumns`, `AllowUserToAddRows`, `ReadOnly`, `SelectionMode`, `AutoSizeColumnsMode`.
- Get the selected item:

```csharp
if (dgvCustomers.CurrentRow?.DataBoundItem is Customer customer)
{
    txtName.Text = customer.Name;
}
```

## Simple binding
Bind one property of a control to one property of an object:

```csharp
txtName.DataBindings.Add("Text", customer, "Name");
```

When the user changes the text, `customer.Name` changes too (two-way).

## BindingSource
A middle layer between the UI and the data. It gives you **navigation**, **filtering** and **sorting**.

```csharp
var bindingSource = new BindingSource();
bindingSource.DataSource = customers;

dgvCustomers.DataSource = bindingSource;
txtName.DataBindings.Add("Text", bindingSource, "Name");

bindingSource.MoveNext();       // go to the next record
bindingSource.Filter = "...";   // works with DataTable / DataView
```

## BindingList<T> vs List<T>
- `List<T>`: the grid does **not** update when you add or remove items.
- `BindingList<T>`: the grid updates **automatically**.

```csharp
var customers = new BindingList<Customer>(list);
dgvCustomers.DataSource = customers;
customers.Add(new Customer { Name = "Ali" });   // the grid shows it
```

For a single property change to update the UI, the class must implement `INotifyPropertyChanged`.

## Working with a database

### ADO.NET (classic)
```csharp
using var connection = new SqlConnection(connectionString);
using var adapter = new SqlDataAdapter("SELECT * FROM Customers", connection);
var table = new DataTable();
adapter.Fill(table);

dgvCustomers.DataSource = table;
```
Main classes: `SqlConnection`, `SqlCommand`, `SqlDataReader`, `SqlDataAdapter`, `DataTable`, `DataSet`.

### Entity Framework Core
```csharp
using var context = new AppDbContext();
dgvCustomers.DataSource = context.Customers.ToList();
```

To get a list that tracks add/remove, use:
```csharp
context.Customers.Load();
dgvCustomers.DataSource = context.Customers.Local.ToBindingList();
```
Then call `context.SaveChanges()` to save.

## Best practices
- Do not create `DbContext` in every event handler. Use a **service / repository** layer (see file 15).
- Use `async` calls for database work (see file 14) so the UI does not freeze.
- Keep connection strings in a config file, not in the code.
