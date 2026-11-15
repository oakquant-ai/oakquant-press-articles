## Plan

Prove the OakQuant stack on a dedicated Mac mini for development and staging. Purchase a separate production server in 2027. Keep production isolated. Move production from DigitalOcean after validation and activation of AT&T business connectivity.

## Hardware

### Permanent development and staging server

- Mac mini with M5 Pro.
- 18-core CPU.
- 20-core GPU.
- 16-core Neural Engine.
- 64GB unified memory.
- 2TB internal SSD.
- 2.5 Gigabit Ethernet.

The existing laptop remains the interactive development and administration machine. The Mac mini remains the development and staging server after production moves.

### Production server

Purchase a separate, dedicated production server in 2027 after proving the stack and measuring its requirements. No development or staging workloads will run on that server.

The production model, specifications, and purchase budget are not yet decided.

## Budget

### DigitalOcean: actual spend and capability budget

| Cost basis | Monthly | Annualized |
|---|---:|---:|
| Verified current DigitalOcean spend | $388.52 | $4,662.24 |
| Planned capability budget | $450.00 | $5,400.00 |
| Difference | $61.48 | $737.76 |

The $388.52 actual spend reflects a reduced running workload:

- The text-to-voice application has been stopped.
- The games application is not running.
- The new Assist application is not yet hosted.

Use $450 per month as the planning baseline for the intended capability set. It is a budget, not a measured invoice for those additional workloads. Retain $388.52 as the verified current-spend baseline.

Annualized figures are monthly amounts multiplied by 12, not a statement of historical annual spending.

### Mac mini: outright and 12-month payment costs

| Payment basis | Upfront hardware payment | Monthly hardware payment | Total hardware cost |
|---|---:|---:|---:|
| Outright purchase | $3,699.00 | $0.00 | $3,699.00 |
| 12-month payment planning basis | $0.00 | $308.25 for 12 months | $3,699.00 |

The selected configuration includes the upgraded CPU/GPU. The hardware amount is the price recorded in this discussion and must be confirmed at checkout.

The 12-month amount is the purchase price divided into 12 equal payments, assuming no down payment, interest, fees, or residual payment. It is not a verified lease or financing quote. Actual lease terms, ownership at the end of the term, eligibility, and charges are not yet established. No payment method has been selected.

All hardware figures exclude sales tax and other unquoted charges.

### Monthly cash flow during the 12-month payment period

| Planning scenario | DigitalOcean monthly basis | Mac mini monthly payment | Combined monthly amount |
|---|---:|---:|---:|
| Overlap at verified current spend | $388.52 | $308.25 | $696.77 |
| Overlap at planned capability budget | $450.00 | $308.25 | $758.25 |

These overlap figures apply while retaining DigitalOcean and making the assumed hardware payments. They exclude AT&T, electricity, backups, production hardware, taxes, and other unquoted costs. They are not steady-state hosting totals.

With an outright purchase, the $3,699 hardware payment occurs upfront and there is no monthly Mac mini payment. DigitalOcean continues during validation and migration.

### Costs not yet established

- Production server purchase or payment plan.
- AT&T Business Fiber and 5G backup, including installation and equipment.
- Sales tax.
- Power protection and any additional networking equipment.
- Backup storage and off-site backup services.
- Electricity.
- Any remaining cloud services after migration.

Retire DigitalOcean resources individually after successful migration. Do not treat either DigitalOcean baseline as guaranteed net savings. No payback period is committed.

## Timing

| Period | Action |
|---|---|
| November 15-22, 2026 | One-week cooling-off period. Hold the hardware purchase and review the plan. |
| November 22, 2026 | Review before ordering. Confirm the final hardware price, payment terms, and delivery. |
| December 2026, subject to delivery | Set up the Mac mini. Prove one application, its database, and a real Timber service end to end. |
| December 2026 into early 2027 | Deploy remaining in-scope applications to staging, measure resources, and rehearse deployment, migration, and restoration. |
| 2027 | Purchase and configure the separate production server after validation. |
| Before production cutover | Activate AT&T business connectivity and validate the actual production hardware and network. |
| Migration period | Move one application at a time, targeting a couple of weeks of parallel validation. |
| After successful cutovers | Complete the rollback window, then retire unneeded DigitalOcean resources. |

The delivery estimate displayed on November 15 was December 1-4. Reconfirm when ordering. Internet activation, production cutover, and DigitalOcean retirement are separate gates.

## Environment layout

### During validation

- Existing laptop: interactive development and administration.
- Mac mini: separate development and staging environments.
- DigitalOcean: authoritative production.

### After migration

- Existing laptop: interactive development and administration.
- Mac mini: development and staging.
- Dedicated production server: production only.
- AT&T Business Fiber: production internet connectivity.

Separate databases, credentials, storage, service instances, and job queues by environment where applicable. Limit nonproduction resource consumption. Restrict production network and deployment access. Do not perform interactive development or experimental model installation on production.

The two servers will still share their physical location and may share power and connectivity. This is not full infrastructure redundancy.

## Production parity

Maintain shared deployment definitions with environment-specific configuration. Staging will use the intended production application and Timber versions, runtime and dependencies, PostgreSQL version and extensions, database migrations, service implementations, API contracts, tokenizers, embedding models, and vector dimensions.

Keep endpoints, secrets, permissions, data, and resource allocations separate. Use real service implementations for acceptance tests. Promote versioned deployment artifacts and validate the final production machine before moving users.

The container runtime and deployment tooling have not yet been selected.

## Timber software changes

### Lightweight core and adapters

- Preserve existing application-facing Timber calls.
- Keep configuration and thin remote adapters inside Timber.
- Do not create a separate Links package.
- Remove heavyweight ML/vector dependencies from mandatory Timber dependencies.
- Keep local implementations in optional extras or separate implementation packages.
- Avoid eager imports of heavyweight libraries.
- Keep server implementation code outside the lightweight client import path.

### Independent capability services

- Deploy heavyweight knowledge/vector and ML execution independently.
- Move embedding, tokenization, vector operations, and ML execution behind the appropriate service boundaries.
- Begin with one capability and one pilot application.
- Use explicit disabled, local, or remote execution selection.
- Use versioned API contracts, scoped authentication, bounded timeouts/retries, and clear errors.
- Use job submission, status, cancellation, and results for long-running operations where required.
- Do not silently switch providers, transmit data elsewhere, or invoke paid fallbacks.
- Keep a standalone SDK, public billing, and paid-service rollout outside this phase.

### Required configuration

When disabled, do not initialize the capability, make its network calls, or import optional heavyweight implementations.

When enabled:

- Require explicit provider selection.
- Validate provider-specific settings at initialization.
- Require the remote endpoint and configured authentication material for remote execution.
- Validate timeout values.
- Require the relevant installed implementation and settings for local execution.
- Resolve required secret references.
- Report configuration errors together without exposing secrets.

Keep configuration validity separate from service connectivity and readiness. Keep secrets out of GitHub. Exact configuration field names remain implementation work.

## Internet: AT&T

Use AT&T Business Fiber with 5G backup.

Before activation:

- Confirm availability, installation timing, pricing, equipment, and contract terms at the address.
- Confirm permission for the intended application hosting and API/ML services.
- Establish inbound access and addressing requirements.
- Confirm and test failover, including the behavior of inbound connections over backup connectivity.

Keep existing connectivity during preparation. Do not assume residential terms authorize commercial staging or production hosting. Activate authorized business connectivity before production cutover, separately from DigitalOcean retirement.

## Backups and recovery

- Keep code and deployment definitions in GitHub.
- Implement database-consistent backups.
- Maintain encrypted off-site copies.
- Test restoration before production cutover.
- Document restart and recovery procedures.

## Migration

- Keep DigitalOcean authoritative while validating staging.
- Use isolated staging data, sanitized where required.
- Disable or sandbox payments, notifications, and side-effecting jobs outside production.
- Define final synchronization, write cutover, and rollback for each application.
- Account for writes accepted after cutover when rolling back.
- Move one application at a time.
- Retain required DigitalOcean resources through a defined rollback window.
- Retire resources only after confirming traffic, data, jobs, and backups have moved successfully.

## Validation before production purchase

- [ ] All in-scope applications work in staging.
- [ ] Real Timber capabilities work without heavyweight application dependencies.
- [ ] Clean installation, build time, and artifact size are measured.
- [ ] Database migration and restoration are tested.
- [ ] CPU, memory, storage, and model requirements are recorded.
- [ ] Deployment and configuration are reproducible.
- [ ] AT&T availability, hosting permission, costs, and installation timing are confirmed.
- [ ] Production specifications and budget are established from measurements.
