# AI Maintenance Engineer: engineering notes

A coding agent can produce a persuasive diff without fixing the defect. AI Maintenance Engineer separates the agent's work from the checks that decide whether the patch is acceptable.

## System path

~~~mermaid
flowchart LR
  Issue --> IsolatedRepository
  IsolatedRepository --> AgentToolLoop
  AgentToolLoop --> ProposedPatch
  ProposedPatch --> IndependentChecks
  IndependentChecks --> Reviewer
  Reviewer --> LocalExport
~~~

This diagram describes the demonstrated system boundary, not a production deployment topology.

## Decisions and tradeoffs

### Keep the repository inside a sandbox

The live agent works through a bounded toolbox in Docker. Path and execution boundaries protect the host and unrelated projects.

### Keep acceptance evidence outside the agent's control

Protected acceptance cases, regressions and typechecking judge the proposed patch independently.

### Make the result inspectable

The reviewer sees the changed file, checks and preview before a local export; no automatic merge or real customer-repository action occurs.

## Review questions

- What evidence demonstrates **keep the repository inside a sandbox**, and what would fail without it?
- What evidence demonstrates **keep acceptance evidence outside the agent's control**, and what would fail without it?
- What evidence demonstrates **make the result inspectable**, and what would fail without it?

## Verification boundary

A fresh recorded repair changed one time-handling module. 12 protected acceptance cases, 23 regression cases, typecheck and preview passed for that repair. Shared live-agent verification included 37 nonpaid tests and eight Docker boundary checks across Maintenance and Readiness.

The successful repair is one controlled task, not a general claim that arbitrary bugs are solved. A saved recording is not presented as a new live run.
