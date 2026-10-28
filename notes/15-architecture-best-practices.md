# 15 - Architecture and Best Practices

Windows Forms is easy to start, but code can become messy fast if all the logic is inside the forms.

## The problem: "Fat forms"
```csharp
private void btnSave_Click(object sender, EventArgs e)
{
    var connection = new SqlConnection("...");   // database inside UI
    // validation, business rules, SQL... all here
}
```
This code is hard to test, hard to read, and hard to reuse.

## Use layers
```
UI (Forms)  ->  Services (business logic)  ->  Repositories (data access)  ->  Database
```

- **Forms**: only show data and handle user actions.
- **Services**: rules and logic.
- **Repositories**: talk to the database (EF Core).
- Use **interfaces** between layers so you can replace or test each layer.

## MVP pattern (Model - View - Presenter)
A good pattern for Windows Forms.

| Part | Job |
|---|---|
| **Model** | Data and business logic |
| **View** | The form. Shows data. Has no logic. It exposes an interface |
| **Presenter** | Gets data from the model and updates the view. Handles user actions |

```csharp
public interface ICustomerView
{
    string CustomerName { get; set; }
    event EventHandler SaveClicked;
    void ShowMessage(string message);
}

public class CustomerPresenter
{
    private readonly ICustomerView _view;
    private readonly ICustomerService _service;

    public CustomerPresenter(ICustomerView view, ICustomerService service)
    {
        _view = view;
        _service = service;
        _view.SaveClicked += async (s, e) =>
        {
            await _service.AddAsync(_view.CustomerName);
            _view.ShowMessage("Saved");
        };
    }
}
```
The presenter can now be unit tested **without** creating a real form.

## Dependency Injection in Program.cs
```csharp
static void Main()
{
    var services = new ServiceCollection();

    services.AddDbContext<AppDbContext>(o => o.UseSqlServer(connectionString));
    services.AddScoped<ICustomerRepository, CustomerRepository>();
    services.AddScoped<ICustomerService, CustomerService>();
    services.AddTransient<MainForm>();

    using var provider = services.BuildServiceProvider();

    ApplicationConfiguration.Initialize();
    Application.Run(provider.GetRequiredService<MainForm>());
}
```
The form gets its services in the constructor:
```csharp
public MainForm(ICustomerService service)
{
    InitializeComponent();
    _service = service;
}
```
> In desktop apps there is no web request, so "scoped" lives as long as the provider. For long-running apps, create a scope per form or per operation (`IServiceScopeFactory`).

## Best practices checklist
- [ ] Rename all controls with clear names.
- [ ] Do not edit `*.Designer.cs` by hand.
- [ ] No business logic or SQL inside event handlers.
- [ ] Use `async/await` for slow work.
- [ ] Use `using` for disposable objects (dialogs, connections, contexts).
- [ ] Validate input in the UI **and** in the business layer.
- [ ] Do not make controls `public` to share data between forms.
- [ ] Handle exceptions and show friendly messages.
