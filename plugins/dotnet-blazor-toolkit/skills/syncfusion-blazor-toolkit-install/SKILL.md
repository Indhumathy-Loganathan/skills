---
name: syncfusion-blazor-toolkit-install
description: >
  Install and configure the open-source Syncfusion Blazor Toolkit package
  Syncfusion.Blazor.Toolkit. Use when adding the Toolkit NuGet package,
  calling AddSyncfusionBlazorToolkit(), updating _Imports.razor, linking theme
  CSS in the correct host file, and enabling interactive render modes for
  Blazor Server, WebAssembly, or split Blazor Web App. Do not use for commercial
  Syncfusion.Blazor* packages, license-key setup, Hybrid guidance (unless
  officially documented for Toolkit), or per-component API reference.
license: MIT
metadata:
  author: "Syncfusion Inc"
  version: "1.0.0"
---

## Purpose

This skill guides complete installation and configuration of the open-source **Syncfusion Blazor Toolkit** (`Syncfusion.Blazor.Toolkit` NuGet package) into Blazor applications. Upon completion, the Toolkit is correctly installed, services are registered, theme CSS is linked, namespaces are imported, and interactive render modes are configured as needed.

The outcome is a functioning Toolkit-based Blazor app ready to use Toolkit components.

### DO

- Produce a completely wired Toolkit installation: package, service registration, namespace imports, theme CSS, and render mode.
- Distinguish the open-source Toolkit (`Syncfusion.Blazor.Toolkit`) from commercial `Syncfusion.Blazor*` packages.
- Confirm the package ID is exactly `Syncfusion.Blazor.Toolkit`.
- Provide clear before/after code snippets showing what needs to be added or changed.

### DON'T

- Promise Toolkit includes commercial Syncfusion components or features not in the open-source package.
- Require or mention license keys as a prerequisite for Toolkit setup.
- Confuse the open-source Toolkit with commercial Syncfusion products.
- Provide per-component API reference (button click handlers, chart series enums, etc.).

---

## When to use / When NOT to use

### Use this skill for

- First-time Toolkit installation on Blazor Server, Blazor WebAssembly, or split Blazor Web App projects.
- Adding or reconfiguring the NuGet package in an existing Blazor project.
- Wiring service registration (`AddSyncfusionBlazorToolkit()`).
- Updating `_Imports.razor` with Toolkit namespaces.
- Linking Toolkit theme CSS in the correct host file.
- Configuring interactive render modes for components to work correctly.
- Diagnosing and fixing common installation failures (missing CSS, missing service registration, static SSR, wrong package, split app partial registration).

### DO NOT use this skill for

- Commercial `Syncfusion.Blazor*` package installation or licensing.
- License-key registration or commercial product setup.
- Detailed component API authoring (buttons, calendars, charts, inputs, notifications, popups, etc.).
- Hybrid hosting setup (unless official Toolkit documentation supports it).
- Sync/SSR-only scenarios where interactive components are not required.

---

## Prerequisites

### DO

- Require .NET 8 or later (minimum requirement for Syncfusion Blazor Toolkit).
- Assume the user has or can create a new Blazor Server, Blazor WebAssembly, or split Blazor Web App project.
- Assume basic familiarity with Blazor project structure and Program.cs.

### DON'T

- Require a Syncfusion license key for Toolkit usage.
- Assume commercial Syncfusion packages are already installed.
- Require npm or node_modules; Toolkit is a managed NuGet package.

---

## Installation Steps

### A. Package Identity

The Toolkit is distributed as a single NuGet package.

#### DO

- **Package name (exact)**: `Syncfusion.Blazor.Toolkit`
- **Install via dotnet CLI**:
  ```bash
  dotnet add package Syncfusion.Blazor.Toolkit
  ```
- **Or via PackageReference** in `.csproj`:
  ```xml
  <PackageReference Include="Syncfusion.Blazor.Toolkit" Version="1.0.2" />
  ```
  (Use the latest stable version from https://www.nuget.org/packages/Syncfusion.Blazor.Toolkit)
- **Distinguish clearly**: The open-source Toolkit is `Syncfusion.Blazor.Toolkit`, not commercial `Syncfusion.Blazor`, `Syncfusion.Blazor.Core`, `Syncfusion.Blazor.Buttons`, etc.

#### DON'T

- Install `Syncfusion.Blazor` or other `Syncfusion.Blazor.*` commercial packages for Toolkit scenarios.
- Assume `Syncfusion.Blazor` is a drop-in replacement for `Syncfusion.Blazor.Toolkit`.
- Add license-key registration APIs for Toolkit.

---

### B. Service Registration

After adding the NuGet package, register Toolkit services in `Program.cs`.

#### DO

- **Call `AddSyncfusionBlazorToolkit()`** (exact method name) in the services builder before `builder.Build()`:
  ```csharp
  var builder = WebApplication.CreateBuilder(args);

  // Register Blazor components
  builder.Services.AddRazorComponents();

  // Register Syncfusion Blazor Toolkit
  builder.Services.AddSyncfusionBlazorToolkit();

  var app = builder.Build();
  // ... rest of app setup
  ```
- **For split Blazor Web App** (Server + Client projects):
  - Call `AddSyncfusionBlazorToolkit()` in the **Server** project's `Program.cs` if server-side components use Toolkit.
  - Call `AddSyncfusionBlazorToolkit()` in the **Client** project's `Program.cs` if client-side (WebAssembly) components use Toolkit.
  - If both halves use Toolkit components, register in **both** `Program.cs` files.

#### DON'T

- Call commercial-only licensing or registration methods (e.g., `AddSyncfusionLicense()` or similar) for Toolkit usage.
- Forget to register in split Web App scenarios where components exist in both Server and Client projects.
- Leave service registration incomplete; components will fail without it.

---

### C. Namespace Imports

Add required Toolkit namespaces to `_Imports.razor` (or component-specific using statements).

#### DO

- **Add to `_Imports.razor` the main Toolkit namespace**:
  ```razor
  @using Syncfusion.Blazor.Toolkit
  ```

- **For specific components, add component-specific namespaces** (only as needed):
  ```razor
  @using Syncfusion.Blazor.Toolkit.Buttons
  @using Syncfusion.Blazor.Toolkit.Dialogs
  @using Syncfusion.Blazor.Toolkit.Inputs
  @* Add other component namespaces only if you use those components *@
  ```

- **Import only the component namespaces** that match the Toolkit components you actually use.
- **For split Web App**: If both Server and Client use Toolkit components, add namespaces to both `_Imports.razor` files (in Server and Client projects).

#### DON'T

- Import commercial-only `Syncfusion.Blazor.*` namespaces (e.g., `Syncfusion.Blazor.Charts`, `Syncfusion.Blazor.Grids`) as a workaround for Toolkit.
- Assume components work without the main `Syncfusion.Blazor.Toolkit` namespace.

---

### D. Theme Stylesheet

Toolkit components require CSS theming. The Toolkit provides a **common (recommended) stylesheet** and optional component-specific stylesheets.

#### DO

- **Use the common stylesheet** (recommended for most applications):
  ```html
  <link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
  ```
  This single stylesheet provides styling for all Toolkit components.

- **Optionally, use individual component stylesheets** for better performance or reduced bundle size:
  ```html
  <link href="_content/Syncfusion.Blazor.Toolkit/styles/button.min.css" rel="stylesheet" />
  <link href="_content/Syncfusion.Blazor.Toolkit/styles/dialog.min.css" rel="stylesheet" />
  ```
  Available individual stylesheets: `button.min.css`, `calendar.min.css`, `chart.min.css`, `checkbox.min.css`, `dialog.min.css`, `dropdown.min.css`, `input.min.css`, `spinner.min.css`, `textbox.min.css`, `tooltip.min.css`, and more.

- **Place stylesheet links in the `<Head>` section** of `App.razor` (all project types: Blazor Server, WebAssembly, Web App)

- **Reference the correct path**: `_content/Syncfusion.Blazor.Toolkit/styles/` (note: `styles/` directory with `.min.css` extension)

#### DON'T

- Assume components render correctly without theme CSS (they will appear unstyled).
- Try to use other themes (e.g., `bootstrap5.css`, `tailwind.css`, `material.css`); Toolkit uses Fluent theme only via `fluent.min.css`.
- Link commercial Syncfusion script tags or non-Toolkit theme files.
- Use individual component stylesheets without understanding the performance tradeoff.

---

### E. Interactive Render Mode

For Blazor Web App projects or scenarios requiring component interactivity, configure interactive render modes.

#### DO

- **Understand the requirement**:
  - **Static SSR (Server-Static Rendering)** is sufficient for read-only Toolkit content.
  - **Interactive SSR or InteractiveWebAssembly** is required for event handlers, two-way binding, and dynamic updates.
- **For Blazor Web App**, configure components using the interactive render mode:
  ```razor
  @rendermode InteractiveServer
  ```
  or
  ```razor
  @rendermode InteractiveWebAssembly
  ```
- **On individual components** that require interactivity:
  ```razor
  <SfButton @onclick="OnClick" @rendermode="InteractiveServer">Click me</SfButton>

  @code {
      private void OnClick() { /* handler */ }
  }
  ```
- **Document why**: Static SSR cannot support event handlers or dynamic state; interactive mode bridges this gap.

#### DON'T

- Assume static SSR alone is sufficient for interactive Toolkit components.
- Leave render mode unspecified if components require clicks, forms, or dynamic updates.
- Claim Toolkit works in static SSR for interactive scenarios.

---

### F. JavaScript Loading

Toolkit components rely on JavaScript for functionality. The Toolkit handles this automatically.

#### DO

- Understand that **Toolkit JavaScript is loaded by the Toolkit package itself** when components are used.
- No manual script tag insertion is required for Toolkit.

#### DON'T

- Add commercial Syncfusion script tags (e.g., `syncfusion-ej2-...js`) for Toolkit.
- Manually inject JavaScript as a required Toolkit setup step.
- Assume Toolkit components work without JavaScript support in the browser.

---

## Common Failure Diagnosis

If Toolkit components are not rendering or behaving as expected, check the following in order:

### 1. Missing or Wrong Theme CSS

**Symptom**: Components render but appear unstyled (no colors, borders, or styling).

**Diagnosis**:
- Verify the Toolkit theme link is present in the `<Head>` section of `App.razor`.
- Confirm the path is correct: `_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css` (note: `styles/` directory, not `themes/`, and `fluent.min.css` exactly).
- Check browser DevTools Network tab for 404 or CORS errors on the CSS file.
- Verify you're using `fluent.min.css` (Toolkit only supports Fluent theme).

**Fix**:
```html
<!-- In App.razor <Head> section -->
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
```

**Common mistake**: Using paths like `_content/Syncfusion.Blazor.Toolkit/themes/bootstrap5.css` or `_content/Syncfusion.Blazor.Toolkit/styles/bootstrap5.min.css` (wrong directory, wrong theme). These will cause 404 errors.

### 2. Missing Service Registration

**Symptom**: Components throw a runtime error: "Services not configured" or similar.

**Diagnosis**:
- Verify `AddSyncfusionBlazorToolkit()` is called in `Program.cs` before `builder.Build()`.
- Check that the method name is spelled exactly as written (case-sensitive).

**Fix**:
```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorComponents();
builder.Services.AddSyncfusionBlazorToolkit(); // <-- Add this line
var app = builder.Build();
```

### 3. Static SSR Insufficient

**Symptom**: Components render but click events, form inputs, or dynamic updates don't work.

**Diagnosis**:
- Verify the component or page has an interactive render mode applied.
- Check if the component is marked `@rendermode InteractiveServer` or `@rendermode InteractiveWebAssembly`.
- Confirm the render mode is supported in your Blazor Web App setup.

**Fix**:
```razor
@page "/counter"
@rendermode InteractiveServer

<SfButton @onclick="IncrementCount">Click</SfButton>
<p>Count: @count</p>

@code {
    private int count = 0;
    private void IncrementCount() => count++;
}
```

### 4. Wrong Package / Commercial Path

**Symptom**: You've installed `Syncfusion.Blazor` (commercial) instead of the Toolkit, or licensing guidance is mentioned.

**Diagnosis**:
- Check `.csproj` file for the package name: should be `Syncfusion.Blazor.Toolkit`, not `Syncfusion.Blazor` or component-specific packages.
- Look for license-key setup or registration methods that don't exist in Toolkit.

**Fix**:
- Uninstall the wrong package:
  ```bash
  dotnet remove package Syncfusion.Blazor
  ```
- Install the Toolkit:
  ```bash
  dotnet add package Syncfusion.Blazor.Toolkit
  ```
- Replace any license-key code with `AddSyncfusionBlazorToolkit()`.

### 5. Split Blazor Web App Partial Registration

**Symptom**: Components work in Server but not in Client (or vice versa) in a split Web App.

**Diagnosis**:
- Verify `AddSyncfusionBlazorToolkit()` is registered in **both** `Program.cs` files (Server and Client) if both use Toolkit components.
- Check that both projects have the Toolkit NuGet package reference.

**Fix**:
- **Server/Program.cs**:
  ```csharp
  builder.Services.AddSyncfusionBlazorToolkit();
  ```
- **Client/Program.cs**:
  ```csharp
  builder.Services.AddSyncfusionBlazorToolkit();
  ```

### 6. JavaScript Loading or Script Failure

**Symptom**: Browser console shows JavaScript errors; components don't respond; errors mention Syncfusion or undefined functions.

**Diagnosis**:
- Open browser DevTools (F12) Console tab and look for JavaScript errors
- Check the Network tab to verify JavaScript files from `_content/Syncfusion.Blazor.Toolkit/` are loading
- Confirm `AddSyncfusionBlazorToolkit()` is called in `Program.cs` before components render
- Check if there are conflicting scripts or browser extensions interfering

**Fix**:
1. Ensure service registration happens early in `Program.cs`:
   ```csharp
   var builder = WebApplication.CreateBuilder(args);
   builder.Services.AddRazorComponents();
   builder.Services.AddSyncfusionBlazorToolkit(); // <-- Before Build()
   var app = builder.Build();
   ```
2. Clear browser cache and do a full rebuild:
   ```bash
   dotnet clean
   dotnet build
   ```
3. Reload the page (Ctrl+F5 for hard refresh)
4. Try in incognito mode to rule out extensions or cached issues
5. Check if package is correctly installed: `dotnet list package | grep Syncfusion.Blazor.Toolkit`

### 7. Wrong Theme Name Used

**Symptom**: Components render unstyled even though CSS link is present; no 404 errors in browser.

**Diagnosis**:
- Check the theme CSS filename in your host file
- The Toolkit supports **only Fluent theme** (`fluent.css`)
- Using other theme names (bootstrap5, tailwind, material) will not work

**Fix**:
Use the correct Fluent theme:
```html
<!-- Correct: Only Fluent is supported -->
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />

<!-- Wrong: These theme names are not supported for Toolkit -->
<!-- <link href="_content/Syncfusion.Blazor.Toolkit/styles/bootstrap5.min.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor.Toolkit/themes/fluent.css" rel="stylesheet" /> -->
```

---

## Validation Checklist

Before considering Toolkit installation complete, verify the following:

### DO Verify

1. **Package identity**: `.csproj` or command output confirms `Syncfusion.Blazor.Toolkit` is installed.
2. **Service registration**: `Program.cs` includes `AddSyncfusionBlazorToolkit()` call.
3. **Namespaces**: `_Imports.razor` (or component files) include `@using Syncfusion.Blazor` and component namespaces.
4. **Theme CSS**: The correct theme link is present in `App.razor`, `index.html`, or `_Host.cshtml`.
5. **Render mode** (if using Web App): Interactive render modes are applied where components require events.
6. **Split Web App** (if applicable): Both Server and Client `Program.cs` have service registration and both have theme links.
7. **No license keys**: No license-key registration code is present (not needed for Toolkit).
8. **No commercial packages**: No `Syncfusion.Blazor` or component-specific commercial packages are installed.

### DON'T Verify

- Treat commercial package presence as successful Toolkit setup.
- Consider installation complete without all five core steps (package, services, imports, theme, render mode).

---

## References

- [Package identity and splits](./references/package-identity.md)
- [Split Blazor Web App registration](./references/split-webapp-registration.md)
- [Theme and host files](./references/theme-and-host-files.md)
- [Render modes explained](./references/render-modes.md)
- [Troubleshooting guide](./references/troubleshooting.md)

---

## Summary

The Toolkit installation consists of five core steps:

1. **Install package**: `dotnet add package Syncfusion.Blazor.Toolkit`
2. **Register services**: Add `AddSyncfusionBlazorToolkit()` to `Program.cs`
3. **Import namespaces**: Add `@using Syncfusion.Blazor` to `_Imports.razor`
4. **Link theme CSS**: Add theme link in `App.razor` or `index.html`
5. **Configure render mode**: Apply `@rendermode InteractiveServer` for interactive components

After these steps, the Toolkit is ready to use.
