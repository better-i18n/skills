```
  ╔══════════════════════════════════════════╗
  ║   _  _  ___        _  _   _              ║
  ║  (_)/ |( _ ) _ __ | || | | |             ║
  ║  | || |/ _ \| '_ \| || |_| |             ║
  ║  | || | (_) | | | |__   _|_|             ║
  ║  |_||_|\___/|_| |_|  |_| (_)             ║
  ║                                          ║
  ║           Best Practices                 ║
  ╚══════════════════════════════════════════╝
```

# i18n Best Practices Skill

A comprehensive agent skill for building production-ready internationalization systems. Covers everything from key management to AI translation, with a focus on scalability, maintainability, and developer experience.

## Installation

```bash
npx skills add better-i18n/i18n-best-practices
```

## What This Skill Covers

### Getting Started
- Creating projects and organizations
- Setting up source and target languages
- Choosing between CDN-first and GitHub-connected workflows

### Key Management
- Naming conventions and namespaces
- Translation statuses and workflows
- Bulk operations and search patterns

### Translation
- AI-assisted translation with context
- Glossary management for consistent terminology
- Quality review and approval workflows

### Integration
- GitHub sync for version-controlled translations
- CDN delivery for edge-cached content
- MCP tools for AI coding assistants
- React and Next.js SDK integration

### Best Practices
- ICU MessageFormat for plurals, dates, numbers
- RTL language support
- Accessibility considerations
- Performance optimization

## Structure

```
i18n-best-practices/
├── SKILL.md                              # Start here - routes to the right resource
└── resources/
    ├── getting-started.md                # Project setup, configuration
    ├── key-management.md                 # Naming, namespaces, organization
    ├── ai-translation.md                 # AI translation, glossary
    ├── github-sync.md                    # Repository integration, PRs
    ├── cdn-delivery.md                   # Edge caching, versioning
    ├── mcp-integration.md                # MCP tools for AI assistants
    ├── sdk-integration.md                # React, Next.js integration
    └── best-practices.md                 # Plurals, formatting, RTL
```

## Quick Start

Open [SKILL.md](./SKILL.md) - it has a routing table that directs you to the right resource based on what you need to do.

## Related Links

- [Better i18n Documentation](https://docs.better-i18n.com)
- [Better i18n Dashboard](https://better-i18n.com)
- [MCP Server Package](https://npmjs.com/package/@better-i18n/mcp)
- [Next.js SDK](https://npmjs.com/package/@better-i18n/next)
- [React SDK](https://npmjs.com/package/@better-i18n/use-intl)

## License

MIT
