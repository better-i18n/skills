# MCP Integration

Use Better i18n tools directly in your AI coding assistant.

## Overview

The Better i18n MCP (Model Context Protocol) server enables AI assistants like Claude to:
- Create and update translation keys
- Translate content with AI
- Manage project settings
- Query translation status

## Installation

### Claude Desktop

Add to your Claude Desktop config (`~/.config/claude/config.json` on macOS):

```json
{
  "mcpServers": {
    "better-i18n": {
      "command": "npx",
      "args": ["@better-i18n/mcp"],
      "env": {
        "BETTER_I18N_API_KEY": "your-api-key"
      }
    }
  }
}
```

### Claude Code

```bash
# Install globally
npm install -g @better-i18n/mcp

# Or run directly
npx @better-i18n/mcp
```

### Getting API Key

```
Dashboard → Settings → API Keys → Create Key

Scopes needed:
- projects:read
- keys:read
- keys:write
- translations:read
- translations:write
```

## Available Tools

### listProjects

List all projects in your workspace.

```json
{
  "tool": "listProjects"
}
```

**Response:**
```json
{
  "projects": [
    {
      "id": "proj_xxx",
      "name": "My App",
      "slug": "my-org/my-app",
      "sourceLanguage": "en",
      "targetLanguages": ["tr", "de", "fr"]
    }
  ]
}
```

### getProject

Get project details including languages and stats.

```json
{
  "tool": "getProject",
  "project": "my-org/my-app"
}
```

### listKeys

List translation keys with filtering.

```json
{
  "tool": "listKeys",
  "project": "my-org/my-app",
  "namespace": "common",
  "status": "draft",
  "language": "en",
  "search": "button",
  "limit": 50
}
```

**Parameters:**
| Param | Type | Description |
|-------|------|-------------|
| project | string | Project identifier (required) |
| namespace | string | Filter by namespace |
| status | string | `draft`, `reviewed`, `approved` |
| language | string | Language code for translations |
| search | string | Search in key names or values |
| limit | number | Max results (default: 50) |

### createKeys

Create new translation keys.

```json
{
  "tool": "createKeys",
  "project": "my-org/my-app",
  "keys": [
    {
      "key": "checkout.title",
      "namespace": "common",
      "sourceText": "Checkout"
    },
    {
      "key": "checkout.subtitle",
      "namespace": "common",
      "sourceText": "Complete your order"
    }
  ]
}
```

### updateKeys

Update translations for existing keys.

```json
{
  "tool": "updateKeys",
  "project": "my-org/my-app",
  "translations": [
    {
      "key": "checkout.title",
      "namespace": "common",
      "language": "tr",
      "text": "Ödeme",
      "status": "reviewed"
    },
    {
      "key": "checkout.title",
      "namespace": "common",
      "language": "de",
      "text": "Kasse"
    }
  ]
}
```

**Parameters per translation:**
| Param | Type | Description |
|-------|------|-------------|
| key | string | Key name (required) |
| namespace | string | Namespace (default: "default") |
| language | string | Language code (required) |
| text | string | Translation text (required) |
| isSource | boolean | `true` if updating source language |
| status | string | Set status after update |

### deleteKeys

Soft-delete translation keys.

```json
{
  "tool": "deleteKeys",
  "project": "my-org/my-app",
  "keys": [
    { "key": "deprecated.feature", "namespace": "common" }
  ]
}
```

### addLanguage

Add a new target language to project.

```json
{
  "tool": "addLanguage",
  "project": "my-org/my-app",
  "language": "ja",
  "name": "Japanese"
}
```

## Usage Examples

### Adding Keys While Coding

When creating a new component:

```
You: I'm creating a new checkout page. Create these translation keys:
- checkout.title: "Checkout"
- checkout.items: "Your items"
- checkout.total: "Total"
- checkout.placeOrder: "Place Order"

Claude: [Uses createKeys tool to add all keys]
```

### Translating Content

```
You: Translate all keys in the checkout namespace to Turkish and German

Claude: [Uses listKeys to find keys, then updateKeys to add translations]
```

### Checking Translation Status

```
You: What keys are missing Turkish translations?

Claude: [Uses listKeys with status filter to find untranslated keys]
```

### Batch Operations

```
You: Our button text changed from "Submit" to "Save". Update this
across all namespaces.

Claude: [Uses listKeys to find "Submit", then updateKeys to change to "Save"]
```

## Project Identifier Format

Always use the format: `organization-slug/project-slug`

```
✅ my-org/my-app
✅ acme-corp/dashboard
❌ proj_xxx (use slug, not ID)
❌ my-app (need org prefix)
```

Find your project slug:
```
Dashboard → Project → Settings → General → Project Slug
```

## Error Handling

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `PROJECT_NOT_FOUND` | Invalid project identifier | Check slug format |
| `UNAUTHORIZED` | Invalid or expired API key | Generate new key |
| `KEY_EXISTS` | Creating duplicate key | Use updateKeys instead |
| `LANGUAGE_NOT_FOUND` | Invalid language code | Use addLanguage first |
| `RATE_LIMITED` | Too many requests | Wait and retry |

### Error Response Format

```json
{
  "error": {
    "code": "KEY_EXISTS",
    "message": "Key 'common.title' already exists",
    "details": {
      "key": "common.title",
      "namespace": "common"
    }
  }
}
```

## Best Practices

1. **Use descriptive keys** - AI can suggest better translations with context
2. **Batch operations** - Create/update multiple keys in one call
3. **Check before create** - Use listKeys to avoid duplicates
4. **Set status explicitly** - Don't rely on defaults for workflow
5. **Use namespaces** - Organize keys logically for easier management

## Debugging

### Enable Verbose Logging

```bash
DEBUG=better-i18n:* npx @better-i18n/mcp
```

### Test Connection

```json
{
  "tool": "listProjects"
}
```

If this returns your projects, MCP is configured correctly.

### Check API Key Scopes

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.better-i18n.com/v1/me
```

Response shows your key's permissions.
