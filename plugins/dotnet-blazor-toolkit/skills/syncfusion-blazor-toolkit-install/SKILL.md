---
name: syncfusion-blazor-toolkit-install
description: >
  Install and configure the open-source Syncfusion Blazor Toolkit
  (Syncfusion.Blazor.Toolkit) for Blazor Server, WebAssembly, and Blazor Web App.
  Use for package identity, AddSyncfusionBlazorToolkit(), _Imports.razor,
  Fluent theme CSS, and interactive render modes. Do not use for commercial
  Syncfusion.Blazor* packages, license keys, or per-component API.
license: MIT
metadata:
  author: "Syncfusion Inc"
  version: "1.0.2"
  compatibility: ".NET 8+, Blazor Server, WebAssembly, Blazor Web App"
---

## Core Rules

1. Use the exact package ID `Syncfusion.Blazor.Toolkit`.
2. Call `AddSyncfusionBlazorToolkit()` in `Program.cs` before `builder.Build()`.
3. Import `@using Syncfusion.Blazor.Toolkit` in `_Imports.razor`.
4. Link `_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css` in the app host file.
5. Use `@rendermode` for interactive Toolkit components; static SSR is read-only only.
6. In split Blazor Web Apps, register Toolkit in every project that uses Toolkit components.
7. Never use commercial `Syncfusion.Blazor*` packages or license-key APIs for Toolkit.
8. Keep this skill install-focused; use the references for details and troubleshooting.

## Quick Decision Table

| Scenario | Package | Program.cs | Host file for CSS | Interactivity |
| --- | --- | --- | --- | --- |
| Blazor Server | `Syncfusion.Blazor.Toolkit` | `builder.Services.AddSyncfusionBlazorToolkit()` | `App.razor` or `_Host.cshtml` | `@rendermode` if clicks/inputs are needed |
| Blazor WebAssembly | `Syncfusion.Blazor.Toolkit` | `builder.Services.AddSyncfusionBlazorToolkit()` in the client app | `wwwroot/index.html` | `@rendermode InteractiveWebAssembly` |
| Split Blazor Web App | `Syncfusion.Blazor.Toolkit` | Register in Server and/or Client, depending on where components live | `App.razor` | `@rendermode InteractiveServer` or `InteractiveWebAssembly` |
| Static SSR only | `Syncfusion.Blazor.Toolkit` | Register services as needed | `App.razor` or `index.html` | None; read-only only |

## Minimal Setup

### Package

```bash
dotnet add package Syncfusion.Blazor.Toolkit
```

### Program.cs

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents();
builder.Services.AddSyncfusionBlazorToolkit();

var app = builder.Build();
```

### _Imports.razor

```razor
@using Syncfusion.Blazor.Toolkit
```

### Host file CSS

```html
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
```

## Top Failure Symptoms

- **Unstyled components**: the Fluent CSS link is missing, wrong, or placed in the wrong host file. See [Theme and host files](./references/theme-and-host-files.md).
- **Services not configured**: `AddSyncfusionBlazorToolkit()` is missing from `Program.cs`. See [Troubleshooting guide](./references/troubleshooting.md).
- **Clicks or inputs do nothing**: the page is using static SSR. Add `@rendermode InteractiveServer` or `@rendermode InteractiveWebAssembly`. See [Render modes explained](./references/render-modes.md).
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