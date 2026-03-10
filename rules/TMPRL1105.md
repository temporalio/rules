# TMPRL1105 - External storage reference found but driver not configured

## Cause

The SDK encountered a payload that was previously offloaded to external storage, but the worker or replayer processing it does not have external storage configured drivers configured.

## Description

SDKs will emit a log statement when it encouters a payload that has been externally stored but external storage is not configured. This payload was likely emitted by another worker that had external storage configured. The current worker sees that it is an externally stored payload but has no way to retrieve the content, so it passes the claim payload through the system as-is. Any workflow or activity code that tries to deserialize the payload will likely receive an unexpected value or a deserialization error.

## Remediation
