⸻

# BaaS Multitenant Skeleton (Spring Boot)

A minimal, practical starting point for a multi-tenant Backend-as-a-Service built with Spring Boot.  
**Focus:** simple tenant onboarding, request routing by tenant, and clear isolation choices.

---

## Badges (replace `<owner>` and `<repo>` after you push)

CI: ![CI](https://github.com/thushar10/baas-multitenant-skeleton/actions/workflows/ci.yml/badge.svg)

---

## Problem Statement (plain English)

We need a backend that serves multiple customer tenants from one deployment.

Each request must:
- Identify which tenant it belongs to (e.g., header `X-Tenant-Id`).
- Enforce data isolation (no cross-tenant leaks).
- Expose basic tenant operations (create, list, get), a health check, and room to grow.

**Tenancy identification (Day 1 choice):**
- HTTP header (easiest dev) — e.g., `X-Tenant-Id` ✅

**Isolation strategy (Day 1 choice):**
- Single database, table + discriminator column (fastest start; lowest isolation) ✅

---

## Roadmap & Milestones

- **M1 – Skeleton Boots**
  - Spring Boot app builds, runs locally, `/health` endpoint works  
  ✅ *Done when: CI is green and /health returns 200.*

- **M2 – Tenant Identification**
  - Read tenant from chosen mechanism (header) and store in request-scoped context  
  ✅ *Done when: `/tenants/{id}` logs/resolves the current tenant id.*

- **M3 – Persistence + Isolation**
  - Use Postgres; implement tenant-aware repository (discriminator or schema).  
  ✅ *Done when: creating data under tenant A does not appear for tenant B.*

- **M4 – Minimal APIs**
  - `POST /tenants` (register), `GET /tenants/{id}`, `GET /health`  
  ✅ *Done when: curl requests succeed and return JSON.*

- **M5 – Security (stub)**
  - Add simple API key or Basic auth to protect admin endpoints  
  ✅ *Done when: unauthorized calls are rejected with 401.*

- **M6 – Containerize & Tag**
  - Build Docker image and tag `hello-service:0.1.0`  
  ✅ *Done when: image runs locally and prints startup logs.*

- **M7 – Observability (starter)**
  - Basic request logging with tenant id; add `/actuator/health`  
  ✅ *Done when: Logs show `tenantId=<id>` for each request.*

- **M8 – Deploy (stretch)**
  - EKS or local k8s manifest placeholders in `/infra`  
  ✅ *Done when: at least a Deployment/Service YAML exists.*

---

## What to Screenshot into `/screenshots`

- Passing CI run (green check)
- `/health` endpoint response in your browser or curl
- One request showing `X-Tenant-Id` in logs
- (Later) DB table rows proving tenant isolation

---

## Quick Start (local)

**Prereqs:** Java 21, Maven, Docker (optional Day 1)

1) Clone and open the project.
2) Build once:
   ```bash
   mvn -q -DskipTests package
   ```
3) Run locally:
  ```
  mvn spring-boot:run
  ```
4) Health check: open http://localhost:8080/health

5)(Optional) Try a tenant call:
  ```
  curl -H "X-Tenant-Id: acme" http://localhost:8080/tenants/acme
  ```
> Don’t overbuild today—just make it run, prove tenant id flows through, and log it.

---

## API Outline (Day 1)

- `GET /health` → `{ "status": "UP" }`
- `POST /tenants` → create/register a tenant (accept JSON with `tenantId`)
- `GET /tenants/{id}` → return basic info for that tenant

### Example Requests/Responses

**Register a Tenant**

```bash
curl -X POST http://localhost:8080/tenants \
  -H "Content-Type: application/json" \
  -d '{ "tenantId": "acme" }'
```

**Response**
```json
{
  "tenantId": "acme",
  "status": "CREATED"
}
```

---

**Get Tenant Info**

```bash
curl -H "X-Tenant-Id: acme" http://localhost:8080/tenants/acme
```

**Response**
```json
{
  "tenantId": "acme",
  "status": "ACTIVE"
}
```



---

## Project Structure (suggested)

```
baas-multitenant-skeleton/
  README.md
  src/…
  infra/
  notes.md
  screenshots/   (drop images here)
  .github/workflows/ci.yml
```


---

## CI (GitHub Actions)

- The provided workflow builds with Maven on every push.
- After you push the repo to GitHub, the badge at the top will show green when CI passes.

**Workflow file:** `.github/workflows/ci.yml`

---

## Day 1 Definition of Done

- [x] CI passing with Maven build
- [x] /health returns 200 locally
- [x] One request proves tenant id is received and logged
- [x] README shows your chosen tenancy + isolation strategy
- [x] At least 2 screenshots in `/screenshots`

---

## License

MIT (or your choice)



