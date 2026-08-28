---
name: fullstack-architect
description: Architect and engineer production-grade, full-stack web applications with modern end-to-end architectures (Next.js, Remix, Astro, SvelteKit, Node/Express/FastAPI/Go), robust REST/GraphQL/tRPC API designs, resilient state management, database schema modeling, authentication/authorization flows, real-time communication (WebSockets/SSE), and scalable deployment patterns. Use this skill when building full-stack applications from scratch, refactoring architecture, integrating complex backend APIs with frontends, or designing scalable system components.
---

# Full-Stack Application Architect

A comprehensive skill for designing, structuring, and implementing production-ready full-stack web applications with senior-level architectural rigor, clean separation of concerns, enterprise-grade security, and robust scalability.

---

## 1. Core Architectural Principles

1. **Separation of Concerns & Modularity**:
   - Keep UI rendering, business logic, data access, and transport layers strictly decoupled.
   - Employ Feature-First or Domain-Driven modular architecture rather than flat, monolithic folder structures.
2. **Type Safety End-to-End**:
   - Share types/contracts between client and server whenever possible (e.g., tRPC, Zod schemas, OpenAPI specs, TypeScript monorepos).
   - Validate all untrusted input at runtime boundaries (API endpoints, form submissions, query parameters, external webhooks).
3. **Resilience & Fault Tolerance**:
   - Implement graceful error boundaries, circuit breakers, retry logic with exponential backoff, and idempotent mutations.
   - Never let unhandled server exceptions crash processes or expose internal stack traces to the client.
4. **Performance & Data Fetching Strategy**:
   - Choose optimal rendering strategies per route: Static Site Generation (SSG), Server-Side Rendering (SSR), Incremental Static Regeneration (ISR), or Client-Side Rendering (CSR).
   - Prevent N+1 query waterfalls with batching, dataloaders, and strategic caching (HTTP cache headers, Redis, Edge CDN).
5. **Security by Default**:
   - Enforce least-privilege RBAC/ABAC authorization at the data layer, not just by hiding UI elements.
   - Store session state securely (HTTP-only, Secure, SameSite cookies or short-lived signed JWTs with rotating refresh tokens).
   - Implement rate limiting, CORS configuration, CSRF protection, and sanitize all user input.

---

## 2. Recommended Directory & Layer Structure

Organize full-stack projects around feature modules for maintainability and scalability:

```text
src/
├── app/                  # Application routing (Next.js App Router / Remix routes / Page views)
├── features/             # Feature-based domain modules
│   ├── auth/
│   │   ├── components/   # Feature-specific UI components
│   │   ├── hooks/        # Feature-specific custom hooks
│   │   ├── server/       # Server actions / controllers / queries
│   │   ├── schemas/      # Zod validation schemas
│   │   └── types/        # TypeScript type definitions
│   └── billing/
├── components/           # Shared, domain-agnostic UI primitives (Button, Modal, Input)
├── lib/                  # Shared utilities, client configurations (DB client, Redis, fetch wrapper)
├── server/               # Core server utilities (middleware, auth config, DB schema & migrations)
└── types/                # Global ambient TypeScript declarations
```

---

## 3. End-to-End Workflow

### Step 1: Requirements & Data Flow Analysis
- Define core entities, relationships (1:1, 1:N, N:M), and state lifecycle.
- Map out user journeys and determine which operations are reads, writes, real-time subscriptions, or background jobs.
- Decide the tech stack fit (e.g., PostgreSQL + Prisma/Drizzle for relational data, Redis for queues/ephemeral cache).

### Step 2: API Contract & Schema First Design
- Create explicit validation schemas (e.g., Zod / Pydantic / JSON Schema) for all requests and responses.
- Design clean, RESTful or RPC endpoints:
  - Clear HTTP status codes (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `429 Too Many Requests`).
  - Standardized JSON envelope for API responses:
    ```json
    {
      "success": true,
      "data": { ... },
      "error": null,
      "meta": { "page": 1, "total": 100 }
    }
    ```

### Step 3: Database & Migration Strategy
- Write declarative migrations with explicit foreign keys, indexes on queried columns, and unique constraints.
- Implement soft-deletes and audit timestamps (`created_at`, `updated_at`, `deleted_at`) where business logic requires tracking.

### Step 4: Authentication & Authorization
- Implement secure session management (Auth.js/NextAuth, Lucia, Supabase Auth, Clerk, or custom JWT/Session handlers).
- Always verify permissions on the server inside services/controllers before executing queries or mutations:
  ```typescript
  // Server-side permission check example
  const session = await getSession(req);
  if (!session || !canUserPerform(session.user, 'workspace:delete', workspaceId)) {
    throw new AuthorizationError('Insufficient permissions');
  }
  ```

### Step 5: Frontend State & Mutation Handling
- Use optimistic UI updates with rollback on failure for snappy user experience.
- Leverage modern data fetching libraries (TanStack Query, SWR, or Server Components) with standardized cache invalidation keys.

---

## 4. Production Checklist

- [ ] **Data Validation**: Every public route validates request body, params, and query strings.
- [ ] **Error Handling**: Custom error class hierarchy with centralized error formatting middleware.
- [ ] **Rate Limiting**: IP and user-token based sliding window rate limits on auth and resource-heavy routes.
- [ ] **Environment Configuration**: Strictly validated environment variables at startup (e.g., via `@t3-oss/env-core` or Zod).
- [ ] **Logging & Telemetry**: Structured JSON logging (Pino/Winston) with request IDs (`x-request-id`) across client and server.
- [ ] **Database Connection Pooling**: Correct pool sizes configured for serverless/edge environments.
