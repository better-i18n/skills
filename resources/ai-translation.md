# AI Translation

Translate content with context-aware AI assistance.

## Overview

Better i18n uses AI to provide intelligent translations that understand:
- Your product context and terminology
- Brand voice and tone
- Technical terms via glossary
- Previous translation patterns

## AI Translation Workflow

```
[Source Text] → [Glossary Lookup] → [Context Analysis] → [AI Translation]
                                                              ↓
                                                    [Human Review]
                                                              ↓
                                                      [Approved]
```

## Using the AI Drawer

The AI Drawer is the primary interface for AI-assisted translation.

### Opening the Drawer

- Click "Better AI" button in header
- Or press `Cmd/Ctrl + I`

### Translation Commands

**Translate selected keys:**
```
Translate these keys to Turkish
```

**Translate with context:**
```
Translate to German. This is for a formal business app, use Sie form.
```

**Batch translation:**
```
Translate all untranslated keys in the auth namespace to French
```

### AI Understands Context

The AI considers:
- Surrounding keys in same namespace
- Previously approved translations
- Glossary terms
- Key naming patterns

## Glossary

The glossary ensures consistent terminology across translations.

### Adding Terms

```
Dashboard → Project Settings → Glossary → Add Term

Term: "workspace"
Translations:
  - tr: "çalışma alanı" (not "işyeri")
  - de: "Arbeitsbereich"
  - fr: "espace de travail"

Note: Technical term, always translate consistently
```

### Glossary Structure

```json
{
  "terms": [
    {
      "term": "workspace",
      "caseSensitive": false,
      "translations": {
        "tr": "çalışma alanı",
        "de": "Arbeitsbereich"
      },
      "notes": "Product-specific term"
    },
    {
      "term": "API",
      "caseSensitive": true,
      "doNotTranslate": true,
      "notes": "Keep as-is in all languages"
    }
  ]
}
```

### Do Not Translate List

Some terms should remain in English:

```
Brand names: "Better i18n", "GitHub"
Technical: "API", "JSON", "webhook"
Product features: "AI Drawer", "MCP"
```

## Translation Quality

### Review Process

1. **AI generates draft** - Initial translation
2. **Human reviews** - Check for accuracy, tone
3. **Approve or reject** - Move to production or revise

### Quality Indicators

| Indicator | Meaning |
|-----------|---------|
| ✅ Glossary match | Uses correct terminology |
| ⚠️ Length warning | Translation significantly longer/shorter |
| 🔄 Pattern match | Similar to approved translations |
| ❓ Low confidence | AI uncertain, needs human review |

## Batch Translation

### Via Dashboard

1. Select multiple keys (checkbox)
2. Click "Translate with AI"
3. Choose target language(s)
4. Review and approve

### Via MCP

```json
{
  "tool": "translateKeys",
  "project": "my-org/my-app",
  "keys": ["common.buttons.*"],
  "targetLanguages": ["tr", "de"],
  "useGlossary": true
}
```

### Via AI Chat

```
Translate all keys in the "checkout" namespace to Spanish and French.
Use formal tone. Mark payment-related terms from glossary.
```

## Context Hints

Provide context to improve translation quality:

### In Key Names

```
auth.login.button.submit        # Clear it's a button
email.welcome.subject           # Email subject line
error.validation.email.invalid  # Error message context
```

### In Source Text

```json
{
  "checkout.total": "Total: {amount}",
  "items.count": "{count, plural, one {# item} other {# items}}"
}
```

### In AI Prompts

```
These are error messages shown to users when form validation fails.
Keep them friendly but clear. Max 100 characters.
```

## Handling Special Content

### Variables and Placeholders

AI preserves placeholders automatically:

```
Source: "Welcome, {name}!"
AI Output (Turkish): "Hoş geldin, {name}!"
```

### Pluralization

ICU MessageFormat is preserved:

```
Source: "{count, plural, one {# item} other {# items}}"
AI Output (Turkish): "{count, plural, one {# öğe} other {# öğe}}"
```

### HTML/Markdown

Formatting tags are preserved:

```
Source: "Click <strong>here</strong> to continue"
AI Output: "Devam etmek için <strong>buraya</strong> tıklayın"
```

## Tips for Better AI Translations

1. **Set up glossary first** - Before bulk translation
2. **Provide context** - In prompts or key names
3. **Review samples** - Check a few before batch approve
4. **Use consistent source** - Clear, grammatically correct English
5. **Avoid idioms** - They don't translate well
6. **Keep it simple** - Shorter sentences = better translations
