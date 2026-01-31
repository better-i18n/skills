# CDN Delivery

Serve translations globally with edge caching.

## Overview

Better i18n CDN provides:
- Global edge distribution (300+ locations)
- Automatic caching and invalidation
- Fallback language support
- Version pinning for deployments

## CDN URL Structure

```
https://cdn.better-i18n.com/{project_id}/{locale}/{namespace}.json
```

### Examples

```bash
# All translations for English
https://cdn.better-i18n.com/proj_xxx/en.json

# Specific namespace
https://cdn.better-i18n.com/proj_xxx/en/common.json

# With version pinning
https://cdn.better-i18n.com/proj_xxx/en.json?v=1.2.3
```

## Publishing Workflow

```
[Draft] → [Reviewed] → [Approved] → [Publish] → [CDN Updated]
```

### Manual Publish

```
Dashboard → Project → Publish → Select languages → Publish
```

### Automatic Publish

Configure auto-publish when translations are approved:

```
Project Settings → Publishing → Auto-publish approved: On
```

## CDN Configuration

### Cache Settings

```
Project Settings → CDN → Cache

TTL: 5 minutes (default)
Stale-while-revalidate: 1 hour
Cache-Control: public, max-age=300, stale-while-revalidate=3600
```

### Custom Domain

```
Project Settings → CDN → Custom Domain

Domain: i18n.yourapp.com
SSL: Automatic (Let's Encrypt)
```

After setup:
```
https://i18n.yourapp.com/en/common.json
```

## Fetching Translations

### Browser (Fetch API)

```javascript
async function loadTranslations(locale, namespace = 'common') {
  const response = await fetch(
    `https://cdn.better-i18n.com/proj_xxx/${locale}/${namespace}.json`
  );
  return response.json();
}

const messages = await loadTranslations('tr', 'common');
```

### With Fallback

```javascript
async function loadWithFallback(locale, namespace) {
  try {
    const response = await fetch(
      `https://cdn.better-i18n.com/proj_xxx/${locale}/${namespace}.json`
    );
    if (!response.ok) throw new Error('Not found');
    return response.json();
  } catch {
    // Fallback to English
    const fallback = await fetch(
      `https://cdn.better-i18n.com/proj_xxx/en/${namespace}.json`
    );
    return fallback.json();
  }
}
```

### Server-Side (Node.js)

```javascript
import { createClient } from '@better-i18n/client';

const i18n = createClient({
  projectId: 'proj_xxx',
  defaultLocale: 'en',
});

// Fetches from CDN with caching
const messages = await i18n.getMessages('tr', 'common');
```

## Version Pinning

Pin to specific versions for deployments:

### By Version Number

```
https://cdn.better-i18n.com/proj_xxx/en.json?v=1.2.3
```

### By Commit SHA

```
https://cdn.better-i18n.com/proj_xxx/en.json?ref=abc123
```

### By Timestamp

```
https://cdn.better-i18n.com/proj_xxx/en.json?ts=1704067200
```

## Response Format

### Single Namespace

```json
{
  "welcome": "Hoş geldiniz",
  "goodbye": "Güle güle",
  "buttons": {
    "submit": "Gönder",
    "cancel": "İptal"
  }
}
```

### All Namespaces

```json
{
  "common": {
    "welcome": "Hoş geldiniz"
  },
  "auth": {
    "login": "Giriş yap"
  }
}
```

### With Metadata

```
GET /proj_xxx/en.json?meta=true
```

```json
{
  "_meta": {
    "locale": "en",
    "version": "1.2.3",
    "publishedAt": "2026-01-31T12:00:00Z",
    "keyCount": 150
  },
  "welcome": "Welcome",
  ...
}
```

## Caching Strategy

### CDN Edge Cache

- First request: Cache MISS, fetch from origin
- Subsequent requests: Cache HIT (within TTL)
- After publish: Cache invalidated globally

### Browser Cache

Recommended headers for client-side caching:

```javascript
// Short cache, quick updates
Cache-Control: public, max-age=60, stale-while-revalidate=300

// Longer cache, version pinned
Cache-Control: public, max-age=86400, immutable
```

### Service Worker

```javascript
// Cache translations for offline use
self.addEventListener('fetch', (event) => {
  if (event.request.url.includes('cdn.better-i18n.com')) {
    event.respondWith(
      caches.match(event.request).then((cached) => {
        return cached || fetch(event.request).then((response) => {
          const clone = response.clone();
          caches.open('i18n').then((cache) => cache.put(event.request, clone));
          return response;
        });
      })
    );
  }
});
```

## Monitoring

### CDN Analytics

```
Dashboard → Project → Analytics

Metrics:
- Requests per locale
- Cache hit rate
- Bandwidth usage
- Error rate
```

### Health Check

```bash
curl -I https://cdn.better-i18n.com/proj_xxx/en.json

HTTP/2 200
cache-status: HIT
x-cache-age: 120
content-type: application/json
```

## Troubleshooting

### "404 Not Found"

1. Check project ID is correct
2. Verify locale code (use `en`, not `en-US`)
3. Ensure namespace is published

### "Stale content"

1. Check if publish completed
2. Force cache purge: Dashboard → CDN → Purge Cache
3. Add cache-busting query param

### "CORS errors"

CDN includes CORS headers by default:
```
Access-Control-Allow-Origin: *
```

For custom domain, configure in CDN settings.

## Best Practices

1. **Pin versions in production** - Avoid surprises
2. **Use namespaces** - Load only what you need
3. **Implement fallbacks** - Handle missing locales gracefully
4. **Monitor cache hits** - Optimize TTL based on update frequency
5. **Preload critical translations** - In `<head>` for above-fold content
