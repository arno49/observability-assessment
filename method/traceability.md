# Traceability to Google SRE and DORA

**What is here:** each dimension of both maturity models traced to the closest chapter of Google's
two SRE books and to the DORA metric it touches, with a label for how the kit relates to it; then
what the kit adds of its own, and what the books cover that the kit does not. **Read it when:**
a client asks how the kit relates to Google SRE or DORA, or you want the source practice behind a
dimension.

The trace orients a reader; it does not change a score. Scores follow the scoring rules of each
model ([observability](maturity-model.md#scoring-rules), [SRE](../sre/maturity-model.md#scoring-rules)).
The SRE model's own mapping to DORA and the AWS Well-Architected Framework is in
[sre/maturity-model.md](../sre/maturity-model.md#mapping-to-external-frameworks).

## Sources and labels

- **Book**: *Site Reliability Engineering* (Google, 2016). **Workbook**: *The Site Reliability
  Workbook* (Google, 2018). Both are published on sre.google; chapter numbers and titles checked
  against their tables of contents on 2026-11-25. Chapters are cited by title; nothing is quoted.
- **DORA**: the delivery metrics as the kit names them: deployment frequency, change lead time,
  change fail rate, failed deployment recovery time, deployment rework rate. "None" where no
  DORA metric applies.
- **Label**, one per row:
  - «совпадает» (matches): the dimension asks for the same practice the books describe.
  - «расширяет» (extends): the same practice, with a stricter criterion or an evidence rule the
    books do not set.
  - «добавляет» (adds): the kit asks for something the books do not treat as a practice of
    its own.

## SRE model, 10 dimensions

| # | Dimension | Closest in Google SRE | DORA | Label | How the kit relates |
|---|---|---|---|---|---|
| 1 | Service catalogue and criticality | Book ch. 32 *The Evolving SRE Engagement Model* (production readiness review); Book ch. 27 *Reliable Product Launches at Scale*; Workbook ch. 18 *SRE Engagement Model* | None | «расширяет» (extends) | Same idea: criticality decides paging and review depth. The kit makes the catalogue a scored object, checked against deploys or infrastructure, and asks for a recorded decision when a tier-1 service rests on an untiered dependency |
| 2 | SLOs and error budgets | Book ch. 3 *Embracing Risk*; Book ch. 4 *Service Level Objectives*; Workbook ch. 2 *Implementing SLOs*; Workbook appendix B *Example Error Budget Policy* | None (DORA reads reliability as operational performance, outside its delivery metrics) | «совпадает» (matches) | The core of both. Level 4 asks for a policy agreed by product and engineering, as the books do; level 5 asks for a record of the policy having changed a decision. The books go deeper on choosing SLIs and revising targets |
| 3 | Monitoring and alerting (light) | Book ch. 6 *Monitoring Distributed Systems* (symptoms over causes, the golden signals); Book ch. 10 *Practical Alerting*; Workbook ch. 4 *Monitoring*; Workbook ch. 5 *Alerting on SLOs* (multiwindow, multi-burn-rate alerts) | None | «совпадает» (matches) | Symptom alerts at level 3 and burn-rate alerts at level 4 follow the books. The kit adds a measured actionable share, and in the combined format leaves the depth to observability dimension 7 |
| 4 | Incidents and learning | Book ch. 14 *Managing Incidents*; Book ch. 15 *Postmortem Culture: Learning from Failure*; Book ch. 16 *Tracking Outages*; Workbook ch. 9 *Incident Response*; Workbook ch. 10 *Postmortem Culture: Learning from Failure* | Failed deployment recovery time (incidents caused by a deployment only) | «расширяет» (extends) | Roles, severity levels and blameless reviews match. The kit measures detection and recovery per severity, closure and recurrence of action items, and treats data-correctness defects apart from outages, with their own detection time. The books carry more of the culture: shared postmortems, practice drills |
| 5 | On-call health | Book ch. 11 *Being On-Call*; Book ch. 29 *Dealing with Interrupts*; Workbook ch. 8 *On-Call* | None (DORA capability: well-being) | «совпадает» (matches) | The kit's default of at most two incidents per 12-hour shift comes from the books, as do backup and handover. The books also bound the share of time on call, the size of a rotation and compensation; the kit does not score those |
| 6 | Change safety | Book ch. 8 *Release Engineering*; Workbook ch. 16 *Canarying Releases* | Deployment frequency, change lead time, change fail rate, failed deployment recovery time; deployment rework rate recorded when the data allows | «расширяет» (extends) | Progressive delivery with an automatic halt matches canarying. The kit gates level 4 on the DORA keys, which the books do not use; it does not check hermetic, reproducible builds, which the release engineering chapter stresses |
| 7 | Resilience architecture and testing | Book ch. 17 *Testing for Reliability*; Book ch. 21 *Handling Overload*; Book ch. 22 *Addressing Cascading Failures*; Workbook ch. 11 *Managing Load*; Workbook ch. 17 *Identifying and Recovering from Overload* | None | «совпадает» (matches) | Timeouts, retries with backoff, degradation, load shedding and game days match. The books go further on retry budgets, deadline propagation and request criticality; the kit checks that a mechanism exists, not its settings |
| 8 | Capacity and performance | Book ch. 1 *Introduction* (demand forecasting and capacity planning as an SRE duty); Book ch. 18 *Software Engineering in SRE* (intent-based capacity planning); Workbook ch. 11 *Managing Load* | None | «совпадает» (matches) | Forecasts and load tests match. The kit does not ask for redundancy headroom such as N+2, or for efficiency and resource cost |
| 9 | Backup and disaster recovery | Book ch. 26 *Data Integrity: What You Read Is What You Wrote* | None | «расширяет» (extends) | Both say a restore, not a backup, is the proof. The kit is more specific: a restore counts only from the backups the RPO relies on, and RTO and RPO are set per tier-1 service |
| 10 | Operations model: readiness, ownership, toil | Book ch. 5 *Eliminating Toil*; Book ch. 30 *Embedding an SRE to Recover from Operational Overload*; Book ch. 32 *The Evolving SRE Engagement Model*; Workbook ch. 6 *Eliminating Toil*; Workbook ch. 18 *SRE Engagement Model* | None | «совпадает» (matches) | Readiness review, the 50% toil cap and support tied to meeting SLOs with handback come from the books. The kit holds them in one dimension; the books build a whole organisational model around them (see below) |

## Observability model, 12 dimensions

| # | Dimension | Closest in Google SRE | DORA | Label | How the kit relates |
|---|---|---|---|---|---|
| 1 | Business-function model | Workbook ch. 2 *Implementing SLOs* (user journeys as the basis of SLIs); Workbook ch. 3 *SLO Engineering Case Studies* | None | «расширяет» (extends) | The books start SLIs from user journeys. The kit decomposes each journey into business functions with owners, and at level 5 keeps the model with the code |
| 2 | Telemetry coverage | Book ch. 6 *Monitoring Distributed Systems*; Workbook ch. 4 *Monitoring* | None | «расширяет» (extends) | The books describe what to monitor. The kit measures coverage, the share of tier-1 calls linked end to end with the denominator stated, and makes it a release gate at level 5 |
| 3 | Error semantics | Workbook ch. 2 *Implementing SLOs* (which events are valid and which are good) | None | «добавляет» (adds) | The books touch it only through what an SLI counts. The kit asks for a full taxonomy: expected, failure, caller defect, ambiguous, unknown, with refusals inside HTTP 200 recognised |
| 4 | Correlation | Book ch. 12 *Effective Troubleshooting* (tracing in diagnosis) | None | «расширяет» (extends) | The books use tracing as a troubleshooting aid. The kit scores the linkage front end to service to downstream, and at level 5 its routine use, sampled in post-incident reviews |
| 5 | Business outcome data | Workbook ch. 2 *Implementing SLOs* (SLIs from logs and processing data) | None | «добавляет» (adds) | Outcome, error class and ids per attempt, used in SLIs, is a kit criterion with no chapter of its own in the books |
| 6 | SLOs | Book ch. 4 *Service Level Objectives*; Workbook ch. 2 *Implementing SLOs*; Workbook appendix A *Example SLO Document* | None | «совпадает» (matches) | SLOs on tier-1 functions, valid and good defined per SLI, budgets tracked: as the books describe. In the combined format the SLO is scored here once |
| 7 | Alert quality | Book ch. 6 *Monitoring Distributed Systems* (every page actionable); Book ch. 10 *Practical Alerting*; Workbook ch. 5 *Alerting on SLOs* | None | «расширяет» (extends) | The books ask that every page be actionable. The kit measures the actionable share against an agreed target and requires an owner, a sample rule and a runbook per alert |
| 8 | Incident response | Book ch. 14 *Managing Incidents*; Workbook ch. 9 *Incident Response* | Failed deployment recovery time | «совпадает» (matches) | Roles, severity levels and post-incident reviews match. In the combined format it is scored in SRE dimension 4 |
| 9 | Outside-in checks | Book ch. 6 *Monitoring Distributed Systems* (black-box monitoring); Workbook ch. 4 *Monitoring* (probers) | None | «совпадает» (matches) | Black-box probing matches. The kit adds checks per tier-1 journey from two locations and real-user data tied to SLOs at the upper levels |
| 10 | Release observability | Book ch. 8 *Release Engineering*; Workbook ch. 16 *Canarying Releases* (canary evaluation) | Change fail rate | «расширяет» (extends) | Canary evaluation compares signals before and after. The kit asks that every release prove its own signals against the spec, and gates the release at level 5 |
| 11 | Data governance and cost | Book ch. 6 *Monitoring Distributed Systems* (keeping monitoring simple and affordable) | None | «добавляет» (adds) | Retention, sampling, personal data and a telemetry budget per signal have no chapter in the books |
| 12 | Operational analytics | Book ch. 6 *Monitoring Distributed Systems* (prefers simple, predictable rules); Book ch. 7 *The Evolution of Automation at Google* | None | «добавляет» (adds) | The books are wary of learned thresholds. The kit keeps the dimension optional and allows assisted remediation only on proven alert quality |

## What the kit adds of its own

| Item | Where | Relation to Google SRE | DORA | Label |
|---|---|---|---|---|
| Evidence per score and a confidence per score; interviews and documents alone cap a score at level 2 | [maturity-model.md](maturity-model.md#scoring-rules), rules 3 and 6; [claims rules](../tools/agent/rules/claims.md) | The books are evidence-minded in postmortems but set no rule for what proves a capability | None | «добавляет» (adds) |
| Not scored is not level 1 | [maturity-model.md](maturity-model.md#scoring-rules), rule 4 | No counterpart | None | «добавляет» (adds) |
| DORA keys as a level criterion | [sre/maturity-model.md](../sre/maturity-model.md), dimension 6 | The books predate the DORA metrics; Workbook ch. 1 *How SRE Relates to DevOps* is the nearest bridge | All four keys; rework rate recorded | «добавляет» (adds) |
| Data-correctness defects as their own incident class, with a detection time | [sre/maturity-model.md](../sre/maturity-model.md), dimension 4 | Book ch. 26 *Data Integrity* covers the defects, not their measurement as incidents | None | «расширяет» (extends) |
| A profile with no overall average | [maturity-model.md](maturity-model.md#scoring-rules), rule 7 | The books do not score maturity | None | «добавляет» (adds) |
| Linkage to the AWS Well-Architected Framework | [sre/maturity-model.md](../sre/maturity-model.md#mapping-to-external-frameworks) | Outside the books | None | «добавляет» (adds) |
| Default thresholds the client confirms or replaces, recorded as decisions | [sre/maturity-model.md](../sre/maturity-model.md), default thresholds | The books give the defaults as Google's own practice | None | «расширяет» (extends) |

## What Google SRE covers that the kit does not

Areas the books treat as practice and the kit does not score, or scores only in passing. They are
recorded here so a reader does not assume them covered; adding any of them is a backlog decision.

1. **SRE team topology and lifecycle**: how an SRE team forms, engages and hands back. Workbook
   ch. 18 *SRE Engagement Model*, ch. 20 *SRE Team Lifecycles*, ch. 21 *Organizational Change
   Management in SRE*; Book ch. 30 and ch. 32. The kit holds only readiness, ownership and toil
   (SRE dimension 10).
2. **Simplicity** as a reliability principle: removing code and features, small interfaces.
   Book ch. 9 *Simplicity*; Workbook ch. 7 *Simplicity*.
3. **Culture, training and psychological safety**: onboarding to on-call, practice drills,
   shared postmortems, stress on call. Book ch. 11 *Being On-Call*, ch. 15 *Postmortem
   Culture*, ch. 28 *Accelerating SREs to On-Call and Beyond*; Workbook ch. 8 and ch. 10.
4. **Load balancing** at the front end and in the data centre. Book ch. 19 *Load Balancing at
   the Frontend*, ch. 20 *Load Balancing in the Datacenter*. Touched only through SRE
   dimension 7.
5. **The cost of reliability**: every further nine has a price, and 100% is the wrong target.
   Book ch. 3 *Embracing Risk*. The kit asks for SLOs and budgets, not for the cost of a target.
6. **Narrower practices** the kit leaves out: emergency response (Book ch. 13), interrupts
   (Book ch. 29), automation (Book ch. 7), data processing pipelines (Book ch. 25, Workbook
   ch. 13), configuration design (Workbook ch. 14 and ch. 15), large system design (Workbook
   ch. 12), and on-call limits beyond incidents per shift, such as the share of time on call and
   the size of a rotation (Book ch. 11 *Being On-Call*).
