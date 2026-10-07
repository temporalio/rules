# TMPRL1300 - External storage experimental API

## Description

External storage offloads large payloads to a storage system you control, such as Amazon S3, and leaves a small
reference to them in the Event History. The SDK stores and retrieves the payloads transparently, so workflows and
activities can work with data larger than the
[payload size limit](https://docs.temporal.io/troubleshooting/blob-size-limit-error) (see also
[TMPRL1103](TMPRL1103.md)).

This feature is experimental. Its APIs may change or be removed in future releases without following the SDK's usual
compatibility guarantees.

## Opting in

### .NET SDK

The external storage APIs are marked with `[Experimental("TMPRL1300")]`, so using them is a compile error until you opt
in. Opt in for a whole project in its `.csproj`:

```xml
<PropertyGroup>
  <NoWarn>$(NoWarn);TMPRL1300</NoWarn>
</PropertyGroup>
```

Or opt in for a single file or region:

```csharp
#pragma warning disable TMPRL1300
```

The diagnostic is reported by the C# compiler shipped with the .NET 8 SDK (Visual Studio 17.8) and later, regardless of
the target framework. Older compilers do not report it, but the APIs are still experimental.

### Other SDKs

No opt-in is required. The external storage APIs are marked as experimental in each SDK's API documentation.

## Statuses

SDK        | Prerelease  | Public Preview | GA
-----------|-------------|----------------|----
.NET       | Unreleased  |                | 
Go         | 1.42.0      | 1.43.0         | 
Java       | 1.39.0      |                | 
PHP        | Unreleased  |                |  
Python     | 1.25.0      | 1.27.0         | 
Ruby       | Unreleased  |                |  
Rust       | Unreleased  |                |  
TypeScript | 1.21.0      |                | 
