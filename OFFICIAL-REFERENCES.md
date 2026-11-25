# Official references: SAP-C03 architecture decisions

Original-publisher links selected from preserved course context. No manuals are bundled and live availability was not checked for this offline companion. Follow the current version and verify Region, service mode and prerequisites before designing or changing a system.

## Exam identity and scope

- [AWS SAP-C03 exam guide](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03.html) — use the current domain/task list to identify study gaps.
- [AWS SAP-C02/SAP-C03 comparison](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/sap-03-comparison.html) — the preserved official comparison identifies C02 through November 16, 2026 and C03 beginning November 17, 2026.

## Paths, permissions and data

- [IAM policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) — evaluate the actual principal/action/resource and applicable policy layers.
- [KMS key policies](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) and [multi-Region keys](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html) — distinguish key relationships from authorization and regional controls.
- [Transit Gateway route tables](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-route-tables.html) and [centralized appliance scenario](https://docs.aws.amazon.com/vpc/latest/tgw/transit-gateway-appliance-scenario.html) — review attachment routing and stateful inspection paths.
- [Aurora endpoints](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.Endpoints.html) — distinguish writer, reader and instance endpoint behavior.
- [S3 replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html) and [KMS-encrypted object replication](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-config-for-kms-objects.html) — verify copy coverage and destination-key requirements.

## Recovery, migration and economics

- [AWS Backup Vault Lock](https://docs.aws.amazon.com/aws-backup/latest/devguide/vault-lock.html) — understand retention protections before making consequential configuration changes.
- [AWS Backup restore testing](https://docs.aws.amazon.com/aws-backup/latest/devguide/restore-testing.html) — distinguish protected copies from demonstrated restoration.
- [AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) — inspect supported source/target and migration behavior; a running task is not complete application validation.
- [Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html) — verify eligibility and commitment behavior instead of assuming universal coverage.
- [Cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) — review allocation visibility without confusing it with a spending cap.

## Automation and AI

- [CloudFormation change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) — separate a proposed change from execution and final state.
- [Bedrock knowledge bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html), [guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html) and [evaluation](https://docs.aws.amazon.com/bedrock/latest/userguide/evaluation.html) — review retrieval, content controls and quality assessment as distinct concerns.
- [Bedrock action groups](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-action-create.html) — inspect how an agent integrates actions; application authorization remains an explicit architecture responsibility.

A reference supports further verification; it is not a statement that an architecture was deployed, reviewed by AWS or tested against a recovery target.
