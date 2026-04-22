# TMPRL1103 - Payload size limit exceeded

## Cause

The size of a payload or collection of payloads has exceed configured size limits, either causing a warning to be emitted or a Workflow/Activity/Nexus task to fail.

## Description

Depending on the SDK version, the payload size limit is enforced either by the Temporal Service or by the SDK itself. When the SDK
enforces the limit, the SDK fails the Task before the payload is sent to the server, allowing the Task to be retried after you deploy a fix. When the Temporal Service enforces the limit, the behavior varies: the Workflow may be terminated, or
the Task may get stuck in a retry loop.

## Remediation

Offload large payloads to external storage using the
[claim check pattern](https://docs.temporal.io/troubleshooting/blob-size-limit-error#payload-size-limit). This replaces
the payload with a small reference token in the Event History. The SDK retrieves the full payload from external storage
transparently.

See [Message size limit errors](https://docs.temporal.io/troubleshooting/blob-size-limit-error)
for a full list of error messages, behavior by SDK version, and resolution strategies.
