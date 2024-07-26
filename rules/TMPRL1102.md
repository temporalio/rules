# TMPRL1102 - Workflow finished while handlers are still running.

## Cause

A signal or update handler had not finished executing when the workflow finished (completed, or continued-as-new).

## Description

An executing workflow comprises one or more concurrent tasks executing in a cooperative and logically single-threaded
fashion. The precise nature of these tasks depends on the language: e.g. in Python they are asyncio coroutines, in
Typescript they are the standard event loop tasks/microtasks, in Go they are goroutines under the control of a custom
cooperative multitasking framework, in Java they are threads under the control of a custom cooperative multitasking
framework, etc. One of these concurrent tasks corresponds to the workflow main function/method, and there may be others
corresponding to spawned child tasks and handler executions. "Cooperative" means that switching between tasks only
occurs at certain points: in Python, Typescript, and .NET these points are syntactically obvious (they have an `await`);
in Java and Go they occur wherever you call an SDK API to perform an async operation.

**Note: this warning, and the following discussion, do not apply to signals in Go.**

When a workflow handles an update or signal, and the handler performs an async operation (e.g. it waits on a condition
or timer, or executes an activity or child workflow), then the workflow executes the handler in a new concurrent task
that can interleave with the execution of all other concurrent tasks.

This warning is informing you that the workflow main task finished while a signal or update handler task was
still executing. There are two reasons that you might want to avoid this:

1. If the handler had been allowed to complete its execution, it might have performed additional operations with side
   effects (e.g. caused an activity to be executed). Are you sure you want the workflow to execute without these side
   effects occuring? Is it appropriate for the handler to have some but not all of its side effects?

2. In the case of an update handler, the caller may be waiting for the update result. If you allow the workflow to
   finish without allowing the update handler to finish, then the caller will get an error instead of the update result.
   Are you sure you want callers to see errors in this situation?


## Remediation

You have two options:

### 1. Wait for handlers to finish before exiting the workflow

Before allowing the workflow to finish (via return, or continue-as-new) you can wait on the "all handlers finished"
condition. For example, in Typescript, this looks like
```typescript
await workflow.condition(() => workflow.allHandlersFinished())
```

Alternatively, you can wait on a condition of your own devising that implies that your signal/update handler executions are not in-progress.


### 2. Silence the warning

You can silence the warning on a per-handler basis by setting the `HandlerUnfinishedPolicy` to `ABANDON`. For example,
in Typescript, this looks like
```typescript
workflow.setHandler(myUpdate, myUpdateHandler, {unfinishedPolicy: HandlerUnfinishedPolicy.ABANDON})
```
The default value of this policy is `WARN_AND_ABANDON`.