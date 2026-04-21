# DVG Subscriber Management API — Apigee Hybrid

Exposes subscriber balance, bundles and profile endpoints through Apigee Hybrid.

## Endpoints

| Method | Path | Auth | Consumer |
|--------|------|------|----------|
| GET | `/dvg/subscribers/v1/{msisdn}/balance` | API Key | Internal + External |
| GET | `/dvg/subscribers/v1/{msisdn}/bundles` | API Key | Internal + External |
| GET | `/dvg/subscribers/v1/{msisdn}/profile` | OAuth2 Bearer | Internal only |

## Security

- External consumers: `x-api-key` header required
- Internal profile: OAuth2 Bearer token with scope `dvg.subscriber.profile.read`
- Spike Arrest: 200 req/sec global
- External Quota: 100 req/min per API key
- Internal Quota: 1000 req/min per client

## Branch Strategy

| Branch | Deploys To | How |
|--------|-----------|-----|
| `feature/*` | Nothing | Work in progress |
| `develop` | Dev environment | Auto on push |
| `main` | Production | Auto after manual approval |

## CI/CD

Pipeline: GitHub Actions
- Push to `develop` → auto-deploys to dev + runs Newman tests
- Merge to `main` → requires approval → deploys to prod

## Local Test

```bash
newman run tests/dvg-subscriber.postman_collection.json \
  --env-var baseUrl=https://YOUR_HYBRID_INGRESS \
  --env-var apiKey=YOUR_DEV_KEY
```

## Infrastructure

- Runtime: Apigee Hybrid on GKE (dev) / OpenShift (prod target)
- GCP Project: emerald-ether-493408-p6
- Dev environment: eval
- Helm chart version: 1.14.0
