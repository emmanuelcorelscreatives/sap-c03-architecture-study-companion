# SAP-C03: architecture study workflow

## Work one decision at a time

Use a 45-minute paper exercise; no AWS account is required.

1. **Capture the requirement, 5 minutes.** Pick an objective from the current official guide and write one business operation. Include recovery, security, consistency and cost constraints.
2. **Draw the paths, 10 minutes.** Show caller identity, forward/return network flow, data copies and deployment control. Label account and Region boundaries.
3. **Compare two designs, 10 minutes.** For each, list what it satisfies, its remaining assumptions and its operator obligations. Reject on a specific constraint, not on service popularity.
4. **Test one failure on paper, 10 minutes.** Remove a dependency, introduce duplicate delivery or deny a required key operation. State the expected observation and recovery action. Mark this as analysis, not an executed drill.
5. **Verify and summarize, 10 minutes.** Read the appropriate official service documentation. Write one changed assumption and a concise decision record.

## Rotate through the five review lenses

| Lens | Required artifact from your session |
|---|---|
| Architecture design | A diagram showing the critical operation and its state/data boundaries |
| Security and governance | A principal/action/resource/policy map, including key access |
| Cost optimization | A list of measured usage inputs and costs requiring current regional prices |
| Resilience and migration | RPO/RTO definitions, recovery dependencies and a cutover/rollback boundary |
| Operational excellence | A change plan with per-target readback, observable success and failure handling |

The lenses are a study method, not a replacement blueprint. Check the exact current domain/task list in the official guide. Include AI/ML integration and security where relevant instead of treating AI as an isolated vocabulary exercise.

## Reusable decision record

```text
Objective and date:
Business operation / caller:
Mandatory constraints:
Unknowns needing clarification:
Accounts, Regions and trust boundaries:
Identity / network / data / control paths:
Design A: strengths, violations, operating work:
Design B: strengths, violations, operating work:
Decision and decisive constraint:
Failure case and expected evidence:
Recovery / rollback / reconciliation:
Cost inputs still needing verification:
Official reference and applicability limits:
Paper analysis or actual authorized test:
What would make me choose differently:
```

Keep secrets, account identifiers and employer diagrams out of shared notebooks. Use invented names and sanitized values.

## Review mistakes by reasoning defect

Useful categories include: wrong principal, missing return route, wrong consistency scope, retry/duplicate confusion, recovery-copy confusion, unmeasured cost assumption, unsupported service mode and accepted-versus-completed automation. A category tells you what to study next; a copied answer letter does not.

For an uncertain correct response, explain why the strongest alternative fails a mandatory constraint. If you cannot do that, mark the concept for remediation rather than treating the percentage as mastery. Return after a delay and change the constraints to see whether your decision changes appropriately.

Optional hands-on work belongs in an authorized sandbox with a budget, data restrictions and teardown verification. Do not experiment with immutable retention, production organization controls or live migration cutovers merely to complete a study checklist. No deployment or drill is claimed by these materials.
