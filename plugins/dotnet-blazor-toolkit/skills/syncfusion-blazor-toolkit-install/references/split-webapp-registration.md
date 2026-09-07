# Split Blazor Web App Service Registration

When using a split Blazor Web App (separate Server and Client projects), Toolkit services must be registered in the correct location based on which projects use Toolkit components.

## Scenario 1: Only Server uses Toolkit

**Server/Program.cs**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents();
builder.Services.AddSyncfusionBlazorToolkit();  // <-- Server-side registration

var app = builder.Build();
// ... rest of configuration
```

**Client/Program.cs**:
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

// No Toolkit registration needed if Client doesn't use Toolkit
builder.Services.AddScoped(sp => new HttpClient { });

await builder.Build().RunAsync();
```

## Scenario 2: Only Client uses Toolkit

**Server/Program.cs**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents();
// No Toolkit registration; Server doesn't use it

var app = builder.Build();
```

**Client/Program.cs**:
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddSyncfusionBlazorToolkit();  // <-- Client-side registration

await builder.Build().RunAsync();
```

## Scenario 3: Both Server and Client use Toolkit (MOST COMMON)

**Server/Program.cs**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents();
builder.Services.AddSyncfusionBlazorToolkit();  // <-- Register here

var app = builder.Build();
```

**Client/Program.cs**:
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddSyncfusionBlazorToolkit();  // <-- Also register here

await builder.Build().RunAsync();
```

## Common Mistake: Forgetting Client Registration

If Toolkit components are used in the Client project but `AddSyncfusionBlazorToolkit()` is only called in Server `Program.cs`, the Client will not have access to Toolkit services and components will fail.

**Symptom**: Components work in Server-hosted components but fail in Client components.

**Solution**: Ensure both projects have the registration call if both use Toolkit.

## Theme CSS in Split Web App

Theme CSS should be linked in the **App.razor** file (which is typically the shared host for both Server and Client):

```razor
<!-- App.razor (shared, loaded first) -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <base href="/" />
    <link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
    <link rel="stylesheet" href="app.css" />
</head>
<body>
    <Routes />
    <script src="_framework/blazor.web.js"></script>
</body>
</html>
```

This ensures the theme is available to both Server and Client components.
