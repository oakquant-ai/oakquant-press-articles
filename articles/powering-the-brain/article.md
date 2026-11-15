## Plan status

Planning baseline as of November 15, 2026. Hold purchases for one week and review the plan on November 22, 2026. Nothing in this document represents a completed purchase, an installation booking, or an authorized migration.

## Hardware

### First purchase: permanent development and staging server

Mac mini with M5 Pro:

- 18-core CPU.
- 20-core GPU.
- 16-core Neural Engine.
- 64GB unified memory.
- 2TB internal SSD.
- Standard 2.5 Gigabit Ethernet unless the optional networking upgrade is selected.
- Three rear Thunderbolt 5 ports, HDMI, two front USB-C ports, and headphone jack, as listed in the selected configuration.

Keep the upgraded CPU/GPU configuration. The quoted increment is $200 and is already included in the selected configuration, not an additional budget item.

The 64GB / 2TB configuration is the working selection. The optional 10 Gigabit Ethernet upgrade remains unconfirmed; the previously discussed increment was $100.

The delivery estimate shown on November 15 was December 1-4. Recheck delivery and the final configuration after the review week.

### Production server: separate purchase in 2027

- Dedicated, physically separate production machine.
- Mac hardware remains the preferred direction; Mac Studio is a candidate, not a finalized purchase.
- Final chip, memory, storage, networking, and budget remain open.
- Size the production machine from the measured staging workload.
- Keep the Mac mini permanently assigned to development and staging after production moves.
- Retain the existing laptop for interactive development and administration.

## Budget

Amounts are planning figures carried forward from this discussion, not a new vendor quote.

| Item | Planning amount | Status |
|---|---:|---|
| Selected Mac mini, 64GB / 2TB / upgraded cores | $3,699 before tax | Previously discussed price; verify at checkout |
| Optional 10 Gigabit Ethernet | +$100 before tax | Not selected yet |
| Mac mini total if faster Ethernet is selected | $3,799 before tax | Conditional total |
| Sales tax and any delivery charges | Not budgeted | Confirm at checkout |
| Separate production server | TBD | Purchase in 2027 after validation |
| AT&T Business Fiber and 5G backup | TBD | Address-specific quote required |
| UPS, backup storage, and any network equipment | TBD | Inventory and price before production |
| Off-site backup service and electricity | TBD | Recurring costs to establish |
| Current hosting and on-demand compute | More than $400/month | Continue during development and migration |

Do not book the full current hosting bill as savings. Identify which charges disappear, which remain, and the new recurring costs before calculating payback.

No paid software license has been selected. Confirm licensing for the chosen container runtime and other tools before adopting them.

## Timing

| Window | Work |
|---|---|
| November 15-22, 2026 | Cooling-off period. Review costs, inventory current workloads, and confirm the selected configuration. No hardware order during the hold. |
| November 22 onward | Decide whether to order the Mac mini. Recheck price and delivery. Request AT&T eligibility, terms, pricing, and installation lead time. |
| December 2026, subject to delivery | Set up development and staging. Deploy one application and the first Timber service end to end. Establish backups and restore testing. |
| December 2026 into early 2027 | Bring the remaining in-scope applications into isolated staging. Measure resources, test failure handling, and rehearse migrations. |
| New year, 2027 | Select and purchase the separate production server after the stack is proven. |
| Before production cutover | Activate authorized business connectivity, deploy the production stack, and validate the actual production network and hardware. |
| Migration period | Move one application at a time. Target a couple of weeks of parallel validation, with completion determined by test results. |
| After successful cutovers | Retain a defined DigitalOcean rollback window, then retire only the resources no longer required. |

Dates after the cooling-off period are targets, not commitments. Internet activation, production cutover, and DigitalOcean retirement are separate gates.

## Environment layout

```text
During validation
  Existing laptop: interactive development
  Mac mini: development + isolated staging
  DigitalOcean: authoritative production

After migration
  Existing laptop: interactive development
  Mac mini: development + isolated staging
  Dedicated production server: production only
  AT&T Business Fiber: authorized production connectivity
```

Development and staging use separate databases, credentials, storage, service instances, and job queues where applicable. Cap nonproduction resource consumption.

Staging is the production blueprint. Use the same runtime versions, dependency versions, database extensions, service implementations, API contracts, tokenizers, embedding models, and deployment process. Change environment-specific endpoints, credentials, data, and resource allocations.

Keep production free of interactive development and experimental model installation. Restrict access between environments. Separate machines do not provide independent power, location, or internet redundancy.

The container runtime and deployment tooling are not finalized. A shared deployment template with environment-specific configuration is the agreed direction.

## Timber software changes

### Keep Timber lightweight

- Preserve existing application-facing capability calls.
- Keep configuration and thin remote adapters inside Timber initially.
- Do not create a separate Links package at this stage.
- Remove heavyweight ML/vector dependencies from mandatory Timber dependencies.
- Place local implementations in optional extras or separate implementation packages.
- Avoid eager imports of heavy libraries.
- Ensure the lightweight client path imports no service implementation code.

### Independently deployed capabilities

- Move heavy embedding, tokenization, vector operations, and ML execution behind service boundaries where appropriate.
- Start with one working capability and one pilot application.
- Keep knowledge/vector and ML workers independently deployable.
- Use scoped authentication, versioned contracts, bounded timeouts/retries, and clear errors.
- Add job submission/status/results for operations that require asynchronous execution.
- Keep execution explicit: disabled, local, or remote. No silent provider fallback or paid data transfer.
- Defer a standalone SDK, generalized connector framework, billing, and public paid-service rollout.

### Required configuration

- When disabled: do not initialize the capability, make network calls, or import its optional heavy implementation.
- When enabled: require an explicit provider and validate that provider's settings.
- Remote execution: require a valid endpoint and the configured authentication material; validate timeout values.
- Local execution: require the relevant optional implementation and its settings.
- Resolve required secret references and report missing configuration together without exposing secret values.
- Keep configuration validation distinct from connectivity/readiness checks.
- Keep secrets out of GitHub and deployment templates.

Exact field names and schemas remain implementation work.

## Internet: AT&T

Preferred service: AT&T Business Fiber with 5G backup.

Before committing:

- Confirm availability at the installation address.
- Obtain written confirmation that the intended application hosting and potential API/ML services are permitted.
- Confirm primary speed, monthly price, installation charges, equipment, and contract terms.
- Confirm inbound-access requirements and the appropriate addressing/routing arrangement.
- Confirm how the proposed 5G backup handles failover and inbound service access. Do not assume it preserves public hosting automatically.
- Test failover and recovery before relying on backup connectivity.

Keep the existing internet service during preparation. Do not assume residential terms authorize commercial staging or production hosting. Local preparation is not permission to expose business services through the residential connection.

Activate the authorized business service before production cutover, not simultaneously with DigitalOcean cancellation.

## Migration and data protection

- Keep DigitalOcean authoritative while the replacement is being tested.
- Use isolated staging databases and sanitized data where needed.
- Disable or sandbox payments, notifications, and side-effecting scheduled jobs outside production.
- Store code and deployment definitions in GitHub.
- Implement database-consistent backups, encrypted off-site copies, and tested restoration.
- Define final data synchronization, write cutover, and rollback procedures for each application.
- Account for writes accepted after cutover; rollback is not just a traffic switch.
- Validate the final production machine before moving users.
- Retire DigitalOcean resources only after successful cutover and the agreed rollback window.

## Completion checklist before production purchase

- [ ] One complete application runs through the new architecture.
- [ ] All in-scope applications work in staging.
- [ ] Real Timber vector/ML capabilities work without heavy application dependencies.
- [ ] Clean installation, artifact size, and build time are measured.
- [ ] Database migrations and restoration are tested.
- [ ] CPU, memory, storage, and model requirements are recorded.
- [ ] Deployment and configuration are reproducible.
- [ ] AT&T service eligibility, hosting permission, price, and installation timing are confirmed.
- [ ] Production hardware and the complete remaining budget are approved.
