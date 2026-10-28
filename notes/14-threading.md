# 14 - Threading and the UI Thread

## The problem
A Windows Forms app has **one UI thread**. It draws the screen and handles events.
If you run a long task (database call, file download, big loop) inside an event handler, the UI thread is busy and the form **freezes** ("Not Responding").

```csharp
// Bad: the form freezes for 5 seconds
private void btnLoad_Click(object sender, EventArgs e)
{
    Thread.Sleep(5000);
    lblStatus.Text = "Done";
}
```

## Solution: async / await
```csharp
private async void btnLoad_Click(object sender, EventArgs e)
{
    btnLoad.Enabled = false;
    lblStatus.Text = "Loading...";

    var data = await Task.Run(() => LoadHeavyData());   // runs on another thread

    dgvCustomers.DataSource = data;                     // back on the UI thread
    lblStatus.Text = "Done";
    btnLoad.Enabled = true;
}
```

- `await` does not block the UI. When the task finishes, the code continues on the **UI thread**.
- For I/O work (database, files, HTTP), use the real async methods: `ToListAsync()`, `ReadAllTextAsync()`, `HttpClient.GetStringAsync()`. You do not need `Task.Run` for them.
- `async void` is allowed **only** for event handlers. Use `async Task` everywhere else.

## Cross-thread operation error
Only the UI thread can touch controls. If another thread changes a control, you get:

`InvalidOperationException: Cross-thread operation not valid`

### Fix with Invoke
```csharp
void UpdateLabel(string text)
{
    if (lblStatus.InvokeRequired)
        lblStatus.Invoke(() => lblStatus.Text = text);
    else
        lblStatus.Text = text;
}
```

## Show progress
```csharp
var progress = new Progress<int>(value => progressBar1.Value = value);

await Task.Run(() =>
{
    for (int i = 1; i <= 100; i++)
    {
        Thread.Sleep(50);
        ((IProgress<int>)progress).Report(i);   // safe: runs on the UI thread
    }
});
```

## Cancel a long task
```csharp
private CancellationTokenSource _cts;

private async void btnStart_Click(object sender, EventArgs e)
{
    _cts = new CancellationTokenSource();
    try
    {
        await DoWorkAsync(_cts.Token);
    }
    catch (OperationCanceledException)
    {
        lblStatus.Text = "Canceled";
    }
}

private void btnCancel_Click(object sender, EventArgs e) => _cts?.Cancel();
```

## Old ways (good to know)
- `BackgroundWorker` component: has `DoWork`, `ProgressChanged`, `RunWorkerCompleted`. Still works, but `async/await` is simpler and recommended.
- `Thread` class: low level, avoid it for normal UI work.
