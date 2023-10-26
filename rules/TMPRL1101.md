# TMPRL1101 - Deadlock detected during workflow run

## Cause

Workflow code took too long to yield to Temporal.

## Description

When an SDK detects that workflow code is running too long (often either 1 or 2 seconds) before waiting on temporal, a
deadlock detection error can occur. This can happen for multiple reasons:

* Code is stuck in a busy loop (e.g. an infinite for loop)
* Code is doing heavy CPU-bound calls in a workflow (e.g. running heavy cryptography)
* Code is accidentally blocking the coroutine with a synchronous/non-deterministic call (e.g. blocking sleep)
* Worker is overloaded which means CPU is blocked.
* Data conversion is taking too long (e.g. a codec is calling an external encryption service)

For healthy execution of the system, workflow code should only run for a very short time before deferring back to
Temporal. In most cases this is only a few milliseconds.

## Remediation

First, it is important to find the cause of the deadlock. Sometimes this can be visible by issuing a stack trace query
to the workflow via the UI or CLI. Sometimes the deadlock can be replicated by downloading the history of the offending
workflow and running it through the replayer. See the
[documentation for replayers](https://docs.temporal.io/workflows#replays). Other times it is a bit harder since it may
only be happening in one environment (e.g. due to overloaded worker).

Fixing options:

### Remove source of deadlock

If this is a busy loop or other bad code that is stopping the workflow from progressing, fix that code to remove that
source of blocking. This can be done in an activity instead. Workflow code cannot block in a way that doesn't yield to
Temporal.

### Reduce worker load

If the deadlock detection is occurring under periods of high load, then it may just be that code is running really slow
due to resource utilization. Make sure there are enough workers or worker resources to handle the workflow load.

### Disable deadlock detection for data converters

In Go and Java SDKs the time spent in data converters contribute to the overall deadlock detection timeout. In other
SDKs, only the payload conversion part contributes, the codec part which might hit a remote server does not. To disable
deadlock detection for a data converter in Go, wrap it with `workflow.DataConverterWithoutDeadlockDetection`. To disable
deadlock detection in Java, put the potentially long-running data converter code inside
`WorkflowUnsafe.deadlockDetectorOff`.