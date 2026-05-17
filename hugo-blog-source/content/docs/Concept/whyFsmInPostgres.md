---
title: "2. Why FSM in Postgres"
weight: 37

---

# Why FSM in Postgres

If you're new to the concept of **Finite State Machines (FSMs)**, check out my previous blog post: [Why Finite State Machines Are Everywhere: From Software to Human Life](https://nirajkashyap.github.io/posts/why-finite-state-machines-are-everywhere-from-software-to-human-life/). That post explores how FSMs are fundamental to both software systems and real-world processes.

---

## Finite State Machine: In Various Languages

FSMs as a concept have been implemented in many programming languages, each with its own data structures and libraries. For example:

- **TypeScript/JavaScript**: [xstate](https://xstate.js.org/) is a popular library for modeling FSMs and statecharts.
- **Python**: Libraries like [transitions](https://github.com/pytransitions/transitions) provide easy-to-use FSM implementations.
- **Java**: Frameworks such as [Spring State Machine](https://projects.spring.io/spring-statemachine/) offer robust FSM support.
- **C#**: Libraries like [Stateless](https://github.com/dotnet-state-machine/stateless) are widely used for FSMs.
- **Go**: Libraries like [looplab/fsm](https://github.com/looplab/fsm) provide simple and effective FSM implementations for Go.

These libraries use different data structures—objects, classes, state tables, or even domain-specific languages—to represent states and transitions, but the core principles remain the same.

---

## Finite State Machine: In PostgreSQL

Finite State Machines (FSMs) are powerful tools for modeling workflows, processes, and business logic that involve a series of states and transitions. While FSMs are often implemented in application code, PostgreSQL's advanced features—such as triggers, constraints, and procedural functions—make it possible to manage state transitions directly within the database.

Using a finite state machine (FSM) with PostgreSQL means:

- Modeling your workflow or business process as states and transitions
- Letting the database enforce correctness, persistence, and history

>This is powerful because PostgreSQL becomes the single source of truth for both data and state.

---

### Why an FSM in PostgreSQL is Useful

**1. Strong Data Integrity**

- Define allowed states and transitions (e.g., `pending → approved`, `pending → rejected`) in tables or constraints
- PostgreSQL can prevent invalid state changes at the database level (via checks, triggers, or dedicated FSM extensions), so your app cannot accidentally jump from `shipped` back to `pending`

**2. Built‑in Durability and History**

- Every state change can be recorded as a row in an audit or history table
- Because Postgres is ACID‑compliant, you get crash‑safe, transactional state updates—exactly what durable workflows need

**3. Simpler, More Auditable Workflows**

- Centralize state logic in the database instead of scattering it across services
- Query "where is this order right now?" or "what path did this workflow take?" using plain SQL, making debugging and compliance easier

**4. Scalable Coordination**

- Multiple services or workers can read and update the same FSM‑backed records safely, because Postgres handles locking and consistency
- Libraries and extensions or custom SQL‑based FSMs let you define states, transitions, and even multi‑tenant workflows directly in Postgres

**5. Language‑Agnostic Integration**

- By defining your FSM in PostgreSQL, you create a single source of truth that any programming language can interface with
- Bring TypeScript, Python, Go, Java, or any other language into your system without redefining state machines—they all query and update the same database layer
- This dramatically reduces the friction when integrating new services or teams using different tech stacks, making it easy to adopt new programming languages as your architecture evolves

---

### How It's Typically Implemented

**Tables for States and Transitions**

- A `states` table lists all valid states
- A `transitions` table defines which `start_state + event` can lead to which `end_state`
- An `instances` table (e.g., orders, jobs) stores the current state of each workflow, and updates go through controlled transitions

**Enforcement via SQL / Extensions**

- Enforce transitions using `CHECK` constraints, triggers, or stored procedures
- Use an FSM extension that adds dedicated functions like `fsm.create_state` and `fsm.create_transition`
- Some Go or application‑level libraries build an FSM layer on top of Postgres, using a command‑style pattern where each state change is a persisted command that must be processed in order

---

In short, putting an FSM inside PostgreSQL gives you durable, auditable, and strongly consistent workflows that are easy to query and hard to break—making it a natural fit for durable execution and human‑like resilient systems.
