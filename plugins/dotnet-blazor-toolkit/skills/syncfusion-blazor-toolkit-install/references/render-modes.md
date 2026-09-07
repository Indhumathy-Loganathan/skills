# Render Modes Reference

Blazor supports multiple render modes. Toolkit components have specific requirements depending on interactivity needs.

## Static SSR (Server-Side Rendering)

**When to use**: Read-only content where no user interaction or dynamic updates are needed.

**Characteristic**: Components are rendered on the server and sent as HTML; no .NET code runs on the client.

**Toolkit limitation**: **Static SSR is NOT sufficient for interactive Toolkit components** (buttons, forms, dropdowns, etc.). Event handlers and state changes will not work.

**Example (won't work for interactive components)**:
```razor
@page "/counter"

<p>This counter is stuck at: @count</p>
<SfButton Disabled="true">Click doesn't work in SSR</SfButton>

@code {
    private int count = 0;
    // @onclick handlers don't fire in static SSR
}
```

## Interactive Server Rendering

**When to use**: Interactive components hosted on a Blazor Server or Server project in a split Web App.

**Characteristic**: User interactions are sent to the server, processed, and updates streamed back to the client in real-time.

**Toolkit requirement**: Fully supported; all event handlers and state changes work.

**Example (use this for interactive Toolkit)**:
```razor
@page "/counter"
@rendermode InteractiveServer

<p>Count: @count</p>
<SfButton @onclick="IncrementCount">Increment</SfButton>

@code {
    private int count = 0;
    private void IncrementCount() => count++;
}
```

**Setup**:
```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerRenderMode();

builder.Services.AddSyncfusionBlazorToolkit();

var app = builder.Build();
```

## Interactive WebAssembly Rendering

**When to use**: Interactive components running as WebAssembly on the client (Blazor WebAssembly projects or Client projects in split Web Apps).

**Characteristic**: .NET runs in the browser; no server round-trip for user interactions (lower latency after initial load).

**Toolkit requirement**: Fully supported; all event handlers and state changes work.

**Example (use this for WASM Toolkit)**:
```razor
@page "/counter"
@rendermode InteractiveWebAssembly

<p>Count: @count</p>
<SfButton @onclick="IncrementCount">Increment</SfButton>

@code {
    private int count = 0;
    private void IncrementCount() => count++;
}
```

**Setup**:
```csharp
// Program.cs (Blazor WebAssembly or Client project)
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");

builder.Services.AddSyncfusionBlazorToolkit();

await builder.Build().RunAsync();
```

## Mixed Modes (Split Web App)

In a split Blazor Web App, you can use both:

**Server components** (Interactive Server):
```razor
@page "/server-counter"
@rendermode InteractiveServer

<SfButton @onclick="OnClick">Server Button</SfButton>

@code {
    private void OnClick() { /* runs on server */ }
}
```

**Client components** (Interactive WebAssembly):
```razor
@page "/client-counter"
@rendermode InteractiveWebAssembly

<SfButton @onclick="OnClick">Client Button</SfButton>

@code {
    private void OnClick() { /* runs in browser as WASM */ }
}
```

## Decision Tree

1. **Does the component need to respond to user clicks or changes?**
   - **No** → Static SSR is fine (no render mode needed)
   - **Yes** → Go to question 2

2. **Should processing happen on the server or in the browser?**
   - **Server** → Use `@rendermode InteractiveServer`
   - **Browser (WASM)** → Use `@rendermode InteractiveWebAssembly`

3. **Toolkit component won't respond?**
   - **Symptom**: Click or input events don't work
   - **Solution**: Ensure the component or page has an interactive render mode applied

## Common Mistakes

1. **Forgetting `@rendermode`**: Interactive components in a static page won't respond to clicks.
2. **Using static SSR for interactive components**: Toolkit components require interactive mode for event handling.
3. **Mismatched render mode**: Trying to use `InteractiveServer` in a WebAssembly-only app won't work.
4. **Assuming automatic**: Render mode doesn't "just work"; it must be explicitly declared.
