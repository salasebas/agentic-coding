# Performance Checklist

Quick reference checklist for web application performance.

## Core Web Vitals Targets

| Metric | Good | Needs Work | Poor |
|--------|------|------------|------|
| LCP (Largest Contentful Paint) | ≤ 2.5s | ≤ 4.0s | > 4.0s |
| INP (Interaction to Next Paint) | ≤ 200ms | ≤ 500ms | > 500ms |
| CLS (Cumulative Layout Shift) | ≤ 0.1 | ≤ 0.25 | > 0.25 |

## TTFB Diagnosis

When TTFB is slow (> 800ms), check each component in DevTools Network waterfall:

- [ ] **TCP/TLS handshake** slow → enable HTTP/2, consider edge deployment, verify keep-alive
- [ ] **Server processing** slow → profile backend, check slow queries, add caching

## Frontend Checklist

### Images
- [ ] Images use modern formats (WebP, AVIF)
- [ ] Images are responsively sized (`srcset` and `sizes`)
- [ ] Images and `<source>` elements have explicit `width` and `height` (prevents CLS in art direction)
- [ ] Below-the-fold images use `loading="lazy"` and `decoding="async"`
- [ ] Hero/LCP images use `fetchpriority="high"` and no lazy loading

### JavaScript
- [ ] Code splitting with dynamic `import()` for routes and heavy features
- [ ] Tree shaking enabled (verify dependency ships ESM and marks `sideEffects: false`)
- [ ] No blocking JavaScript in `<head>` (use `defer` or `async`)
- [ ] Heavy computation offloaded to Web Workers (if applicable)
- [ ] `React.memo()` on expensive components that re-render with same props
- [ ] `useMemo()` / `useCallback()` only where profiling shows benefit
- [ ] Long tasks (> 50ms) broken up to keep the main thread available — main lever for INP
- [ ] `yieldToMain` pattern used inside long-running loops so input events can run between chunks
- [ ] Modern scheduling APIs used where available: `scheduler.yield()` (preferred), `scheduler.postTask()` with priorities
- [ ] `requestIdleCallback` for deferrable, non-urgent work (analytics flush, prefetch, warmup)
- [ ] Non-critical work deferred out of event handlers (e.g. analytics, logging) so the response to the interaction is not delayed
- [ ] Third-party scripts loaded with `async` / `defer`, audited for size, and fronted by a facade when heavy (chat widgets, embeds)

### CSS
- [ ] Critical CSS inlined or preloaded
- [ ] No render-blocking CSS for non-critical styles
- [ ] No CSS-in-JS runtime cost in production (use extraction)

### Fonts
- [ ] Limited to 2–3 font families, 2–3 weights each (every additional weight is another request)
- [ ] WOFF2 format only (smallest, universal support — skip WOFF/TTF/EOT)
- [ ] Self-hosted when possible (third-party font CDNs add DNS + TCP + TLS round-trips)
- [ ] LCP-critical fonts preloaded: `<link rel="preload" as="font" type="font/woff2" crossorigin>`
- [ ] `font-display: swap` (or `optional` for non-critical) to avoid FOIT blocking render
- [ ] Subsetted to ship only the glyphs each page needs (`unicode-range` selects which subset file downloads)
- [ ] Variable fonts considered when multiple weights/styles are required (one file replaces many)
- [ ] Fallback font metrics adjusted with `size-adjust`, `ascent-override`, `descent-override` to reduce CLS on font swap
- [ ] System font stack considered before any custom font

### Network
- [ ] Static assets cached with long `max-age` + content hashing
- [ ] API responses cached where appropriate (`Cache-Control`)
- [ ] HTTP/2 or HTTP/3 enabled
- [ ] Resources preconnected (`<link rel="preconnect">`) for known origins
- [ ] `fetchpriority` used on critical non-image resources (e.g., key `<link rel="preload">`, above-the-fold `<script>`) — not only on `<img>`
- [ ] No unnecessary redirects

### Rendering
- [ ] No layout thrashing (forced synchronous layouts)
- [ ] Animations use `transform` and `opacity` (GPU-accelerated)
- [ ] Long lists use virtualization (e.g., `react-window`)
- [ ] No unnecessary full-page re-renders
- [ ] Off-screen sections use `content-visibility: auto` with `contain-intrinsic-size` to skip layout/paint of non-visible areas
- [ ] No `unload` event handlers — preserves back/forward cache (bfcache) eligibility

## Backend Checklist

### Database
- [ ] No N+1 query patterns (use eager loading / joins)
- [ ] Queries have appropriate indexes
- [ ] List endpoints paginated (never an unbounded `SELECT`)
- [ ] Connection pooling configured
- [ ] Slow query logging enabled

#### Query plans
- [ ] `EXPLAIN ANALYZE` captured **before** the fix, not just after — it is the baseline
- [ ] `Seq Scan` on a large table understood: index missing, unusable, or genuinely not worth it
- [ ] Estimated vs actual `rows=` within an order of magnitude (if not, refresh statistics before touching indexes)
- [ ] No `Sort` node that a composite index could absorb
- [ ] Plan re-checked after the change — an index that did not change the plan gets reverted

#### Index strategy
- [ ] Composite index column order is equality first, then range/sort
- [ ] Index covers the query shape (filter + sort), not just one column in isolation
- [ ] Covering index considered for hot read paths (index-only scan avoids the heap fetch)
- [ ] Not indexing low-selectivity columns *for the dominant value*; a partial index still serves the rare-value query (`WHERE status = 'failed'`)
- [ ] Expression index used where the query applies a function (`lower(email)`)
- [ ] Full-text or trigram index used for leading-wildcard search, not a B-tree
- [ ] Write cost measured on write-heavy tables (every index taxes every `INSERT`/`UPDATE`)
- [ ] Unused and duplicate indexes dropped (they cost writes and buy nothing)

#### Connection pooling
- [ ] One pool per process, not per request or per module
- [ ] `instances × pool max` stays under the database's `max_connections`
- [ ] `connectionTimeoutMillis` set so exhaustion fails fast instead of queueing forever
- [ ] Exhaustion diagnosed before resizing: find what holds connections (long transactions, missing `await`, leaked clients)
- [ ] Serverless / autoscaling fronted by a multiplexing proxy (pgbouncer, RDS Proxy) rather than a larger pool

### API
- [ ] No synchronous heavy computation in request handlers
- [ ] Bulk operations instead of loops of individual calls
- [ ] Response compression (gzip/brotli)
- [ ] Appropriate caching (in-memory, Redis, CDN)

## Caching Strategies

### Read and write patterns

| Pattern | How it works | Use when | Watch out for |
|---|---|---|---|
| **Cache-aside** (lazy) | App checks cache, on miss reads origin and populates | Default choice; read-heavy, tolerant of a cold first hit | Every miss hits the origin, so it needs stampede protection |
| **Read-through** | Cache layer itself loads on miss | You want the load path in one place, not at every call site | Hides origin latency; a slow origin looks like a slow cache |
| **Write-through** | Write goes to cache and origin together, synchronously | Reads must never see a stale value after a write | Adds cache latency to every write |
| **Write-behind** (write-back) | Write hits cache, origin updated asynchronously | Write-heavy, and the origin is the bottleneck | Data loss window if the cache dies before the flush. Needs durability you can defend |

### Negative caching

Cache the *absence* of a result too. A key that misses on every lookup (a nonexistent user ID probed in a loop, a 404 asset) sends every request to the origin, which is a cache that only protects the happy path.

- Store an explicit "not found" sentinel with a **shorter** TTL than positive entries
- Keep the negative TTL short enough that a newly created record appears promptly
- Never let an origin *error* become a negative cache entry, or one failing minute becomes many

### Request coalescing (stampede protection)

One recompute, N waiters. Prevents a hot key's expiry from delivering the full concurrent load to the origin:

```typescript
const inFlight = new Map<string, Promise<unknown>>();

function loadOnce<T>(key: string, fetcher: () => Promise<T>): Promise<T> {
  const existing = inFlight.get(key) as Promise<T> | undefined;
  if (existing) return existing;
  const p = fetcher().finally(() => inFlight.delete(key));
  inFlight.set(key, p);
  return p;
}
```

For a shared cache, the same idea needs a distributed lock, or `stale-while-revalidate` so waiters serve the stale value instead of blocking.

### Cache checklist
- [ ] The cached call was measured as expensive first (caching a fast call adds a hop and buys nothing)
- [ ] Read/write ratio justifies the cache (re-read far more often than written)
- [ ] Cache key includes every input the response varies on: tenant, viewer, locale, permissions, feature flags
- [ ] No per-user data cached under a key that does not identify the user
- [ ] One invalidation strategy chosen (TTL, event/tag, or versioned keys), not an accidental mix
- [ ] Acceptable staleness window written down, not implied by whatever TTL was typed
- [ ] Stampede protection on hot keys (coalescing, lock, or `stale-while-revalidate`)
- [ ] Negative results cached with a shorter TTL; origin errors never cached
- [ ] Eviction policy and memory ceiling set (an unbounded cache is a memory leak)
- [ ] Hit rate monitored — a cache nobody measures is an assumption, and a low hit rate is pure overhead
- [ ] Nothing cached whose staleness is a correctness bug (balances, permissions, inventory at checkout)
