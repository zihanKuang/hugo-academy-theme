Reusable hugo theme module that powers the layer5 academy .
this modules provides the layouts , shortcodes and partials to get academy up and running

The recommended approach to use this theme is through the academy starter template .

## Getting started with this theme


The recommended approach to use this theme is through the academy starter template .


## 📄 `tenant-context.html` (Required Partial)

**Location:** `layouts/partials/tenant-context.html`
**Purpose:** Provides a hardcoded tenant UUID (`orgId`) for shortcodes and templates to use. This enables tenant-aware content in a multi-tenant Hugo theme.

---

### 📌 Why this is required

This partial is **mandatory** when using this theme. Many of the theme’s built-in shortcodes (`usestatic`, `useshortcode`, etc.) internally depend on this file to fetch the correct tenant UUID.

Each tenant must **hardcode their own UUID** in this file to properly namespace assets, shortcodes, and logic.


```gohtml
{{/*
  Partial: tenant-context
  Required by: usestatic, useshortcode, and other reusable shortcodes.
  Description:
    Hardcoded context for the current tenant.
    This file must be customized per tenant site to return the correct UUID/orgId.

  Usage in shortcodes or templates:
    {{ $ctx := partial "tenant-context.html" . }}
    {{ $uuid := $ctx.uuid }}
*/}}

{{- dict "uuid" "123e4567-e89b-12d3-a456-426614174000" -}}
```

> 🔐 Replace `"123e4567-e89b-12d3-a456-426614174000"` with your actual tenant UUID.

---

### 🔁 Used By

| Theme Shortcode | Purpose                                |
| --------------- | -------------------------------------- |
| `usestatic`     | Resolves static asset paths per tenant |
| `useshortcode`  | Renders tenant-specific shortcodes     |
| Others          | Custom logic scoped by tenant          |

---

### 🚨 Required per Tenant

Each tenant site **must define** this file in their local `layouts/partials/` directory. Without it, most tenant-aware theme features will not work.

---
