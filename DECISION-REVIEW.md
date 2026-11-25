# SAP-C03: practical architecture decision review

## Write the constraints before selecting services

Create a one-page decision record with: business outcome, callers, trust boundaries, existing estate, data classification, consistency/ordering, peak demand, recovery targets, retention, cost limits and operator capability. Mark each as **mandatory**, **preferred** or **unknown**. Unknown recovery or retention requirements need clarification, not a convenient assumption.

Choose the smallest architecture that satisfies the mandatory constraints. Then explain its failure modes and operational obligations. A feature-rich design can be worse if its staffing, migration or recovery requirements cannot be met.

## 1. Separate the architecture's paths

Draw four paths for a critical operation:

1. **Identity path:** how the caller obtains credentials and which principal is used.
2. **Network path:** name resolution, routing, endpoints and inspection in both directions.
3. **Data path:** which service reads/writes which copy, with what consistency and encryption.
4. **Control path:** how policy, infrastructure and application changes are approved, applied and observed.

A private endpoint addresses connectivity, not every authorization check. A central policy governs allowed actions, not whether packets can reach the service. A healthy control plane does not prove the application's data transaction completed.

### Hybrid traffic through centralized inspection

Start with source/destination CIDRs, the relevant VPC and transit-gateway route tables, attachment associations/propagations and the required inspection path. Trace both the forward and return direction. Stateful inspection can fail when the two directions take incompatible paths even though each side has a route.

Compare the actual route entries and appliance-mode requirements before replacing the inspection appliance or adding broad routes. A route-table diagram expresses intent; packet/flow evidence and the relevant deployment configuration establish what happened. Also include data processing and transfer costs for the selected path.

## 2. Diagnose multi-account access by policy layer

| Layer | Decision it answers | Tempting but insufficient substitute |
|---|---|---|
| Trust policy | Who may assume the role? | Giving the target role more action permissions |
| Identity/resource policies | Which principal may perform the operation on which resource? | Proving the principal can sign in |
| Permissions boundary/session policy | What ceiling applies to identity-based permissions in this context? | Adding another identity Allow outside that ceiling |
| Organization controls | What organization-level constraints apply? | Treating an SCP as a grant of workload permission |
| Key authorization | May this principal use the relevant encryption key for the operation? | S3 permission without the required KMS path |
| Endpoint and network controls | Is the request permitted through this connectivity path? | An otherwise valid IAM policy |

Evaluate the actual principal, resource, action and request context. Applicable explicit denies matter. Cross-account access may require cooperation on both sides; do not generalize a simplified same-account result to every resource-session or service integration.

For replicated encrypted data, inspect destination-key usability as well as replication settings. A second encrypted copy is not a usable recovery artifact if the recovery role cannot obtain the needed key access. Multi-Region key relationships do not mean that every regional key policy and permission is automatically interchangeable.

## 3. Match data behavior to the business operation

| Requirement | Review decision | Failure to avoid |
|---|---|---|
| Read-after-write correctness | Which exact read operation/copy supplies the required consistency? | Adding an eventually consistent cache and silently weakening the requirement |
| Repeatable delivery | Stable business identity and duplicate-safe side effects | Equating a transport acknowledgement with exactly-once payment or email |
| Ordered processing | Ordering scope, partition/group key and consumer behavior | Assuming one global order across independent partitions |
| Recovery to an earlier time | Recovery-point coverage, retention and restore path | Treating the current replica as an immutable historical backup |
| Regional evacuation | Writable endpoints, dependencies, key access and client reconnection | Moving DNS while the target data/service path is not ready |

For database failures, identify which endpoint the client actually uses, whether it can reconnect and whether the selected engine/deployment supports the required failover behavior. An available replica, a reader endpoint and a writable recovery target are not synonyms.

Replication is a configuration and data-flow problem. Check eligible objects, existing-object treatment, ownership, encryption and destination permissions before declaring that every required byte is protected. Verify the copies the recovery process will actually use.

## 4. Make resilience measurable

**RPO** describes acceptable data-loss exposure; **RTO** describes acceptable recovery time. Define the start/end events used to measure them. Measure through usable application recovery, not merely resource creation or database availability.

An architecture review should account for:

- The failure being tested: instance, Availability Zone, Region, account, key access or operator mistake.
- The latest recoverable application-consistent point, not just a backup's creation timestamp.
- Identity, DNS, secrets, encryption keys, application artifacts and external dependencies.
- The evidence that restoration works, including permissions and data validation.
- Failback, duplicate processing and the handling of writes made during recovery.

Vault immutability and restore testing solve different problems. A protected recovery point can still require a tested restoration procedure. A successful drill also does not prove an untested failure class or a shorter target than was measured.

## 5. Plan migration as a sequence, not a copy command

Inventory dependencies, choose the migration pattern, establish a baseline, perform the initial move, synchronize changes where supported, validate, cut over and preserve the agreed rollback boundary. Define who can approve each step.

For database migration, distinguish schema compatibility from data replication. Document supported source/target features and any schema conversion, change-data-capture prerequisites and validation exclusions. A replication task being **running** is not proof of zero lag, complete data validation or application compatibility.

Before cutover, test the application's transactions, credentials and new endpoint behavior. Avoid accepting writes on both sides unless the design explicitly handles that mode. State when rollback ceases to be a simple endpoint reversal because data has diverged.

## 6. Optimize costs from a measured baseline

Review resource usage, demand shape, data movement, requests, storage growth, retrieval and operator effort. Separate a unit-price reduction from an end-to-end cost reduction. A cheaper instance that doubles runtime or a storage tier that makes frequent retrieval expensive may not be cheaper overall.

Commitments should follow an eligible, reasonably stable baseline rather than cover every temporary peak. Compare coverage and utilization separately. Tagging and cost allocation improve visibility; they do not by themselves cap spending. An anomaly notification is a signal to investigate, not proof that an unwanted resource was stopped.

For network cost, trace the actual request and response path, including cross-AZ/Region movement, centralized processing and endpoints. Avoid quoting a rate from memory: verify current regional pricing for the architecture being considered.

## 7. Treat AI architectures as multiple security decisions

Separate retrieval permission, tenant filtering, model invocation, content safeguards, tool authorization and output evaluation. They address different risks.

A guardrail is not an identity system. A retrieved document can be relevant but not authorized for the current user. A model-generated tool argument can be well-formed but request an unauthorized action. Apply trusted server-side identity and authorization before retrieval and side effects, validate tool inputs, and manage sensitive logging independently.

Use an evaluation set representative of the workload, including abstention, unsupported answers, retrieval misses and unsafe-action attempts. A fluent answer or a low latency metric alone does not establish correctness, tenant isolation or safe tool execution. Verify model/Region support rather than assuming every integration has identical features.

## 8. Distinguish change intent from completed change

For infrastructure as code, inspect the proposed change and replacement/deletion behavior. A change set is a preview, not proof that the update executed successfully. Drift detection reports supported observations, not a guarantee of complete compliance.

For multi-account or regional automation, capture a stable operation identity, per-target outcomes and final readback. An accepted request can remain pending or partially fail. Reconcile an ambiguous outcome before retrying a write that could duplicate effects.

Observability should identify the service/version, account/Region, time window and business outcome. Missing data is not a measured zero. A rollback needs a known usable artifact and recovery policy; a previous artifact's existence does not prove compatibility with today's data state.

**A defensible architecture explains both normal flow and the exact evidence needed to recognize, contain and recover from its important failures.**
