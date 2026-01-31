# Getting Started with Better i18n

Set up internationalization for your application in minutes.

## Overview

Better i18n is a GitHub-first localization platform that combines AI translation with human approval workflows. This guide covers initial setup and configuration.

## Prerequisites

- A Better i18n account (sign up at [better-i18n.com](https://better-i18n.com))
- Your application codebase (React, Next.js, or any JavaScript framework)
- (Optional) GitHub repository for sync integration

## Quick Start

### 1. Create an Organization

Organizations are workspaces that contain your projects and team members.

```
Dashboard → Create Organization → Enter name → Create
```

### 2. Create a Project

Each project represents one application or product.

```
Organization → New Project → Configure:
  - Name: "My App"
  - Source Language: English (en)
  - Target Languages: Turkish (tr), German (de), etc.
```

### 3. Choose Your Workflow

**CDN-First (Recommended for new projects)**
- Upload JSON files directly
- Edit in dashboard
- Publish to CDN instantly
- No GitHub required

**GitHub-Connected**
- Sync with your repository
- Changes create PRs
- Version-controlled translations
- Team code review workflow

## Project Configuration

### Source Language

The language your developers write content in. Usually English.

```json
{
  "sourceLanguage": "en",
  "sourceFiles": "locales/en/**/*.json"
}
```

### Target Languages

Languages you want to translate into.

```json
{
  "targetLanguages": ["tr", "de", "fr", "es", "ja"],
  "targetFiles": "locales/{locale}/**/*.json"
}
```

### File Format Detection

Better i18n automatically detects your translation file structure:

| Format | Example Path | Structure |
|--------|-------------|-----------|
| Flat JSON | `/locales/en.json` | `{"key": "value"}` |
| Namespaced | `/locales/en/common.json` | Filename = namespace |
| Nested | `/locales/en.json` | `{"namespace": {"key": "value"}}` |

## Adding Your First Translations

### Via Dashboard

1. Navigate to your project
2. Click "Add Key"
3. Enter key name: `welcome.title`
4. Enter source text: "Welcome to our app"
5. Save

### Via MCP Tools

If using Claude or another AI assistant with MCP:

```json
{
  "tool": "createKeys",
  "project": "my-org/my-app",
  "keys": [
    {
      "key": "welcome.title",
      "namespace": "common",
      "sourceText": "Welcome to our app"
    }
  ]
}
```

### Via API

```bash
curl -X POST https://api.better-i18n.com/v1/keys \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "projectId": "proj_xxx",
    "keys": [{"key": "welcome.title", "text": "Welcome to our app"}]
  }'
```

## Next Steps

- [Key Management](./key-management.md) - Organize your translation structure
- [AI Translation](./ai-translation.md) - Translate content with AI
- [SDK Integration](./sdk-integration.md) - Connect your React/Next.js app

## Common Issues

### "Project not found"

Ensure you're using the correct project identifier format: `organization-slug/project-slug`

### "Unauthorized"

Check your API key has the correct scopes. Generate a new key from Dashboard → Settings → API Keys.

### "Invalid file format"

Better i18n expects valid JSON. Validate your files:

```bash
cat locales/en.json | jq .
```
