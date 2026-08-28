---
name: cloud-devops-architect
description: Design, automate, and manage production cloud infrastructure, CI/CD pipelines, container orchestration (Docker, Kubernetes), Infrastructure as Code (Terraform, OpenTofu, Pulumi), cloud providers (AWS, GCP, Azure), zero-downtime deployment strategies (blue/green, canary), monitoring, logging, and observability (Prometheus, Grafana, OpenTelemetry). Use this skill when setting up CI/CD workflows, writing Dockerfiles/docker-compose, designing cloud architectures, debugging deployment failures, or provisioning resilient cloud infrastructure.
---

# Cloud Infrastructure & DevOps Architect

A senior DevOps and Site Reliability Engineering (SRE) skill for architecting cloud-native platforms, automating deployment pipelines, containerizing applications, provisioning infrastructure declaratively, and ensuring 99.99% system availability.

---

## 1. DevOps & Cloud Principles

1. **Infrastructure as Code (IaC)**: Never configure production resources manually through cloud consoles. All infra must be versioned, audited, and applied via Terraform/OpenTofu/Pulumi.
2. **Immutability & Statelessness**: Treat servers and containers as cattle, not pets. Build immutable container images tagged with git commit SHAs.
3. **Zero-Downtime Deployment**: Utilize Blue/Green or Canary deployment models with automated rollback triggers based on health check telemetry and error rate spikes.
4. **Shift-Left Security & Secrets Management**: Scan dependencies (Trivy, Snyk) and lint IaC in CI. Never bake secrets into images; inject at runtime via secret managers (AWS Secrets Manager, HashiCorp Vault, Kubernetes Secrets).
5. **Full Observability**: Instrument apps with the Three Pillars of Observability: Metrics (Prometheus), Logs (Loki/Elasticsearch with structured JSON), and Traces (OpenTelemetry/Jaeger).

---

## 2. Dockerfile Best Practices & Multi-Stage Builds

Production-ready multi-stage Docker build pattern (Node.js/TypeScript example):

```dockerfile
# Stage 1: Dependency resolution & builder
FROM node:20-alpine AS builder
WORKDIR /app
RUN apk add --no-cache libc6-compat
COPY package.json package-lock.json ./
RUN npm ci --frozen-lockfile
COPY . .
RUN npm run build && npm prune --production

# Stage 2: Minimal runtime
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
RUN addgroup --system --gid 1001 nodejs && \
    adduser --system --uid 1001 nodeapp

COPY --from=builder /app/package.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist

USER nodeapp
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

CMD ["node", "dist/server.js"]
```

---

## 3. GitHub Actions CI/CD Pipeline Template

Comprehensive continuous integration and deployment workflow:

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  lint-and-test:
    name: Lint, Test & Security Scan
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm run test:coverage
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          ignore-unfixed: true
          severity: 'CRITICAL,HIGH'

  deploy:
    name: Build & Deploy to Production
    needs: lint-and-test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Authenticate to Cloud Provider
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy
          aws-region: us-east-1
      - name: Build & Push Image
        run: |
          IMAGE_TAG=${{ github.sha }}
          docker build -t app:$IMAGE_TAG .
          # Push to registry and trigger blue-green rolling rollout
```

---

## 4. Disaster Recovery & Reliability Checklist

- [ ] **Health Endpoints**: Distinct `/healthz` (liveness: process alive) and `/ready` (readiness: DB & cache connected) probes.
- [ ] **Graceful Shutdown**: Intercept `SIGTERM` and `SIGINT` signals, close active DB connections, finish inflight requests, and terminate cleanly within 30s.
- [ ] **Resource Limits**: Set CPU/Memory requests and limits on all container manifests to prevent OOM cascade failures.
- [ ] **Backups**: Automated daily encrypted database snapshots with retention policies and tested restore procedures.
- [ ] **Alerting Thresholds**: P1 alerts on 5xx error spikes (>1%), high latency p99 (>1000ms), and disk usage (>80%).
