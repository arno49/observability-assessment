# Agent data handling

**What is here:** what happens to the client's data when an assessment runs on the
[agent route](agent-harness.md), four ways to arrange it, the one recommended by default and a
checklist for it. **Read it when:** a proposal offers the agent route, before the statement of
work is signed, and again at kick-off before any access is granted.

## Why it matters

An agent on read-only access changes nothing in the client's estate, but it reads. Every file,
export, query result and log line it opens enters the context of a model: traces, logs,
configuration, alert rules, incident timelines, and in them, often, personal data and secrets
the estate was never meant to hold.

- **Read-only access is not a legal shield.** It limits what the agent can change, not what it
  can see. Reading personal data is processing it, whoever reads it and however briefly.
- **The agent route makes the assessment a data processing activity.** The model provider, and
  anyone it relies on, receives the client's data on the engagement's behalf. That has to be
  arranged before launch, contractually and technically: in the statement of work
  ([sow.md](../templates/sow.md), confidentiality and data handling) and the data processing
  agreement, and at kick-off ([kickoff-note.md](../templates/kickoff-note.md)), before stage 1
  starts.
- **The agent's output carries the same data.** Drafts, scratch files, progress logs and copies
  of evidence can quote what the model read. They fall under the same rules as the inputs.
- **The prompt is not the control.** The boundary rules in the stage prompts say what the agent
  must not print; where data goes is set by the arrangement below and by the agent tool's own
  permissions ([setup.md](../tools/agent/setup.md)).

The [privacy checklist](../templates/privacy-checklist.md) covers the data the engagement
collects; this page covers the model the agent sends it to.

## Four options

They are not exclusive: option 2 combines with 3 or 4, and those combinations are often the best
fit. Whatever is chosen is a row in the decision register, agreed by the client's data owner,
before access is granted.

### 1. Self-hosted model inside the client's perimeter

**How it works.** The model runs on infrastructure the client controls (its own data centre or
its own cloud account), and the agent tool calls it there. Nothing leaves the client's network.

| Pros | Cons |
|---|---|
| No third party receives the data; no new sub-processor | The client builds and runs the hosting; it costs time and money before the assessment starts |
| Simplest legal position: the data stays where it already is | Models that can be self-hosted may draft less well; the harness measures (accepted drafts, reproduced evidence) can drop |
| Fits a client that bans external AI services outright | The engagement depends on the client's platform team for capacity and uptime |

**When to use.** Regulated or classified estates where no external processing is allowed, or a
client that already runs a model internally and has approved it for this use.

### 2. API provider under a DPA with a no-training, limited-retention commitment

> **Recommended** as the default for clients who prioritise data privacy and safety.

**How it works.** The agent calls a commercial model API under a contract: a data processing
agreement with the provider, a commitment that inputs and outputs are not used to train models,
and a stated retention period for them (zero retention where the provider offers it). Region and
sub-processors are fixed in that contract. The engagement, in turn, holds a data processing
agreement or equivalent clause with the client that names the provider.

| Pros | Cons |
|---|---|
| The strongest models, so the harness measures hold as recorded | A third party processes the data; it must be named, approved and contracted |
| Ready in days if the client already approves the provider | Commitments differ by provider, plan and region; they are read in the contract, not assumed from a web page |
| The obligations are written down and auditable | The client's legal and security review can take longer than the assessment's first stage |

**When to use.** Most engagements where the client accepts an approved cloud processor and wants
the agent route's speed without giving up control of its data.

**Checklist before launch.** Each item is answered in writing and recorded in the decision
register; an item left open blocks stage 1.

1. **Which data enters the model context.**
   - [ ] An inventory per input class: code, monitoring exports, alert rules, logs, traces,
         incident and ticket exports, captures, billing.
   - [ ] For each class, whether it can hold personal data or secrets, and what is done about it.
   - [ ] Minimisation: the agent reads only the paths and systems the stage lists; the
         stage inputs are cut to what the checks need (a window, a sample, the services in scope).
   - [ ] Classes that never go to the model, named (for example request bodies, free-text
         fields, session tokens).
2. **Where the model runs.**
   - [ ] The provider's processing region and data residency, matching the client's
         requirement.
   - [ ] The provider's sub-processors, listed and accepted by the client.
   - [ ] The agent tool itself: where it runs, what it logs, what it sends besides the model
         calls.
3. **A DPA with the provider, and with the client.**
   - [ ] A data processing agreement between the engagement (or the client directly) and the
         provider, covering this use.
   - [ ] The client's agreement that names the provider, the purpose, the data classes and the
         period: the statement of work's data handling section, or a separate DPA.
   - [ ] Who notifies whom of a breach, and within what time.
4. **No training, and a retention period.**
   - [ ] A written commitment that inputs and outputs are not used to train or improve models.
   - [ ] The retention period for prompts and outputs on the provider's side, and whether it can
         be zero; any exception (abuse monitoring, legal hold) named.
   - [ ] The commitment checked on the plan and region actually used, not on a general page.
5. **How agent outputs are stored and processed.**
   - [ ] Location: drafts, scratch, progress logs and evidence copies live only in the
         engagement repository and the scratch directory, in a store the client approved.
   - [ ] Access: named people only; the same access the inputs have, no wider.
   - [ ] Content: outputs record that a value exists, not the value; query text and windows
         are kept, results with personal data are not.
   - [ ] Retention and deletion: the scratch directory and raw copies are deleted at
         hand-over, the rest returned or deleted as the contract states, and the deletion
         recorded.

### 3. Masking and redaction before anything is sent to the model

**How it works.** A step between the client's sources and the agent masks or removes personal
data and secrets: names, e-mail addresses, identifiers, tokens, IP addresses, free text.
The agent sees only the redacted copy.

| Pros | Cons |
|---|---|
| Lowers what reaches the model whichever model is used | Redaction misses things: structured fields are caught, values inside free text and payloads often are not |
| Can be checked: the redacted copy is the evidence of what was sent | Masked values can break the analysis: correlation across services needs a stable pseudonym, not a blank |
| Reuses rules the privacy checklist already asks for | Building and testing the rules takes engineer days; an untested rule is a false comfort |

**When to use.** Together with option 2 when the estate holds personal data in telemetry, or
alone with option 1 as defence in depth. Never as the only control for data that must not leave
the client.

### 4. Hybrid: metadata and aggregates to the model, raw data stays with the client

**How it works.** The model receives structure, not content: object inventories, alert and
dashboard definitions, metric names and labels, counts and rates over a window, error-class
distributions. Raw logs, traces and records stay in the client's tools. When a check needs a
fragment, the agent asks for it by name, a person or a client-side tool runs the query, and only
the fragment needed is passed back, recorded as evidence.

| Pros | Cons |
|---|---|
| Most of the inventory, gap and maturity work needs only metadata and aggregates | Slower: each fragment is a request and a wait, which the calendar has to absorb |
| The data that reaches the model is small and known | Needs a person or a tool on the client side to answer fragment requests |
| Fits the evidence rule: each fragment is a cited query with a window | Findings that rest on reading code paths or payloads need more fragments, or a person |

**When to use.** Estates where raw telemetry is sensitive but its structure is not; often with
option 2 for the model and option 3 on the fragments.

## Choosing

| The client... | Start with |
|---|---|
| Accepts an approved cloud processor | 2, with 3 on telemetry that holds personal data |
| Accepts a processor, but not raw telemetry leaving its tools | 2 with 4 |
| Allows no external processing | 1, with 3 as defence in depth |
| Has no answer before the start date | No agent route yet: people run the stages until an option is agreed |

Whichever is chosen, the harness measures are read in its light: a weaker self-hosted model or
heavy redaction can lower the share of drafts accepted, and that is recorded, not hidden
([agent-harness.md](agent-harness.md), measuring the harness).
