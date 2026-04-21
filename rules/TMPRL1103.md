# TMPRL1103 - Payload size limit exceeded

## Cause

A payload or collection of payloads exceeded the configured size limit. The SDK logs a warning when payloads exceed the
warning threshold and fails the task when payloads exceed the error limit.

## Description

Depending on the SDK version, this limit is enforced either by the Temporal Service or by the SDK itself. When the SDK
enforces the limit, the task is failed before the payload is sent to the server, allowing the task to be retried after a
fix is deployed. When the Temporal Service enforces the limit, the behavior varies: the Workflow may be terminated, or
the task may get stuck in a retry loop.

## Remediation

Offload large payloads to external storage using the
[claim check pattern](https://docs.temporal.io/troubleshooting/blob-size-limit-error#payload-size-limit). This replaces
the payload with a small reference token in the Event History. The SDK retrieves the full payload from external storage
transparently.

See [Troubleshoot payload and gRPC message size limit errors](https://docs.temporal.io/troubleshooting/blob-size-limit-error)
for a full list of error messages, behavior by SDK version, and resolution strategies.
