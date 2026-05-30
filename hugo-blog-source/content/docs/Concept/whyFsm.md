---
title: "1. Why FSM"
weight: 36

---

**Why Finite State Machine ( State Chart )** 

## 1. What is a diagram

A diagram is a visual model of behavior, structure, or flow — it helps people share understanding faster than plain text.

A good diagram is:

- **Visual** — gives a shared picture of behavior instead of only linear text
- **Helpful** — improves understanding, communication, or analysis for a specific audience


## 2. Visual Diagrams Are Better Than Text

When complexity grows, text-based logic becomes hard to follow. Diagrams are better because:

- Scattered `if/else` blocks and reducers are hard to audit at a glance
- Prose specs lose relationships, order, and boundaries
- Visual layouts show state, flow, and ownership in one view


## 3. Which Visual Diagram is better for software ?

Two standards dominate: **UML** (for software design) and **BPMN** (for business processes). Within UML, Statecharts are the closest to actual runtime behavior.

## 4. What is UML

UML (Unified Modeling Language) is a standardized modeling language used to describe software systems from multiple perspectives, such as structure, behavior, and interactions.


| Category | Diagram Type | Primary Purpose | Key Visual Elements |
| :--- | :--- | :--- | :--- |
| **Structural** | **Class Diagram** | Shows system classes, attributes, methods, and relationships. | Class boxes, lines (inheritance, association). |
| | **Object Diagram** | Captures a snapshot of class instances at a specific moment. | Named instances, link lines, attribute values. |
| | **Component Diagram** | Illustrates physical software modules and their dependencies. | Components, interfaces, lollipop connectors. |
| | **Deployment Diagram** | Maps software components to physical hardware infrastructure. | Nodes (3D cubes), artifacts, device paths. |
| | **Package Diagram** | Groups elements into packages to organize large systems. | Folder-shaped containers, dependency arrows. |
| | **Composite Structure** | Explores internal class structures, ports, and connectors. | Internal parts, ports, interfaces. |
| | **Profile Diagram** | Customizes standard UML with domain-specific extensions. | Stereotypes, tagged values, constraints. |
| **Behavioral** | **Use Case Diagram** | Models high-level requirements and user interactions. | Actors (stick figures), oval use cases, system boundary. |
| | **Activity Diagram** | Flows through step-by-step business or logic processes. | Rounded action steps, decision diamonds, forks/joins. |
| | <span style="color:red">**State Machine / State Charts**</span> | <span style="color:red">Tracks how an object changes states from start to finish.</span> | <span style="color:red">State boxes, transition arrows, trigger events.</span> |
| | **Sequence Diagram** | Details time-ordered message exchanges between objects. | Lifelines (dashed lines), activation bars, message arrows. |
| | **Communication Diagram** | Shows structural organization of message-exchanging objects. | Numbered message sequences, object boxes, links. |
| | **Timing Diagram** | Analyzes object state changes against precise time constraints. | Waveform timelines, duration constraints, state axes. |
| | **Interaction Overview**| Controls flow between different interaction segments. | Activity nodes containing sequence/communication pieces. |



<span style="color:red">Statecharts are close to execution behavior because legal moves are explicit and testable.</span>



## 5. What is BPMN

BPMN (Business Process Model and Notation) is a process notation standard focused on business workflows — readable like a flowchart but with richer semantics.

The notation defines core building blocks:

- Tasks: units of work performed by people, services, or bots.
- Events: start, intermediate, and end markers that capture triggers and outcomes.
- Gateways: decision points with explicit routing behavior.
- Message flows: interactions between participants or external systems.
- Pools and lanes: ownership and responsibility boundaries.

On its own, BPMN is descriptive. It needs an execution engine to run workflows.

### BPMN as a diagram tool

Common diagram tools: **Camunda Modeler**, **bpmn.io**, **Bizagi Modeler**.

These validate shapes, swimlanes, and connections to keep exported BPMN XML standards-compliant.

> Think of them as CAD tools for process design: great for blueprints, not for runtime execution.

### BPMN inside workflow engines

Executing a process requires an engine such as Camunda Platform, Flowable, or Zeebe. These engines take BPMN definitions and:

- Parse the diagram into executable state transitions.
- Step through tasks, timers, and events while persisting state.
- Call APIs, wait for external callbacks, and manage retries or escalations.
- Coordinate humans and services through request/response patterns.

Without an engine, a BPMN file is still just a diagram and nothing in your stack actually moves.

The closest functional equivalent to BPMN within UML is the UML Activity Diagram,

## 6. BPMN vs. UML

| Metric | BPMN (Business Process Model and Notation) | UML (Unified Modeling Language) |
| :--- | :--- | :--- |
| **Primary Focus** | Business processes, operational workflows, and business strategies. | Software architecture, object-oriented design, and system development. |
| **Target Audience** | Business analysts, process owners, and business managers. | Software engineers, system architects, and developers. |
| **Scope** | Strictly focused on business workflows and orchestration. | Broad scope covering structure, behavior, and system interactions. |
| **Execution** | Can be directly executed via process engines using BPEL/BPMN XML. | Used primarily for human-readable design documentation and blueprints. |





### StateCharts win over others

Not all diagrams are equal. Flowcharts let you draw arrows between any box — silently misrepresenting the system. David Khourshid (Stately.ai) calls this the **"pointless arrows" problem**.

Statecharts solve this with **visual formalism** (David Harel): every transition requires an explicit event and valid source/target. Impossible transitions cannot exist. The result:

- The diagram *is* the specification — not a copy of it
- Impossible states are unrepresentable by construction
- Diagrams remain in sync with runtime behavior, enabling automatic test generation
- Non-developers can read statecharts and understand what a system does and what can happen next

[Stop Drawing Pointless Arrows — David Khourshid, YOW! 2024](https://www.youtube.com/watch?v=d3hVTXrV7ms)


## 7. What is workflow

A workflow is the end-to-end progression of work across tasks, decisions, waiting periods, and outcomes. It can involve humans, services, data updates, and time-based rules.

In production systems, workflow quality depends on more than visual clarity:

- **Determinism** — same inputs always produce the same state transitions
- **Recoverability** — runs can resume after crashes or deploys without losing progress
- **Observability** — current state and history are always inspectable
- **Safe retries** — failed steps can be replayed without side-effect duplication

### Workflow conversations layers

Most confusion around "workflow" comes from mixing up 3 distinct layers:

1. How we <span style="color:red">sketch</span> a process 
2. which tools <span style="color:red">validate</span> the sketch 
3. what actually <span style="color:red">runs</span> in production

### workflow from BPMN VS workflow from Statechart

Both can model the same business process, but they lead to different execution styles.

| Concern | Workflow from BPMN (flowchart/activity/sequence style) | Workflow from statechart |
| --- | --- | --- |
| Primary artifact | Diagram + BPMN XML | State/event/transition model |
| Main strength | Business-readable process communication and governance | Deterministic runtime behavior and explicit transition rules |
| Execution mapping | Requires BPMN engine interpretation | Usually maps directly to runtime state transitions |
| Failure handling | Modeled with events/timers and engine features | Modeled as explicit transitions, guards, retries, and error states |
| Change impact | Diagram changes may be broad and governance-heavy | Transition-level diffs make behavioral impact easier to inspect |
| Best fit | Cross-team process visibility and compliance | Event-driven systems requiring strict behavioral control |


## 8. Recent shift toward code-first workflow definitions

Many teams no longer start with a diagram as the primary artifact. Instead, workflows are defined directly in code — step-by-step logic that reads like plain text but executes durably.

**How code-first platforms work:**

- Workflows are expressed in application code or platform-specific constructs
- The runtime handles persistence, retries, timers, callbacks, and crash recovery
- Execution state is durably persisted so runs resume after failures or deploys without losing progress

Platforms following this pattern: **Temporal**, **DBOS**, **Restate**, and **Vercel Workflows**.

Temporal was the first widely adopted open-source platform to popularize this pattern. Many newer platforms followed. Vercel Workflows is a recent addition — see ["A new programming model for durable execution"](https://vercel.com/blog/a-new-programming-model-for-durable-execution).

**Why engineering teams prefer this model:**

- Executable definition lives with the code that owns business behavior
- Workflow stays in version control alongside the rest of the application
- Changes move through normal CI/CD pipelines
- Step order is readable in code — no translation from a separate diagram
- Transition-level diffs make behavioral changes easy to review and audit
- The runtime still provides durable orchestration semantics behind the scenes

### Platform comparison: Temporal vs DBOS vs Vercel Workflows

| Aspect | Temporal | DBOS | Vercel Workflows |
| --- | --- | --- | --- |
| OSS/public start | OSS alpha release: 2020-03-30 | Public OSS SDK release: 2024-03-15 | Beta launch: Oct 2025, GA: 2026-04-16 |
| Git repository | temporalio/temporal | dbos-inc/dbos-transact-ts | vercel/workflow |
| Powerful feature | Deterministic replay with built-in durable retries for long-running workflows | Exactly-once durable workflow and queue processing integrated with app code | "use workflow" and "use step" model with managed durable execution and observability |
| Database/persistence under the hood | Pluggable persistence with PostgreSQL, MySQL, or Cassandra (plus visibility stores) | PostgreSQL-backed workflow state and checkpointing | Managed event-log + queue runtime; exact managed DB engine is not publicly specified; self-hosted reference implementation uses Postgres |
