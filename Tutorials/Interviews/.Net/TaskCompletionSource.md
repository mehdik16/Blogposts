# TaskCompletionSource in .NET: Comprehensive Guide with Examples

## What is TaskCompletionSource?

`TaskCompletionSource<T>` is a powerful .NET class that allows you to create and control a `Task<T>` manually. Instead of a task being created by an async method or a thread pool operation, you explicitly set the result, exception, or cancellation state of the task. This is essential for bridging asynchronous APIs that don't natively support the Task-based Asynchronous Pattern (TAP).

## When to Use TaskCompletionSource

TaskCompletionSource is useful in the following scenarios:

1. **Wrapping callback-based APIs** into Task-based APIs
2. **Wrapping event-based asynchronous patterns** (EAP) into TAP
3. **Implementing custom async operations** that don't fit standard patterns
4. **Coordinating multiple asynchronous operations** with custom logic
5. **Implementing timeouts and cancellation** for non-cancellable operations

## Core Example: Wrapping a Callback-Based API

### Problem: Legacy Callback API

```csharp
// Old callback-based API
public class LegacyDataService
{
    public void FetchData(string id, Action<string> onSuccess, Action<Exception> onError)
    {
        // Simulates async operation
        Task.Run(() =>
        {
            try
            {
                Thread.Sleep(1000); // Simulate work
                onSuccess($"Data for {id}");
            }
            catch (Exception ex)
            {
                onError(ex);
            }
        });
    }
}
```

### Solution: Convert to Task-Based API

```csharp
public class ModernDataService
{
    private readonly LegacyDataService _legacy = new();

    public Task<string> FetchDataAsync(string id)
    {
        var tcs = new TaskCompletionSource<string>();

        _legacy.FetchData(
            id,
            result => tcs.SetResult(result),           // Success
            exception => tcs.SetException(exception)   // Error
        );

        return tcs.Task;
    }
}

// Usage
var service = new ModernDataService();
var data = await service.FetchDataAsync("123");
Console.WriteLine(data); // Output: Data for 123
```

## Example 2: Wrapping Event-Based Asynchronous Pattern (EAP)

### Problem: Event-Based API

```csharp
public class FileDownloader
{
    public event EventHandler<DownloadCompletedEventArgs> DownloadCompleted;

    public void Download(string url)
    {
        Task.Run(() =>
        {
            try
            {
                Thread.Sleep(2000); // Simulate download
                DownloadCompleted?.Invoke(this, new DownloadCompletedEventArgs 
                { 
                    Data = "Downloaded content",
                    Error = null
                });
            }
            catch (Exception ex)
            {
                DownloadCompleted?.Invoke(this, new DownloadCompletedEventArgs 
                { 
                    Data = null,
                    Error = ex
                });
            }
        });
    }
}

public class DownloadCompletedEventArgs : EventArgs
{
    public string Data { get; set; }
    public Exception Error { get; set; }
}
```

### Solution: Convert to Task-Based API

```csharp
public static Task<string> DownloadAsync(this FileDownloader downloader, string url)
{
    var tcs = new TaskCompletionSource<string>();
    
    EventHandler<DownloadCompletedEventArgs> handler = null;
    handler = (sender, e) =>
    {
        // Unsubscribe to prevent memory leaks
        downloader.DownloadCompleted -= handler;
        
        if (e.Error != null)
            tcs.SetException(e.Error);
        else
            tcs.SetResult(e.Data);
    };

    downloader.DownloadCompleted += handler;
    downloader.Download(url);

    return tcs.Task;
}

// Usage
var downloader = new FileDownloader();
var content = await downloader.DownloadAsync("http://example.com/file.txt");
Console.WriteLine(content);
```

## Example 3: Implementing Timeout for Non-Cancellable Operations

### Problem: Operation Without Timeout Support

```csharp
public class SlowService
{
    public Task<string> GetDataAsync()
    {
        return Task.Run(async () =>
        {
            await Task.Delay(10000); // Long-running operation
            return "Result";
        });
    }
}
```

### Solution: Add Timeout Using TaskCompletionSource

```csharp
public static async Task<T> WithTimeoutAsync<T>(Task<T> task, TimeSpan timeout)
{
    var tcs = new TaskCompletionSource<T>();
    
    using (var cts = new CancellationTokenSource(timeout))
    {
        var completedTask = await Task.WhenAny(task, Task.Delay(timeout));
        
        if (completedTask == task)
        {
            // Original task completed first
            return await task;
        }
        else
        {
            // Timeout occurred
            tcs.SetException(new TimeoutException($"Operation timed out after {timeout.TotalSeconds} seconds"));
            return await tcs.Task;
        }
    }
}

// Usage
try
{
    var service = new SlowService();
    var result = await WithTimeoutAsync(service.GetDataAsync(), TimeSpan.FromSeconds(2));
}
catch (TimeoutException ex)
{
    Console.WriteLine(ex.Message);
}
```

## Example 4: Coordinating Multiple Asynchronous Operations

### Problem: Custom Coordination Logic

```csharp
public class RiskCalculationService
{
    public Task<decimal> CalculateRiskAsync(string portfolioId)
    {
        var tcs = new TaskCompletionSource<decimal>();
        var operationsCompleted = 0;
        decimal totalRisk = 0;
        var lockObj = new object();

        // Start multiple async operations
        var tasks = new List<Task>();

        for (int i = 0; i < 3; i++)
        {
            var task = Task.Run(async () =>
            {
                try
                {
                    // Simulate risk calculation for different asset classes
                    await Task.Delay(Random.Shared.Next(100, 500));
                    var riskComponent = Random.Shared.Next(10, 50);

                    lock (lockObj)
                    {
                        totalRisk += riskComponent;
                        operationsCompleted++;

                        if (operationsCompleted == 3)
                        {
                            tcs.SetResult(totalRisk);
                        }
                    }
                }
                catch (Exception ex)
                {
                    tcs.TrySetException(ex);
                }
            });

            tasks.Add(task);
        }

        return tcs.Task;
    }
}

// Usage
var service = new RiskCalculationService();
var risk = await service.CalculateRiskAsync("portfolio-123");
Console.WriteLine($"Total Risk: {risk}");
```

## Example 5: Implementing Cancellation Support

### Problem: Operation That Doesn't Support Cancellation

```csharp
public static Task<T> CancellableOperationAsync<T>(Func<Task<T>> operation, CancellationToken cancellationToken)
{
    var tcs = new TaskCompletionSource<T>();

    // Register cancellation callback
    using (cancellationToken.Register(() =>
    {
        tcs.TrySetCanceled(cancellationToken);
    }))
    {
        // Start the operation
        _ = operation().ContinueWith(task =>
        {
            if (task.IsCompletedSuccessfully)
                tcs.TrySetResult(task.Result);
            else if (task.IsFaulted)
                tcs.TrySetException(task.Exception);
            else if (task.IsCanceled)
                tcs.TrySetCanceled();
        });
    }

    return tcs.Task;
}

// Usage
var cts = new CancellationTokenSource();
cts.CancelAfter(TimeSpan.FromSeconds(2));

try
{
    var result = await CancellableOperationAsync(
        async () =>
        {
            await Task.Delay(5000);
            return "Completed";
        },
        cts.Token
    );
}
catch (OperationCanceledException)
{
    Console.WriteLine("Operation was cancelled");
}
```

## Best Practices and Important Considerations

### 1. Use Try* Methods to Avoid Exceptions

```csharp
// Good: TrySetResult returns false if already set
tcs.TrySetResult(value);

// Avoid: SetResult throws if already set
// tcs.SetResult(value);
```

### 2. Avoid Race Conditions

```csharp
// Problem: Race condition
var tcs = new TaskCompletionSource<string>();
Task.Run(() => tcs.SetResult("First"));
Task.Run(() => tcs.SetResult("Second")); // May throw

// Solution: Use TrySetResult
var tcs = new TaskCompletionSource<string>();
Task.Run(() => tcs.TrySetResult("First"));
Task.Run(() => tcs.TrySetResult("Second")); // Returns false, no exception
```

### 3. Unsubscribe from Events to Prevent Memory Leaks

```csharp
// Always unsubscribe when done
EventHandler<EventArgs> handler = null;
handler = (sender, e) =>
{
    source.Event -= handler; // Unsubscribe
    tcs.SetResult(e.Data);
};

source.Event += handler;
```

### 4. Use RunSynchronously for Synchronous Continuations

```csharp
var tcs = new TaskCompletionSource<string>();

// If you know the continuation will be quick, use RunSynchronously
tcs.Task.ContinueWith(t => Console.WriteLine(t.Result), 
    TaskScheduler.FromCurrentSynchronizationContext());
```

## Real-World Financial Systems Example

In a risk management system, TaskCompletionSource is useful for coordinating market data feeds:

```csharp
public class MarketDataAggregator
{
    public Task<MarketData> GetAggregatedDataAsync(string[] symbols, CancellationToken cancellationToken)
    {
        var tcs = new TaskCompletionSource<MarketData>();
        var results = new Dictionary<string, PriceData>();
        var lockObj = new object();
        var pendingRequests = symbols.Length;

        foreach (var symbol in symbols)
        {
            _ = FetchPriceAsync(symbol, cancellationToken).ContinueWith(task =>
            {
                if (task.IsCompletedSuccessfully)
                {
                    lock (lockObj)
                    {
                        results[symbol] = task.Result;
                        pendingRequests--;

                        if (pendingRequests == 0)
                        {
                            tcs.SetResult(new MarketData { Prices = results });
                        }
                    }
                }
                else if (task.IsFaulted)
                {
                    tcs.TrySetException(task.Exception);
                }
            });
        }

        return tcs.Task;
    }

    private Task<PriceData> FetchPriceAsync(string symbol, CancellationToken cancellationToken)
    {
        // Simulate fetching price data
        return Task.Delay(Random.Shared.Next(100, 500), cancellationToken)
            .ContinueWith(_ => new PriceData { Symbol = symbol, Price = Random.Shared.Next(100, 1000) });
    }
}

public class MarketData
{
    public Dictionary<string, PriceData> Prices { get; set; }
}

public class PriceData
{
    public string Symbol { get; set; }
    public decimal Price { get; set; }
}
```

## Summary

TaskCompletionSource is an essential tool for advanced async programming in .NET. The key scenarios are:

| Scenario | Use Case |
|---|---|
| **Callback APIs** | Convert old callback-based APIs to Task-based |
| **Event-Based APIs** | Convert EAP to TAP |
| **Custom Coordination** | Implement custom async patterns |
| **Timeouts** | Add timeout support to operations |
| **Cancellation** | Add cancellation support to operations |

Remember to always use `TrySetResult`, `TrySetException`, and `TrySetCanceled` to avoid exceptions when the task is already completed, and always clean up event subscriptions to prevent memory leaks.
