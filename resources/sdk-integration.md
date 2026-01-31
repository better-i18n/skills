# SDK Integration

Integrate Better i18n with your React or Next.js application.

## Available SDKs

| Package | Framework | Features |
|---------|-----------|----------|
| `@better-i18n/next` | Next.js | App Router, SSR, middleware |
| `@better-i18n/use-intl` | React | Hooks, context, formatters |
| `@better-i18n/cli` | Any | Key extraction, sync |

## Next.js Integration

### Installation

```bash
npm install @better-i18n/next
# or
bun add @better-i18n/next
```

### Setup

**1. Create config file**

```typescript
// i18n.config.ts
export const i18nConfig = {
  projectId: 'proj_xxx',
  defaultLocale: 'en',
  locales: ['en', 'tr', 'de'],
};
```

**2. Add middleware**

```typescript
// middleware.ts
import { createI18nMiddleware } from '@better-i18n/next';
import { i18nConfig } from './i18n.config';

export default createI18nMiddleware(i18nConfig);

export const config = {
  matcher: ['/((?!api|_next|.*\\..*).*)'],
};
```

**3. Create layout**

```typescript
// app/[locale]/layout.tsx
import { getMessages } from '@better-i18n/next';
import { I18nProvider } from '@better-i18n/next/client';

export default async function LocaleLayout({
  children,
  params: { locale },
}) {
  const messages = await getMessages(locale);

  return (
    <I18nProvider locale={locale} messages={messages}>
      {children}
    </I18nProvider>
  );
}
```

**4. Use translations**

```typescript
// app/[locale]/page.tsx
'use client';

import { useTranslations } from '@better-i18n/next/client';

export default function HomePage() {
  const t = useTranslations('common');

  return (
    <div>
      <h1>{t('welcome')}</h1>
      <p>{t('description')}</p>
    </div>
  );
}
```

### Server Components

```typescript
// Server component - no 'use client'
import { getTranslations } from '@better-i18n/next';

export default async function ServerPage({ params: { locale } }) {
  const t = await getTranslations(locale, 'common');

  return <h1>{t('welcome')}</h1>;
}
```

### Generating Static Params

```typescript
// app/[locale]/layout.tsx
import { i18nConfig } from '@/i18n.config';

export function generateStaticParams() {
  return i18nConfig.locales.map((locale) => ({ locale }));
}
```

## React Integration

### Installation

```bash
npm install @better-i18n/use-intl
```

### Setup

```typescript
// App.tsx
import { IntlProvider, useIntl } from '@better-i18n/use-intl';

function App() {
  const [locale, setLocale] = useState('en');
  const [messages, setMessages] = useState({});

  useEffect(() => {
    fetch(`https://cdn.better-i18n.com/proj_xxx/${locale}.json`)
      .then((res) => res.json())
      .then(setMessages);
  }, [locale]);

  return (
    <IntlProvider locale={locale} messages={messages}>
      <MyApp />
    </IntlProvider>
  );
}
```

### Using Translations

```typescript
import { useTranslations } from '@better-i18n/use-intl';

function MyComponent() {
  const t = useTranslations('common');

  return (
    <div>
      <h1>{t('title')}</h1>
      <button>{t('buttons.submit')}</button>
    </div>
  );
}
```

### Namespaced Hook

```typescript
import { useTranslations } from '@better-i18n/use-intl';

function AuthPage() {
  const t = useTranslations('auth');

  return (
    <form>
      <h1>{t('login.title')}</h1>
      <input placeholder={t('login.email.placeholder')} />
      <button>{t('login.submit')}</button>
    </form>
  );
}
```

## Formatting

### Numbers

```typescript
const t = useTranslations('common');

// Format as currency
t('price', { amount: 99.99 });
// "Price: $99.99" or "Fiyat: ₺99,99"

// In translation file:
// "price": "Price: {amount, number, currency}"
```

### Dates

```typescript
t('lastUpdated', { date: new Date() });
// "Last updated: January 31, 2026"

// Translation:
// "lastUpdated": "Last updated: {date, date, long}"
```

### Pluralization

```typescript
t('items', { count: 5 });
// "5 items" or "5 öğe"

// Translation (ICU format):
// "items": "{count, plural, one {# item} other {# items}}"
```

### Lists

```typescript
t('languages', { items: ['English', 'Turkish', 'German'] });
// "English, Turkish, and German"

// Translation:
// "languages": "{items, list, conjunction}"
```

## Advanced Patterns

### Lazy Loading Namespaces

```typescript
import { useNamespace } from '@better-i18n/use-intl';

function CheckoutPage() {
  const { t, isLoading } = useNamespace('checkout');

  if (isLoading) return <Spinner />;

  return <h1>{t('title')}</h1>;
}
```

### Rich Text (HTML)

```typescript
// Translation: "terms": "By signing up, you agree to our <link>Terms of Service</link>"

t.rich('terms', {
  link: (chunks) => <a href="/terms">{chunks}</a>,
});
```

### Accessing Raw Messages

```typescript
import { useMessages } from '@better-i18n/use-intl';

function LanguageSwitcher() {
  const messages = useMessages();
  const availableLocales = Object.keys(messages._meta?.locales || {});

  return (
    <select>
      {availableLocales.map((locale) => (
        <option key={locale} value={locale}>
          {messages._meta.locales[locale].name}
        </option>
      ))}
    </select>
  );
}
```

## CLI Tool

### Extract Keys

Scan your codebase for translation keys:

```bash
npx @better-i18n/cli extract --src="src/**/*.tsx"
```

Output:
```
Found 150 translation keys
New keys: 12
Missing translations: 45 (Turkish), 89 (German)
```

### Sync with Platform

```bash
# Pull translations from Better i18n
npx @better-i18n/cli pull --locale=all

# Push new keys to Better i18n
npx @better-i18n/cli push --locale=en
```

### Validate Translations

```bash
npx @better-i18n/cli validate

# Checks:
# - Missing translations
# - Placeholder mismatches
# - Invalid ICU syntax
# - Unused keys
```

## TypeScript Support

### Generate Types

```bash
npx @better-i18n/cli generate-types
```

Creates:
```typescript
// generated/i18n.d.ts
declare module '@better-i18n/use-intl' {
  interface Messages {
    common: {
      welcome: string;
      buttons: {
        submit: string;
        cancel: string;
      };
    };
    auth: {
      login: {
        title: string;
        email: { placeholder: string };
      };
    };
  }
}
```

### Typed useTranslations

```typescript
const t = useTranslations('common');

t('welcome'); // ✅ Valid
t('buttons.submit'); // ✅ Valid
t('nonexistent'); // ❌ TypeScript error
```

## Troubleshooting

### "Missing translation" warnings

```typescript
// Suppress in development
<IntlProvider
  locale={locale}
  messages={messages}
  onError={(err) => {
    if (err.code !== 'MISSING_TRANSLATION') {
      console.error(err);
    }
  }}
>
```

### Hydration mismatch

Ensure locale is consistent between server and client:

```typescript
// Use cookies or URL, not navigator.language
const locale = cookies().get('locale')?.value || 'en';
```

### Bundle size

Use namespace-based code splitting:

```typescript
// Only loads 'checkout' namespace when needed
const CheckoutPage = lazy(() => import('./CheckoutPage'));
```
