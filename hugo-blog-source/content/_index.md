---
title: "pgFsm"
layout: landing
---

<div class="book-hero">

# Workflow State Machines, Powered by PostgreSQL {anchor=false}

Define states. Enforce transitions. Let Postgres do the rest.

{{< button href="/docs/get-started" >}}Get Started{{< /button >}}
{{< button href="/docs/Concept/whyFsmInPostgres" >}}Why pgFsm{{< /button >}}

</div>

--- 

## Capabilities

{{% columns %}}

- {{< card >}}
  ## Durable by Default
  ACID-compliant state transitions backed by PostgreSQL. Every state change is crash-safe, transactional, and fully auditable — no extra infrastructure needed.
  {{< /card >}}

- {{< card >}}
  ## Single Source of Truth
  The database enforces your workflow rules — not scattered application code. Define allowed transitions once; every service respects them automatically.
  {{< /card >}}

- {{< card >}}
  ## Language Agnostic
  TypeScript, Python, Go, Java — any language queries the same FSM layer. Bring new services into your system without redefining state machines.
  {{< /card >}}
  

{{% /columns %}}


{{% columns %}}

- {{< card >}}
  ### Strong Integrity
  Invalid transitions are blocked at the database level via constraints and triggers. Your application cannot accidentally skip a required state.
  {{< /card >}}

- {{< card >}}
  ### Built-in History
  Every state change is recorded as a row. Query "where is this workflow now?" or "what path did it take?" with plain SQL — no special tooling needed.
  {{< /card >}}

- {{< card >}}
  ### Scalable Coordination
  Multiple services and workers safely read and update shared FSM-backed records. PostgreSQL handles locking and consistency across all callers.
  {{< /card >}}

{{% /columns %}}

---

## How It Works

pgFsm models your business process as **states**, **events**, and **transitions** stored directly in PostgreSQL. When a transition fires, the database validates it against defined rules and records the change atomically — giving you deterministic, recoverable, observable workflows without a separate orchestration service.

| Concern | Flowchart / BPMN | pgFsm Statechart |
|---------|-----------------|-----------------|
| Execution | Engine interprets diagram | Direct runtime state transitions |
| Failure handling | Events, timers, engine retries | Explicit transitions, guards, error states |
| Change impact | Broad, governance-heavy | Transition-level diffs, easy to audit |
| Best fit | Cross-team process visibility | Event-driven systems with strict behavioral control |

---


## Client Libraries

Connect from any language using existing ecosystem libraries:

comming soon.
<!-- {{< badge style="default" title="TypeScript" value="xstate" >}}
{{< badge style="default" title="Python" value="transitions" >}}
{{< badge style="default" title="Go" value="looplab/fsm" >}}
{{< badge style="default" title="Java" value="Spring State Machine" >}} -->

> [!NOTE]
> pgFsm acts as the authoritative FSM layer in PostgreSQL. Client libraries handle local state modeling; pgFsm enforces correctness and persistence.

---

Ready to model your first workflow in Postgres?

{{< button href="/docs/get-started" >}}Get Started{{< /button >}}
