# Codrona Grid

The execution plane: running untrusted code safely, under contest-day load.

## What lives here

- Judge service with hardened sandboxes (nsjail/gVisor, Judge0 CE core)
- Queue: Redis plus Redpanda
- Kubernetes manifests; KEDA scaling on queue depth, HPA on the API
- Terraform for AKS and EKS, values-driven with no cloud hard-coding
- OpenTelemetry instrumentation, SLOs, and alerting
- k6 load profiles used as CI gates
- FinOps: cost exports feeding Lens

## Principles

**Untrusted by default.** Every submission is hostile until proven otherwise.
Non-root, read-only filesystem, no network, strict resource caps.

**Portable by construction.** Terraform is values-driven. When credits end,
`terraform destroy` and the same stack boots on a laptop via kind and
docker-compose. `MIGRATION.md` documents re-pointing to any paid cloud as a
configuration change.

**Never on the critical path.** Nothing that can pause on inactivity is permitted
to serve a user-facing request.

## Gates

G7 (contest-day load) blocks release: p99 verdict latency and error rate under a
k6 spike profile that models a contest start.

## Licence

AGPL-3.0-or-later.
