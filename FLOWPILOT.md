# FlowPilot

FlowPilot is an adaptive backpressure controller for background workloads. Its goal is to keep a downstream dependency productive when incoming work exceeds the capacity that dependency can safely handle.

## The problem

Workers can continue taking jobs while PostgreSQL or an external API slows down. In-flight operations accumulate, time out, and retry. Those retries add load and can prolong the overload. FlowPilot will observe downstream health and adjust how much work starts, how retries are admitted, and how available capacity is shared between customers.

## First research question

Can a controller adjust worker concurrency in response to PostgreSQL health so that useful throughput and recovery improve during overload, without unstable limit changes or starvation?

The concrete workload, service targets, and control policy are deliberately undecided. We will research and justify those decisions before implementing them.

## Questions to answer before architecture

1. What job does a worker execute, and which operation stresses PostgreSQL?
2. What are the arrival rate, service time, and safe concurrency under normal conditions?
3. How long may accepted work wait, and what happens when its deadline passes?
4. Which signals reveal overload soon enough to act?
5. How should retries share capacity with first attempts?
6. What does fair treatment mean for customers and job priorities?
7. How can workers enforce a global limit and behave when control data is unavailable?
8. How will the controller detect recovery while work is restricted?

## Demonstration

Compare three conditions: no protection, a fixed concurrency limit, and adaptive control. Increase load, inject database latency, then remove the fault. Measure successful completions, end-to-end p99 latency including queue time, error rate, queue age, fairness, and recovery time.

## Candidate stack

TypeScript, Node.js, PostgreSQL, Redis, SQS, ECS, CloudWatch, OpenTelemetry, and k6. Each component should be selected only when a requirement calls for it.
