---
name: i18n-best-practices
description: Use when building internationalization features, managing translation keys, setting up localization workflows, integrating AI translation, connecting GitHub repositories, delivering translations via CDN, using MCP tools for translation management, or implementing i18n SDKs in React/Next.js applications.
---

# i18n Best Practices

Guidance for building scalable, maintainable internationalization systems with Better i18n.

## Architecture Overview

```
[Source Code] → [Key Extraction] → [Better i18n Platform]
                                           ↓
                              ┌────────────┼────────────┐
                              ↓            ↓            ↓
                        [Dashboard]    [MCP Tools]  [AI Agent]
                              ↓            ↓            ↓
                              └────────────┼────────────┘
                                           ↓
                                    [Translations]
                                           ↓
                    ┌──────────────────────┼──────────────────────┐
                    ↓                      ↓                      ↓
              [GitHub PR]            [CDN Delivery]          [API Access]
                    ↓                      ↓                      ↓
              [Code Review]          [Edge Cached]           [Real-time]
                    ↓                      ↓                      ↓
                    └──────────────────────┼──────────────────────┘
                                           ↓
                                    [Your App]
                                           ↓
                              ┌────────────┼────────────┐
                              ↓            ↓            ↓
                         [React]      [Next.js]     [Other]
```

## Quick Reference

| Need to... | See |
|------------|-----|
| Set up a new i18n project | [Getting Started](./resources/getting-started.md) |
| Organize translation keys and namespaces | [Key Management](./resources/key-management.md) |
| Translate content with AI assistance | [AI Translation](./resources/ai-translation.md) |
| Sync translations with GitHub repository | [GitHub Sync](./resources/github-sync.md) |
| Serve translations via CDN | [CDN Delivery](./resources/cdn-delivery.md) |
| Use MCP tools in your IDE/agent | [MCP Integration](./resources/mcp-integration.md) |
| Integrate with React or Next.js | [SDK Integration](./resources/sdk-integration.md) |
| Structure keys, handle plurals, formatting | [Best Practices](./resources/best-practices.md) |

## Start Here

**New project?**
Start with [Getting Started](./resources/getting-started.md) to create your project and add languages. Then follow [Key Management](./resources/key-management.md) to organize your translation structure before adding content.

**Existing codebase?**
Connect your GitHub repository using [GitHub Sync](./resources/github-sync.md) to automatically import existing translation files. The platform will detect your file format and namespace structure.

**Need translations fast?**
Use [AI Translation](./resources/ai-translation.md) to translate content with context-aware AI. Set up a glossary first to ensure consistent terminology across all translations.

**Building with React/Next.js?**
Check [SDK Integration](./resources/sdk-integration.md) for framework-specific setup. Use the `@better-i18n/next` package for Next.js or `@better-i18n/use-intl` for React.

**Using AI coding assistants?**
Install the MCP server via [MCP Integration](./resources/mcp-integration.md). Your agent can then create, update, and manage translations directly from your IDE.

**Production deployment?**
Set up [CDN Delivery](./resources/cdn-delivery.md) for edge-cached translations with automatic fallbacks. Combine with [GitHub Sync](./resources/github-sync.md) for version-controlled deployments.
