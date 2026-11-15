## Plan

Purchase the development server and monitor in mid-December 2026. Purchase the isolated production server, UPS, and production backup drive in March 2027. Switch to AT&T business internet in April 2027, then migrate production and retire DigitalOcean after validation.

This document records decisions, not hardware alternatives.

> **Missing record.** The previously selected monitor's model and price have not been recovered in this conversation. That is a missing record, not an undecided purchase. The priced subtotal below excludes that monitor and is not an all-in total.

![infrastructure-before-after](images/infrastructure-before-after.svg)

*Before and after. Production moves from a shared cloud bill to a dedicated, isolated Mac Studio behind a UPS with local and off-site backups. The Mac mini takes over development and staging, and the laptop stays for interactive work.* ([PNG](images/infrastructure-before-after.png))

## Target timeline

Dates are targets. Procurement lead times and acceptance tests control cutover.

![timeline](images/timeline.svg)

*December 2026 to May 2027. Bar length is to scale by calendar day.* ([PNG](images/timeline.png))

| Date | Milestone |
|---|---|
| November 22, 2026 | Complete the one-week review. Retain the December purchase schedule. |
| December 15, 2026 | Purchase Mac mini and selected monitor. |
| December 31, 2026 | Complete initial dev/staging setup, subject to delivery. Deploy one application, PostgreSQL, and the first real Timber service. |
| January 31, 2027 | Complete Timber's lightweight remote execution path and migrate remaining in-scope applications into isolated staging. |
| February 15, 2027 | Complete functional tests, measure memory/CPU/storage/model requirements, and test repeatable deployment. |
| February 28, 2027 | Complete migration rehearsals. Confirm AT&T eligibility, hosting permission, pricing, and April installation appointment. |
| March 1, 2027 | Purchase Mac Studio, UPS, and LaCie production backup drive. |
| March 15, 2027 | Target hardware installation, subject to delivery. Configure isolated production, power protection, monitoring, and backups. |
| March 31, 2027 | Complete production deployment rehearsal, restore test, and orderly UPS shutdown test. |
| April 1-7, 2027 | Target AT&T business internet activation and network/failover validation. |
| April 8-21, 2027 | Run isolated parallel validation over the authorized business connection. DigitalOcean remains authoritative until each application's write cutover. |
| April 22-28, 2027 | Target application-by-application production cutovers. |
| April 29-May 12, 2027 | Retain required DigitalOcean rollback resources. |
| May 13, 2027 | Target retirement of unneeded DigitalOcean resources after the final successful cutover and rollback window. |

Delay dependent milestones if delivery or validation slips. Do not combine internet activation, production cutover, and cloud cancellation into one event.

## Hardware

| Role | Selected hardware | Purchase target |
|---|---|---|
| Development and staging | Mac mini, M5 Pro, 18-core CPU, 20-core GPU, 16-core Neural Engine, 64GB unified memory, 2TB SSD, 2.5 Gigabit Ethernet | December 15, 2026 |
| Monitor | Previously selected monitor; original model and price still to be restored to this record | December 15, 2026 |
| Production | Mac Studio, M5 Ultra, 30-core CPU, 64-core GPU, 32-core Neural Engine, 96GB unified memory, 2TB SSD, 10 Gigabit Ethernet | March 1, 2027 |
| Production power protection | CyberPower CP1500PFCLCD, 1500VA / 1000W, sine-wave UPS | March 1, 2027 |
| Local production backup | LaCie Mobile Drive Secure USB-C 4TB with Rescue | March 1, 2027 |

Keep the Mac mini permanently assigned to development and staging. Keep the existing laptop for interactive development and administration. Production runs on the dedicated Mac Studio, with no interactive development or experimental model installations.

## Hardware budget

Prices are before tax and shipping. Recheck when ordering. The Mac mini price is carried forward from this discussion; the Mac Studio price comes from the uploaded Apple configuration. UPS and backup prices were checked November 15, 2026.

| Purchase | Outright cost | 12-month payment comparison |
|---|---:|---:|
| Mac mini | $3,699.00 | $308.25/month, calculated allocation |
| Monitor | Original price missing from this record | Not calculable until price is restored |
| Mac Studio | $5,999.00 | Apple lists $499.91/month, final payment adjusted |
| CyberPower CP1500PFCLCD | $239.95 | $20.00/month equivalent, not a financing offer |
| LaCie 4TB backup drive | $209.95 | $17.50/month equivalent, not a financing offer |
| **Known hardware subtotal, excluding monitor** | **$10,147.90** | **$845.66/month equivalent** |

The monthly-equivalent column divides purchase cost over 12 months; rounded figures are not exact payment schedules. It does not establish financing eligibility or lease availability for the mini or accessories. Apple's uploaded Studio configuration offers 12-month Apple Card financing at 0% APR, subject to approval and terms. Financing is not a lease. Do not label these figures as a verified 12-month lease quote.

### Purchase timing and cash requirements

![hardware-cash](images/hardware-cash.svg)

*Outright cash by purchase window. The hatched block marks the monitor, whose price is not recorded. Its width carries no value.* ([PNG](images/hardware-cash.png))

| Purchase window | Outright hardware cash requirement |
|---|---:|
| December 2026 | $3,699.00 plus selected monitor |
| March 2027 | $6,448.90 |
| **Total known hardware** | **$10,147.90 plus selected monitor** |

If the two computers are paid over 12 months at 0% with no fees, the planning schedule is $308.25/month for the mini from its December purchase, plus approximately $499.92/month for the Studio from its March purchase. The overlapping computer payment allocation is $808.17/month. Pay the UPS and backup drive outright in March, totaling $449.90. This is a cash-flow model, not a selected payment method or new financing quote; actual billing start dates and final payments depend on checkout terms.

### Hosting baselines

| Baseline | Monthly | Annualized |
|---|---:|---:|
| Verified current DigitalOcean spend | $388.52 | $4,662.24 |
| Intended capability budget | $450.00 | $5,400.00 |

Current spending excludes the stopped text-to-voice application, the inactive games application, and the not-yet-hosted Assist application. Use $450/month for capability planning and retain $388.52/month as verified actual spend. Annualized figures are monthly amounts multiplied by 12, not historical annual expenditure.

Continue DigitalOcean during preparation and cutover. Do not count its full bill as net savings before identifying retained cloud services and new operating costs.

### Recurring and additional costs still requiring quotes

- AT&T Business Fiber with 5G backup, equipment, installation, and required addressing.
- Encrypted off-site production backup storage.
- Electricity and any necessary network equipment.
- Tax and any shipping charges.
- Any retained cloud services and applicable software licenses.

No all-in operating total or payback period is asserted before these costs and the missing monitor price are recorded.

## Timber software changes

![timber-before-after](images/timber-before-after.svg)

*Before and after. Heavy tokenization, embedding, vector and ML code leaves the mandatory install. A thin client calls independently deployed services over a scoped, versioned contract.* ([PNG](images/timber-before-after.png))

- Preserve existing application-facing calls.
- Keep configuration and thin remote adapters inside Timber. No separate Links package.
- Remove heavy ML/vector dependencies from mandatory Timber dependencies.
- Put local implementations in optional extras or separate implementation packages; avoid eager imports.
- Keep client imports independent of service implementation code.
- Independently deploy knowledge/vector and ML services, moving heavyweight tokenization, embedding, vector operations, and ML execution behind the appropriate boundaries.
- Start with one pilot application and one capability, then extend.
- Use scoped authentication, versioned contracts, bounded retries/timeouts, and clear errors.
- Use asynchronous job APIs where required for long-running work.
- Keep disabled/local/remote selection explicit. No silent paid fallback or data transfer.
- Defer a standalone SDK and public paid-service rollout.

### Enabled configuration contract

Disabled capabilities do not initialize clients, make capability-specific network calls, or import optional heavy implementations.

Enabled capabilities require explicit provider selection and valid provider-specific configuration. Remote execution requires an endpoint and the configured authentication material. Local execution requires the installed implementation and its settings. Validate timeout values and resolve secret references. Collect configuration errors without exposing secrets.

Keep configuration validation separate from service connectivity/readiness. Keep secrets out of GitHub.

## Development and staging

Run separate logical environments on the mini, with separate data, credentials, service instances, and queues where applicable. Limit their resource consumption.

Use GitHub for code, migrations, seed scripts, and deployment definitions. Use iCloud for documents and planning files. Push work-in-progress code regularly; accept that unpushed local-only work is not protected by GitHub.

Development and staging databases are disposable. Rebuild them from migrations and seed data. Do not purchase an external backup drive or implement database backups for these disposable databases.

Match production's service implementations, runtime versions, dependency versions, database extensions, tokenizer, embedding model, and vector dimensions. Keep environment-specific settings and data separate. Use real capabilities for acceptance tests and versioned deployment artifacts.

## Production backups and power

Dedicate the LaCie to the production Mac Studio. Automate encrypted local backups and database-consistent PostgreSQL backups. Maintain independent encrypted off-site copies and test restoration. Keep production backup credentials inaccessible to development workloads.

Use the UPS for the production Mac Studio and essential network equipment, including the gateway/ONT where separately powered and physically reachable. Connect the bus-powered LaCie to the protected Studio. Configure and test orderly automatic shutdown. Verify actual runtime at measured load; do not assume extended outage operation.

Separate machines do not provide separate location, power, or internet redundancy. The UPS handles brief interruptions and shutdown time, not indefinite operation.

## Internet: AT&T

Switch to AT&T Business Fiber with 5G backup in April 2027.

Confirm address availability, hosting permission for applications and API/ML services, contract terms, price, installation, inbound addressing, and backup behavior before activation. Test inbound access, failover, and recovery. Do not assume 5G preserves inbound hosting automatically.

Keep existing connectivity during preparation. Do not expose commercial staging or production services through residential connectivity without authorization. Complete local preparation first; perform externally accessible commercial validation only on authorized connectivity.

## Cutover and retirement

Keep DigitalOcean authoritative during staging validation. Isolate copied data and disable or sandbox payments, notifications, and side-effecting jobs outside production.

For each application, define synchronization, write cutover, verification, and rollback. Preserve writes made after cutover if rollback is required. Confirm traffic, data, background jobs, monitoring, and backup restoration before retiring resources.

Production is ring-fenced with separate credentials, data, restricted network access, and controlled releases. Retain rollback resources through May 12 if the final cutover succeeds April 28; move the retirement date if cutover slips.
