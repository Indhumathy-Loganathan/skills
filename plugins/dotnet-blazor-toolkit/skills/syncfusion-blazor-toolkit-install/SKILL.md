---
name: syncfusion-blazor-toolkit-install
description: >
  Install and register the open-source Syncfusion Blazor Toolkit package,
  configure AddSyncfusionBlazorToolkit(), link the Fluent theme, and apply
  the correct interactive render mode for your app topology (Server / WASM / Auto).
  
  USE FOR: Toolkit setup, package verification, theme linking, split-app registration, render-mode troubleshooting.
  DO NOT USE FOR: component API details (use author-component), Blazor project creation (use create-blazor-project),
  commercial Syncfusion packages or license keys, Hybrid/MAUI.
license: MIT
compatibility: ".NET 8+, Blazor Server / WebAssembly / Auto / Static SSR"
metadata:
  author: "Syncfusion Inc"
  version: "1.0.2"
---

# Install Syncfusion Blazor Toolkit

## Core Rules

1. Use the exact package ID `Syncfusion.Blazor.Toolkit`.
2. Call `AddSyncfusionBlazorToolkit()` in `Program.cs` before `builder.Build()`.
3. Import `@using Syncfusion.Blazor.Toolkit` in `_Imports.razor`; add component namespaces such as `@using Syncfusion.Blazor.Toolkit.Buttons` only when a component needs them.
4. Link `_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css` in the app host file.
5. Use `@rendermode` for interactive Toolkit components in .NET 8+ Blazor Web App; legacy Blazor Server templates are already interactive (no `@rendermode` needed).
6. In split Blazor Web Apps, register Toolkit in every project that uses Toolkit components.
7. Never use commercial `Syncfusion.Blazor*` packages or license-key APIs for Toolkit.
8. Keep this skill install-focused; use the references for details and troubleshooting.

## Don’ts

- Don’t treat `Syncfusion.Blazor` or component-specific commercial packages as Toolkit dependencies.
- Don’t rely on static SSR when the component must respond to clicks, binding, or dynamic updates.
- Don’t turn this skill into a component API reference; use the demos or component skills for that.

## Quick Decision Table

| Scenario | Package | Program.cs | Host file for CSS | Interactivity |
| --- | --- | --- | --- | --- |
| Blazor Server (legacy _Host.cshtml) | `Syncfusion.Blazor.Toolkit` | `builder.Services.AddSyncfusionBlazorToolkit()` | `_Host.cshtml` | Not required (always interactive) |
| Blazor WebAssembly (standalone) | `Syncfusion.Blazor.Toolkit` | `builder.Services.AddSyncfusionBlazorToolkit()` | `wwwroot/index.html` | Already interactive (no `@rendermode`) |
| Blazor Web App (Auto) | `Syncfusion.Blazor.Toolkit` | Register in Server and Client projects that use Toolkit | `App.razor` | `@rendermode InteractiveAuto` |
| Blazor Web App (Split Server/Client) | `Syncfusion.Blazor.Toolkit` | Register in Server and Client projects that use Toolkit | `App.razor` | `@rendermode InteractiveServer`, `InteractiveWebAssembly`, or `InteractiveAuto` |
| Static SSR only | `Syncfusion.Blazor.Toolkit` | Register services as needed | `App.razor` or `index.html` | None; read-only only |

## Minimal Setup

### Package

```bash
dotnet add package Syncfusion.Blazor.Toolkit
```

### Program.cs

Match the render modes from the Quick Decision Table above. Do not add unused render modes.

**For Blazor Web App (Auto or Server+Client split)**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazorToolkit();

var app = builder.Build();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode();
```

**For Blazor Server only**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

builder.Services.AddSyncfusionBlazorToolkit();

var app = builder.Build();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();
```

**For Blazor WebAssembly**:
```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");

builder.Services.AddSyncfusionBlazorToolkit();

await builder.Build().RunAsync();
```

### _Imports.razor

```razor
@using Syncfusion.Blazor.Toolkit

@* Optional: Add component-specific namespaces only when using those components *@
@using Syncfusion.Blazor.Toolkit.Buttons
@using Syncfusion.Blazor.Toolkit.Calendars
```

For component-specific features, include the appropriate namespace in `_Imports.razor` as shown above.

### Host file CSS

```html
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
```

Always pick the host file from the Quick Decision Table above: `_Host.cshtml` for legacy Blazor Server, `App.razor` for Blazor Web App, or `wwwroot/index.html` for Blazor WebAssembly.

## Top Failure Symptoms

- **Unstyled components**: the Fluent CSS link is missing, wrong, or placed in the wrong host file. See [Theme and host files](./references/theme-and-host-files.md).
- **Services not configured**: `AddSyncfusionBlazorToolkit()` is missing from `Program.cs`. See [Troubleshooting guide](./references/troubleshooting.md).
- **Clicks or inputs do nothing**: the page is using static SSR. Add `@rendermode InteractiveServer`, `@rendermode InteractiveWebAssembly`, or `@rendermode InteractiveAuto`. See [Render modes explained](./references/render-modes.md).
- **Only one half of a split app works**: register Toolkit in both Server and Client if both use Toolkit. See [Split Blazor Web App registration](./references/split-webapp-registration.md).
- **Wrong package or license guidance**: the project uses a commercial `Syncfusion.Blazor*` package instead of the Toolkit. See [Package identity and splits](./references/package-identity.md).

## Next Steps

After installation, use the component demos or component-specific guidance for API details; this skill only covers setup and configuration.

## References

- [Package identity and splits](./references/package-identity.md)
- [Split Blazor Web App registration](./references/split-webapp-registration.md)
- [Theme and host files](./references/theme-and-host-files.md)
- [Render modes explained](./references/render-modes.md)
- [Troubleshooting guide](./references/troubleshooting.md)