# Key Management

Organize translation keys for maintainability and scalability.

## Key Naming Conventions

### Use Dot Notation

Structure keys hierarchically using dots:

```
✅ Good
common.buttons.submit
auth.login.title
auth.login.errors.invalidEmail
dashboard.widgets.revenue.title

❌ Bad
submitButton
loginTitle
invalid_email_error
revenue-widget-title
```

### Be Descriptive, Not Generic

```
✅ Good
checkout.payment.cardNumber.label
checkout.payment.cardNumber.placeholder
checkout.payment.cardNumber.error.invalid

❌ Bad
label1
input_placeholder
error_message
```

### Use Context Prefixes

```
page.home.hero.title          # Page-specific
component.navbar.links.about  # Component-specific
common.actions.save           # Shared across app
email.welcome.subject         # Email templates
error.network.timeout         # Error messages
```

## Namespaces

Namespaces group related keys and enable code-splitting.

### When to Use Namespaces

| Namespace | Use For |
|-----------|---------|
| `common` | Shared UI: buttons, labels, errors |
| `auth` | Login, signup, password reset |
| `dashboard` | Dashboard-specific content |
| `settings` | User/app settings |
| `emails` | Email templates |
| `errors` | Error messages |
| `validation` | Form validation messages |

### File Structure Examples

**Flat (Single File)**
```
locales/
├── en.json
└── tr.json
```

**Namespaced (Multiple Files)**
```
locales/
├── en/
│   ├── common.json
│   ├── auth.json
│   └── dashboard.json
└── tr/
    ├── common.json
    ├── auth.json
    └── dashboard.json
```

**Nested (Single File, Namespace as Root Keys)**
```json
// locales/en.json
{
  "common": {
    "buttons": { "submit": "Submit", "cancel": "Cancel" }
  },
  "auth": {
    "login": { "title": "Sign In" }
  }
}
```

## Translation Statuses

Each translation has a status:

| Status | Meaning | Action Needed |
|--------|---------|---------------|
| `draft` | Initial translation | Review required |
| `reviewed` | Human reviewed | Ready for approval |
| `approved` | Ready for production | Can be published |
| `rejected` | Needs revision | Re-translate |

### Status Workflow

```
[New Key] → draft → reviewed → approved → [Published]
                ↓
            rejected → draft (revised)
```

## Bulk Operations

### Creating Multiple Keys

```json
{
  "tool": "createKeys",
  "project": "my-org/my-app",
  "keys": [
    { "key": "nav.home", "namespace": "common", "sourceText": "Home" },
    { "key": "nav.about", "namespace": "common", "sourceText": "About" },
    { "key": "nav.contact", "namespace": "common", "sourceText": "Contact" }
  ]
}
```

### Updating Multiple Translations

```json
{
  "tool": "updateKeys",
  "project": "my-org/my-app",
  "translations": [
    { "key": "nav.home", "language": "tr", "text": "Ana Sayfa" },
    { "key": "nav.about", "language": "tr", "text": "Hakkinda" },
    { "key": "nav.contact", "language": "tr", "text": "Iletisim" }
  ]
}
```

## Key Search and Filtering

### Search Patterns

```
# By prefix
auth.*                    # All auth keys
dashboard.widgets.*       # All widget keys

# By status
status:draft              # Untranslated keys
status:approved           # Production-ready

# By language completion
missing:tr                # Missing Turkish translations
missing:de,fr             # Missing German OR French

# Combined
auth.* status:draft       # Draft auth keys
```

### Using MCP listKeys

```json
{
  "tool": "listKeys",
  "project": "my-org/my-app",
  "namespace": "common",
  "status": "draft",
  "language": "en",
  "limit": 50
}
```

## Deleting Keys

Keys are soft-deleted (can be recovered).

```json
{
  "tool": "deleteKeys",
  "project": "my-org/my-app",
  "keys": [
    { "key": "deprecated.oldFeature", "namespace": "common" }
  ]
}
```

## Best Practices

### DO

- Use consistent naming patterns across the project
- Group related keys in namespaces
- Keep keys short but descriptive
- Use lowercase with dots as separators
- Document key purposes in comments (source files)

### DON'T

- Use spaces or special characters in keys
- Create deeply nested keys (max 4 levels)
- Duplicate keys across namespaces
- Use sequential names like `item1`, `item2`
- Mix naming conventions in same project

## Migrating Existing Keys

If restructuring your key hierarchy:

1. Export current translations
2. Create mapping file (old → new)
3. Update source code references
4. Import with new structure
5. Verify all keys resolve correctly

```javascript
// Migration mapping
const keyMigration = {
  'submitBtn': 'common.buttons.submit',
  'loginTitle': 'auth.login.title',
  'errorMsg': 'common.errors.generic'
};
```
