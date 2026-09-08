# Troubleshooting Toolkit Installation

## Problem: Components render unstyled

**Symptoms**:
- Components appear in the page but have no colors, borders, or styling
- DevTools shows missing CSS file (404 error on the theme link)

**Root cause**: Theme CSS is not linked in the app's host file, or the path/filename is incorrect.

**Diagnosis**:
1. Check the correct host file for the project type (`App.razor` for Blazor Web App / modern Blazor Server; `_Host.cshtml` for legacy Blazor Server; `wwwroot/index.html` for Blazor WebAssembly)
2. Look for a line like:
   ```html
   <link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
   ```
3. If missing, add it in the `<Head>` section
4. If present, verify:
   - Path is exactly `_content/Syncfusion.Blazor.Toolkit/styles/` (note: `styles/`, not `themes/`)
   - Filename is exactly `fluent.min.css` (Toolkit only supports Fluent theme)
   - No typos or wrong theme names (e.g., `bootstrap5.min.css`, `tailwind.min.css` won't work)
5. Open browser DevTools (F12) Network tab and check if the CSS file is loading (404 vs. 200 status)

**Common mistakes**:
- Using wrong path: `_content/Syncfusion.Blazor.Toolkit/themes/` (should be `styles/`)
- Using wrong theme: `bootstrap5.min.css`, `tailwind.min.css`, `material.min.css` (should be `fluent.min.css`)
- Missing `.min` extension: `fluent.css` (should be `fluent.min.css`)
- Linking in the wrong host file: use `App.razor` for Blazor Server/Web App or `wwwroot/index.html` for Blazor WebAssembly

**Fix**:
```html
<!-- In the <Head> section of App.razor -->
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
```

---

## Problem: "Services not configured" or NullReferenceException

**Symptoms**:
- Runtime error when component tries to use Toolkit services
- Error message mentions missing or null service

**Root cause**: `AddSyncfusionBlazorToolkit()` is not called in `Program.cs`.

**Diagnosis**:
1. Open `Program.cs` (or the Server/Client `Program.cs` in split Web Apps)
2. Look for:
   ```csharp
   builder.Services.AddSyncfusionBlazorToolkit();
   ```
3. If missing, add it before `builder.Build()`.

**Fix**:
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents();
builder.Services.AddSyncfusionBlazorToolkit();  // <-- Add this

var app = builder.Build();
```

---

## Problem: Click events or form inputs don't work

**Symptoms**:
- Toolkit component is visible and styled correctly
- Clicking buttons or entering text in forms has no effect
- No errors in the browser console

**Root cause**: Component is rendered in static SSR mode; interactive render mode is not applied.

**Diagnosis**:
1. Check the component or page definition:
   - Look for `@rendermode InteractiveServer` or `@rendermode InteractiveWebAssembly`
   - If missing, the component is static
2. Confirm the interaction type (Server or WebAssembly)

**Fix**:
```razor
@page "/mypage"
@rendermode InteractiveServer  <!-- Add this line -->

<SfButton @onclick="OnClick">Click me</SfButton>

@code {
    private void OnClick()
    {
        Console.WriteLine("Clicked!");
    }
}
```

Or for WebAssembly:
```razor
@page "/mypage"
@rendermode InteractiveWebAssembly  <!-- Or this -->

<SfButton @onclick="OnClick">Click me</SfButton>

@code {
    private void OnClick()
    {
        Console.WriteLine("Clicked!");
    }
}
```

---

## Problem: `Syncfusion.Blazor.Toolkit` namespace not found

**Symptoms**:
- Compilation error: "The type or namespace name 'Syncfusion' could not be found"
- IntelliSense doesn't show Toolkit types

**Root cause**: The NuGet package is not installed, or namespaces are not imported.

**Diagnosis**:
1. Check the `.csproj` file for:
   ```xml
   <PackageReference Include="Syncfusion.Blazor.Toolkit" Version="..." />
   ```
2. If missing, install the package.
3. Check `_Imports.razor` (or component file) for:
   ```razor
   @using Syncfusion.Blazor.Toolkit
   ```
4. If missing, add the namespace import.

**Fix**:
1. Install the package:
   ```bash
   dotnet add package Syncfusion.Blazor.Toolkit
   ```
2. Add to `_Imports.razor`:
   ```razor
   @using Syncfusion.Blazor.Toolkit
   ```

---

## Problem: Split Web App: components work in Server but not in Client

**Symptoms**:
- Toolkit components work fine in Server-side pages
- Same components in Client pages fail or don't render

**Root cause**: `AddSyncfusionBlazorToolkit()` is registered in Server `Program.cs` but not in Client `Program.cs`.

**Diagnosis**:
1. Check both `Program.cs` files (Server and Client)
2. Look for `AddSyncfusionBlazorToolkit()` in each
3. If only one has it, the other is missing registration

**Fix**:
- **Server/Program.cs**:
  ```csharp
  builder.Services.AddSyncfusionBlazorToolkit();
  ```
- **Client/Program.cs**:
  ```csharp
  builder.Services.AddSyncfusionBlazorToolkit();
  ```

Also ensure both projects have the NuGet package reference in their `.csproj` files and both have namespace imports in their `_Imports.razor` files.

---

## Problem: Wrong package installed (commercial Syncfusion instead of Toolkit)

**Symptoms**:
- You see `Syncfusion.Blazor` or `Syncfusion.Blazor.Buttons` in the `.csproj` instead of `Syncfusion.Blazor.Toolkit`
- Code references license keys or methods like `AddSyncfusionLicense()`
- Components work but you're on a commercial license

**Root cause**: The commercial Syncfusion.Blazor package was installed instead of the open-source Toolkit.

**Diagnosis**:
1. Check the `.csproj` file:
   ```xml
   <!-- Wrong -->
   <PackageReference Include="Syncfusion.Blazor" Version="..." />
   
   <!-- Correct -->
   <PackageReference Include="Syncfusion.Blazor.Toolkit" Version="..." />
   ```
2. Check `Program.cs` for license-key setup.

**Fix**:
1. Uninstall the commercial package:
   ```bash
   dotnet remove package Syncfusion.Blazor
   ```
2. Install the Toolkit:
   ```bash
   dotnet add package Syncfusion.Blazor.Toolkit
   ```
3. Remove any license-key registration code.
4. Add `AddSyncfusionBlazorToolkit()` instead.

---

## Problem: Theme CSS path 404 in browser DevTools

**Symptoms**:
- Browser DevTools shows 404 error for the theme CSS file
- Components are unstyled

**Root cause**: The theme CSS path is incorrect or the Toolkit package is not installed.

**Diagnosis**:
1. Verify the package is installed: `dotnet list package` and check if `Syncfusion.Blazor.Toolkit` is listed
2. Check the link path in the host file:
   - Should be: `_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css`
   - Not: `_content/Syncfusion.Blazor/themes/...` (that's commercial)
3. Verify the theme filename is correct (e.g., `fluent.min.css`, not `fluent.css`)

**Fix**:
1. Ensure package is installed:
   ```bash
   dotnet add package Syncfusion.Blazor.Toolkit
   ```
2. Verify the link is correct in the host file:
   ```html
   <link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />
   ```

---

## Problem: JavaScript errors in browser console

**Symptoms**:
- Browser DevTools Console shows JavaScript errors related to Toolkit or Syncfusion
- Components may not respond or may crash
- Errors mention "script", "undefined", or Syncfusion library functions

**Root cause**: Toolkit JavaScript is not loading correctly, or there's a conflict with browser scripts.

**Diagnosis**:
1. Open browser DevTools (F12) and check the Console tab
2. Look for errors that mention Syncfusion, Toolkit, or JS loading
3. Check the Network tab to verify JavaScript files are loading (look for `_content/` files)
4. If 404 errors appear, the package may not be installed correctly
5. If "undefined" errors appear, services may not be registered or JS is loading before services

**Fix**:
1. Ensure `AddSyncfusionBlazorToolkit()` is called in `Program.cs` before any components render
2. Verify the package is installed: `dotnet list package` and check if `Syncfusion.Blazor.Toolkit` is listed
3. Clear browser cache (Ctrl+Shift+Delete) and reload
4. Rebuild and republish: `dotnet build` and `dotnet publish`
5. Check that no other scripts are conflicting (disable extensions, try incognito mode)

---

## Problem: Theme CSS not loading (wrong theme name or path)

**Symptoms**:
- Components render unstyled even though you linked the CSS
- Browser shows no 404 errors on the CSS file
- The Toolkit supports **only Fluent theme**

**Root cause**: Using a theme other than `fluent.min.css` (e.g., `bootstrap5.min.css`, `tailwind.min.css`, etc.), or using wrong path/filename.

**Diagnosis**:
1. Check the CSS link in `App.razor` `<Head>` section
2. Verify the filename is exactly `fluent.min.css` (with `.min`)
3. Confirm the path is exactly `_content/Syncfusion.Blazor.Toolkit/styles/` (note: `styles/`, not `themes/`)

**Fix**:
```html
<!-- Correct: Fluent theme in styles directory with .min extension -->
<link href="_content/Syncfusion.Blazor.Toolkit/styles/fluent.min.css" rel="stylesheet" />

<!-- Wrong: Other themes are not supported -->
<!-- <link href="_content/Syncfusion.Blazor.Toolkit/styles/bootstrap5.min.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor.Toolkit/themes/fluent.css" rel="stylesheet" /> -->
<!-- <link href="_content/Syncfusion.Blazor.Toolkit/styles/tailwind.min.css" rel="stylesheet" /> -->
```

---

## Problem: Script fails to load during component initialization

**Symptoms**:
- Console shows errors like "Uncaught ReferenceError" or "undefined" when component renders
- Components appear briefly then disappear or show errors
- Toolkit JavaScript functions are not available

**Root cause**: Toolkit script files are not loading correctly, or services are not registered before components attempt to use them.

**Diagnosis**:
1. Open browser DevTools (F12) Network tab
2. Look for files in `_content/Syncfusion.Blazor.Toolkit/` directory
3. Check if any show 404 status (not found)
4. Verify `AddSyncfusionBlazorToolkit()` is registered **before** `builder.Build()` in `Program.cs`
5. Check if any page/component is rendering before services are initialized

**Fix**:
1. Ensure service registration is early in `Program.cs`:
   ```csharp
   var builder = WebApplication.CreateBuilder(args);
   builder.Services.AddRazorComponents()
       .AddInteractiveServerComponents();
   builder.Services.AddSyncfusionBlazorToolkit(); // <-- Must be before Build()
   var app = builder.Build();
   ```
2. Rebuild and clear cache:
   ```bash
   dotnet clean
   dotnet build
   ```
3. Clear browser cache (Ctrl+Shift+Delete) and reload (Ctrl+F5)
4. For split Web App, ensure **both** Server and Client `Program.cs` files have the registration
5. Verify the package is installed: `dotnet list package` and check if `Syncfusion.Blazor.Toolkit` is listed

---

## General Checklist

Before troubleshooting further, verify:

1. ✓ Package name is `Syncfusion.Blazor.Toolkit` (not commercial packages)
2. ✓ `AddSyncfusionBlazorToolkit()` is in `Program.cs` (both in split Web Apps)
3. ✓ `@using Syncfusion.Blazor.Toolkit` is in `_Imports.razor` (both in split Web Apps)
4. ✓ Theme CSS link is `fluent.min.css` in the correct host file (`App.razor` for Blazor Server/Web App or `wwwroot/index.html` for Blazor WebAssembly)
5. ✓ Theme filename is exactly `fluent.min.css` (not other theme names)
6. ✓ Interactive components have `@rendermode InteractiveServer`, `@rendermode InteractiveWebAssembly`, or `@rendermode InteractiveAuto`
7. ✓ No license-key registration code is present
8. ✓ Build completes without errors: `dotnet build`
9. ✓ Browser console shows no 404 or JavaScript errors
