# Temporal Rules

This repository houses numbered and documented Temporal rules. These rules capture common errors, warnings, and
guidelines that occur across Temporal. The rules are meant for referencing and linking.

## Rules

All rules are in tables below by high-level category.

### SDK Rules (TMPRL1100-1299)

Rules that apply to Temporal SDKs.

Identifier | Description
-----------|-------------
[TMPRL1100](rules/TMPRL1100.md) | Non-determinism detected during workflow replay
[TMPRL1101](rules/TMPRL1101.md) | Deadlock detected during workflow run
<!-- TODO:
[TMPRL1102](rules/TMPRL1102.md) | Payload conversion failure
[TMPRL1103](rules/TMPRL1103.md) | Using known non-deterministic construct (TODO: often for static analyzers)
[TMPRL1104](rules/TMPRL1104.md) | Invalid workflow definition
[TMPRL1105](rules/TMPRL1105.md) | Invalid activity definition
[TMPRL1106](rules/TMPRL1106.md) | Invalid signal, query, or update definition on workflow
[TMPRL1107](rules/TMPRL1107.md) | Invalid timer duration
[TMPRL1108](rules/TMPRL1108.md) | Invalid activity options
[TMPRL1109](rules/TMPRL1109.md) | Should provide business ID when starting workflow. (TODO: best practice, not error)
[TMPRL1110](rules/TMPRL1110.md) | Unknown workflow on worker
[TMPRL1111](rules/TMPRL1111.md) | Unknown activity on worker
[TMPRL1112](rules/TMPRL1112.md) | Unknown signal, query, or update on workflow
[TMPRL1113](rules/TMPRL1113.md) | Server connection failed
[TMPRL1114](rules/TMPRL1114.md) | Invalid search attribute
[TMPRL1115](rules/TMPRL1115.md) | Prefer start-to-close over schedule-to-close
[TMPRL1116](rules/TMPRL1116.md) | Worker poll failure
[TMPRL1117](rules/TMPRL1117.md) | Workflow task failure
[TMPRL1118](rules/TMPRL1118.md) | Read-only context issued command
[TMPRL1119](rules/TMPRL1119.md) | Not in workflow context
[TMPRL1120](rules/TMPRL1120.md) | Not in activity context
-->

### Server Rules (TMPRL2100-2299)

Rules that apply to the Temporal server.

Identifier | Description
-----------|-------------
<!-- TODO:
[TMPRL2100](rules/TMPRL2100.md) | Payload too large
[TMPRL2101](rules/TMPRL2101.md) | gRPC request too large
[TMPRL2102](rules/TMPRL2102.md) | History size/length too large (TODO: For both warning and error use)
[TMPRL2103](rules/TMPRL2103.md) | Pending event limit reached
[TMPRL2104](rules/TMPRL2104.md) | Workflow task timeout
[TMPRL2105](rules/TMPRL2105.md) | Activity heartbeat timeout
-->

### Cloud Rules (TMPRL3100-3299)

Rules that apply to Temporal cloud.

Identifier | Description
-----------|-------------
<!-- TODO -->