# ADR-0001: Adopt a Modular Monolith Architecture

## Status
Accepted

## Context
This project is being built as a backend portfolio project focused on core commerce flows such as product discovery, cart, ordering, inventory, and operational reliability.

At this stage, the priority is to keep the system easy to develop, test, refactor, and explain clearly. The domain boundaries are expected to evolve during implementation, so the architecture should support fast iteration without unnecessary operational complexity.

## Decision
We will adopt a modular monolith architecture.

The system will run as a single application and be deployed as a single unit, while enforcing clear module boundaries in code. Internal modules such as catalog, cart, ordering, inventory, and operations should remain explicitly separated in package structure, application services, and dependencies.

Modules should communicate through explicit interfaces and avoid unrestricted direct access to each other's internal implementation.

## Rationale
We are choosing a modular monolith because it is the best fit for the current phase of this project.

- It keeps implementation and deployment simple while the product scope is still evolving.
- It allows faster iteration on domain boundaries without the cost of service-to-service contracts and distributed coordination.
- It keeps commerce flows with tight consistency requirements easier to implement, especially around cart, ordering, and inventory interactions.
- It avoids introducing network boundaries before there is a real scaling, ownership, or release management need.
- It is easier to test, debug, and explain in a portfolio setting than an early microservices architecture.

We are not choosing microservices now because the typical reasons to split services do not exist yet.

- There are no independent teams that need isolated ownership or release cycles.
- There is no proven module that requires independent scaling.
- Distributed transactions, inter-service communication, observability, deployment pipelines, and failure handling would add complexity too early.

## Consequences

### Positive
- Local development remains straightforward.
- Testing and debugging stay simpler because the system runs in one process.
- Transaction handling is easier for closely related commerce use cases.
- The codebase can still express domain boundaries clearly if modules are enforced consistently.
- Future extraction remains possible because boundaries are defined intentionally from the start.

### Negative
- The system remains a single deployment unit.
- A mistake in one module can affect the whole application.
- Independent scaling by module is limited.
- Module boundaries must be enforced by discipline, architecture rules, and code review rather than infrastructure isolation.

## Alternatives Considered

### Microservices
Rejected for now.

Microservices would create early overhead in service communication, data consistency, deployment, monitoring, and troubleshooting. That tradeoff is not justified yet for a single evolving codebase and a portfolio project whose priority is clear design and reliable core flows.

### Layered Monolith Without Explicit Module Boundaries
Rejected.

A plain layered monolith would be simpler at first, but it tends to blur domain responsibilities over time. This project needs explicit module boundaries so that growth in catalog, ordering, inventory, and operations does not collapse into one large shared service layer.

## Future Evolution
This decision is intentionally reversible.

We may extract one or more modules into separate services later if one or more of the following become true:

- a module requires independent scaling characteristics
- a module needs an independent release cycle
- ownership is split across separate teams or clear bounded contexts
- operational or reliability requirements justify stronger isolation
- integration boundaries become stable enough that service extraction reduces complexity instead of increasing it

Until those conditions are real, the default approach is to keep a single deployable application with strong internal modular boundaries.
