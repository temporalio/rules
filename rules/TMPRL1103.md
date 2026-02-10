# TMPRL1103 - Payload size limit exceeded

## Cause

The size of a payload or collection of payloads has exceed configured size limits, either causing a warning to be emitted or a workflow/activity/Nexus task to fail.

## Description

By default, the SDK will check the size of payloads as they move throughout workflows, activities, and Nexus operations. The SDK will log warnings if the size of the payloads are over the warning limit and throw exceptions or return errors if they are over the error limit.

The error limit is only enforced within workers when attempting to submit messages to the Temporal server and they exceed the error limits as described by the server. When the error limit is exceeded, the worker task is proactively failed instead of sending the large payload to the server. This helps the server to prevent excessive resource usage and avoid potential performance issues when handling large payloads. Additionally, failing the task allows the task to be retried instead of failing the associated workflow.

## Remediation

To resolve this error, reduce the size of the payload so that it is below the error limit.

Refer to https://docs.temporal.io/troubleshooting/blob-size-limit-error for further information about payload error limits and strategies to reduce payload sizes.

Customers who use gRPC proxies that alter payloads before they are passed to the server (e.g. encrypting payloads, offloading to external storage, etc) should disable the error limit check in their workers. Please refer to the worker option documentation for the SDK on how to disable this behavior.