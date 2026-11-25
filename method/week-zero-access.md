# Week-zero access gate

**What is here:** how read-only production access (a token, a read-only MCP server, exports) is
arranged before an assessment starts, the options a client chooses between, the fallback when
neither fits, and what the client's own hours for it should cover. **Read it when:** you scope a
proposal, write the statement of work, or prepare the kick-off.

## Why a gate before day one

Every format starts by reading: the code, the monitoring estate, the delivery pipeline, the
incident history. Without verified read-only access there is nothing to read, and the calendar
has no slack to absorb the wait.

- **The focused format has no buffer for access delays.** In two to three weeks the calendar
  asks for every data-request item in its first week
  ([getting-started.md](getting-started.md#3-calendar)); a security review or a token that
  arrives late eats evidence work the format cannot give back. The estimation model forbids
  compressing evidence work ([estimation.md](estimation.md), capacity check).
- **A missed client obligation moves the date, not the price.** This is the kit's existing rule:
  [sow.md](../templates/sow.md#4-client-obligations), section 4 "Client obligations", and
  [estimation.md](estimation.md), step 5 of "How to estimate". Access is the first of those
  obligations.
- **So access is a gate, not a task.** If access is not verified by the end of week zero, the
  start date moves. The lead records the miss the day it happens, with the new date, and tells
  the sponsor, as section 4 of the statement of work says.
- **Getting access costs the client time.** Security review, legal review and provisioning are
  hours the client spends before anyone reads anything. The estimation model counts only our
  days; the proposal states the client's hours as well, so nobody is surprised by them.

"Verified" means a person on our side has used each access path once, read-only, and seen it
return what the [data request](../templates/data-request.md) needs; a token issued but never
tried is not access.

## Options the client chooses during scoping

Options 1 and 2 are equal choices: the client takes whichever is easier for its organisation,
and the choice is a row in the decision register and a line in the proposal. Option 3 is the
fallback when neither fits.

### 1. Week zero as its own phase

**How it works.** A short phase before day one, in the proposal and the statement of work with
its own dates. The client runs its usual approvals; we send the
[data request](../templates/data-request.md) on the first day, answer the security and legal
questions, and verify each access path as it arrives. The assessment's calendar starts only when
the gate below passes.

| Pros | Cons |
|---|---|
| Works with any client process, however slow; nothing is bypassed | Adds calendar time before the assessment starts |
| The client's hours are visible in the proposal | Needs a named client owner for each access path, or it stalls |
| The calendar of the assessment itself stays intact | Long approvals still delay the start; they just stop eating the audit |

**When to use.** Clients with a formal security review, a vendor-onboarding process or a
first engagement with us.

### 2. Pre-approved standing access

**How it works.** The client grants access once, through a mechanism it already governs: a
workload or managed identity, or a pre-approved read-only role in the monitoring tool, the code
host and the ticketing system, scoped to the services in scope and time-boxed to the engagement.
Each tool and export then uses that identity, without a fresh approval per system.

| Pros | Cons |
|---|---|
| One approval instead of many; fastest when the client already has the pattern | The role must be scoped tightly; a broad read-only role reads more than the scope needs |
| Easy to revoke at hand-over: one identity, one end date | Not every tool supports it; exports may still need a person |
| Reusable for a Blueprint or Implementation that follows | Setting it up the first time can take as long as option 1 |

**When to use.** Clients that already grant contractors access through managed identities or
standard read-only roles, and repeat clients.

Either way, the agent route adds its own decision on where the data goes; see the kit's agent
data-handling page (method/agent-data-handling.md) where it is available.

### 3. Fallback: start on staging or a read replica

**How it works.** When neither option can be ready in time, the minimal viable audit starts on
what can be reached: a staging environment, a read replica, or exports the client sends.
Production read-only access becomes an optional upgrade, added by change request when it
arrives.

What it loses:

- **Real traffic.** Rates, volumes, error mixes and denominators on staging say little about
  production; SLIs and thresholds read there are not production facts.
- **Real alerts.** Paging history, firing, notification routes and actionable share exist only
  where production alerts fire.
- **Production-only configuration.** Sampling, retention, alert rules, integrations and
  dashboards often differ between environments; a staging read does not show production drift.

How findings are marked: each finding and each maturity score names the environment it was read
in. A claim read on staging or a replica is bounded to it ("on staging, in the checked
window"), keeps its status as an assumption for production, and lowers the confidence of any
score that rests on it. Production-dependent checks are listed as "not scored" with the reason,
never as level 1 ([maturity-model.md](maturity-model.md#scoring-rules), rules 4 and 6).

**When to use.** Only when options 1 and 2 cannot be ready, and the sponsor accepts in writing
what the audit will not be able to say.

## Client hours to put in the proposal

The client's own effort, estimated with the client at scoping and written into the proposal and
the statement of work as an obligation. The ranges are **starting values**, not measured
actuals, in the sense of [estimation.md](estimation.md): record the actual hours of each
engagement next to them and recalibrate.

| Item | Typically done by (client side) | Hours, starting values |
|---|---|---|
| Security review of the engagement and its access paths (scope, read-only proof, where data goes) | Security or information-security team | 4–16 |
| Legal and data-protection review: confidentiality, data processing agreement, the agent route's model provider if used | Legal, data protection officer | 2–12 |
| Access provisioning: tokens, roles or a managed identity per system (monitoring tool, code host, ticketing, delivery pipeline, backup console) | Platform team, tool administrators | 2–8 per system |
| Verification with us: each path tried once, gaps fixed | Tool administrators | 1–2 per system |
| Exports that cannot be pulled through access (incident history, billing, paging) | Tool owners, finance for billing | 2–6 |
| Owner time for answers and owner-question batches during the assessment | Technical owners, sponsor | 1–3 per batch |

**Checklist for the proposal**

- [ ] The option chosen (1, 2 or fallback 3) is written in the proposal and recorded as a
      decision.
- [ ] Each row above has a client owner by name or role, and a range agreed with the client.
- [ ] The access paths match the [data request](../templates/data-request.md) and the access
      dates in the [kick-off note](../templates/kickoff-note.md).
- [ ] The statement of work's client obligations ([sow.md](../templates/sow.md#4-client-obligations))
      list access as verified before day one, and repeat that a miss moves the date.
- [ ] Inputs from the client in [offerings.md](../offerings.md) are restated with the chosen
      option and its hours.
- [ ] The first step of the [playbook](playbook.md) is scheduled from the day the gate passes,
      not from the signature date.

## The gate

The assessment's day one is the first working day after all of these hold:

1. Each access path in the data request has been used once by our side and returned what the
   request needs, or has been moved to fallback 3 with the sponsor's written agreement.
2. Access is read-only, scoped to the services in scope, and has an end date.
3. The legal and data-protection items are signed.
4. The verification is recorded, with its date, in the engagement's facts ledger.

If the gate has not passed by the end of week zero, the start date moves; the price does not.
