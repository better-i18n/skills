# MCP Integration

Use Better i18n tools directly in your AI coding assistant.

## Overview

The Better i18n MCP (Model Context Protocol) server enables AI assistants like Claude to:
- Query and search translation keys
- Create and update translations
- Translate content with AI using glossary context
- Delete obsolete keys
- Manage translation workflows

## Installation

### Claude Desktop

Add to your Claude Desktop config (`~/Library/Application Support/Claude/claude_desktop_config.json` on macOS):

```json
{
  "mcpServers": {
    "better-i18n": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/better-i18n-mcp@latest"],
      "env": {
        "BETTER_I18N_API_KEY": "your-api-key"
      }
    }
  }
}
```

### Getting API Key

1. Go to Better i18n Dashboard
2. Navigate to **Settings → API Keys**
3. Create a new key with required scopes:
   - `projects:read`
   - `keys:read`
   - `keys:write`
   - `translations:read`
   - `translations:write`

## Available Tools

### getTranslations

Search and retrieve translations with powerful filtering.

```
Parameters:
- projectId: Project ID (required)
- status: Filter by status (all, draft, reviewed, approved)
- namespace: Filter by namespace
- search: Search in key names
- searchValue: Search in translation values
- searchLanguage: Language to search values in
- limit: Max results (default: 50)
```

**Example usage:**
```
"Find all draft translations in the auth namespace"
→ Uses getTranslations with status: "draft", namespace: "auth"

"Search for translations containing 'login' in Turkish"
→ Uses getTranslations with searchValue: "login", searchLanguage: "tr"
```

### getKeyDetails

Get detailed information about specific keys including all translations.

```
Parameters:
- projectId: Project ID (required)
- keyNames: Array of key names to retrieve
- namespace: Namespace (default: "default")
```

**Returns:**
- Key metadata (created, updated dates)
- All translations across languages
- Translation statuses
- Context and notes

### updateTranslations

Update translations for existing keys.

```
Parameters:
- projectId: Project ID (required)
- translations: Array of translation updates
  - key: Key name
  - namespace: Namespace
  - language: Target language code
  - value: Translation text
  - status: Optional status (draft, reviewed, approved)
```

**Example:**
```json
{
  "translations": [
    {
      "key": "auth.login.title",
      "namespace": "common",
      "language": "tr",
      "value": "Giriş Yap",
      "status": "reviewed"
    }
  ]
}
```

### createKeys

Create new translation keys with source text.

```
Parameters:
- projectId: Project ID (required)
- keys: Array of keys to create
  - key: Key name
  - namespace: Namespace
  - sourceText: Source language text
```

**Example:**
```json
{
  "keys": [
    {
      "key": "checkout.title",
      "namespace": "common",
      "sourceText": "Checkout"
    },
    {
      "key": "checkout.subtitle",
      "namespace": "common",
      "sourceText": "Complete your purchase"
    }
  ]
}
```

### deleteKeys

Soft-delete translation keys (can be recovered).

```
Parameters:
- projectId: Project ID (required)
- keys: Array of keys to delete
  - key: Key name
  - namespace: Namespace
```

### translateKeys

AI-powered translation with glossary support.

```
Parameters:
- projectId: Project ID (required)
- keys: Array of key names or patterns (e.g., "auth.*")
- targetLanguages: Array of language codes
- useGlossary: Boolean (default: true)
- context: Optional context for better translations
```

**Example:**
```
"Translate all checkout keys to German and French"
→ Uses translateKeys with keys: ["checkout.*"], targetLanguages: ["de", "fr"]
```

## Usage Patterns

### Adding Keys While Coding

When building a new feature:

```
You: I'm creating a checkout page. Create these translation keys:
- checkout.title: "Checkout"
- checkout.items: "Your items"
- checkout.total: "Total"
- checkout.placeOrder: "Place Order"

Claude: [Uses createKeys to add all keys to the project]
```

### Translating Content

```
You: Translate the checkout namespace to Turkish

Claude: [Uses getTranslations to find keys, then translateKeys to generate translations]
```

### Checking Translation Status

```
You: What keys are missing Turkish translations?

Claude: [Uses getTranslations with status filter to find untranslated keys]
```

### Updating Existing Translations

```
You: The "Submit" button text should be "Save Changes" instead

Claude: [Uses getTranslations to find keys with "Submit", then updateTranslations]
```

## Project Identifier

Use the project ID from your dashboard:

```
Dashboard → Project → Settings → Project ID
```

Format: `proj_xxxxxxxxxxxx`

## Error Handling

### Common Errors

| Error | Cause | Fix |
|-------|-------|-----|
| `PROJECT_NOT_FOUND` | Invalid project ID | Check project ID in dashboard |
| `UNAUTHORIZED` | Invalid or expired API key | Generate new key |
| `KEY_EXISTS` | Creating duplicate key | Use updateTranslations instead |
| `LANGUAGE_NOT_FOUND` | Invalid language code | Add language to project first |
| `RATE_LIMITED` | Too many requests | Wait and retry |

## Best Practices

1. **Batch operations** - Create/update multiple keys in one call
2. **Use search** - Find keys before creating to avoid duplicates
3. **Set status explicitly** - Mark translations as reviewed/approved
4. **Provide context** - Use the context parameter for better AI translations
5. **Use namespaces** - Organize keys logically (auth, common, checkout, etc.)

## Debugging

### Test Connection

Ask Claude to list your projects:

```
"List my Better i18n projects"
```

If this returns your projects, MCP is configured correctly.

### Verbose Logging

```bash
DEBUG=better-i18n:* npx @anthropic-ai/better-i18n-mcp
```
