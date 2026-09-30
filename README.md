# FlowPilot

FlowPilot is an adaptive backpressure controller for background workloads. It will observe downstream health and adjust worker concurrency, retry admission, and capacity sharing between customers when incoming work exceeds what a dependency can safely handle.

## Current stage

Research and problem definition. FlowPilot does not yet have application code, a selected architecture, or a runnable demo.

Start with the [project brief](FLOWPILOT.md) for the research question, open decisions, candidate stack, and demonstration plan.

## First milestone

Define a concrete PostgreSQL-backed worker workload, its service targets, and an overload experiment. Compare unprotected workers, a fixed concurrency limit, and adaptive control using successful completions, end-to-end p99 latency, error rate, queue age, fairness, and recovery time.

Select the architecture and implementation stack after those requirements are clear.
