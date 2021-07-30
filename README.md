﻿# Stowage [![Nuget](https://img.shields.io/nuget/v/Stowage?style=for-the-badge)](https://www.nuget.org/packages/Stowage)


![](media/icon/icon-64.png)

**<span style="color:blue">Stowage</span>** is a **bloat-free .NET cloud storage kit** that supports at minimum THE major ☁ providers.

- **Independent** 🆓. Provides an independent implementation of the ☁ storage APIs. Because you can't just have official corporate SDKs as a single source of truth.
- **Readable**. Official SDKs like the ones for [AWS](https://github.com/aws/aws-sdk-net), [Google](https://github.com/googleapis/google-cloud-dotnet), or [Azure](https://github.com/Azure/azure-storage-net) are overengineered and unreadable. Some are autogenerated and look just bad and foreign to .NET ecosystem. Some won't even compile without some custom rituals.
- **Beautiful** 🦋. Designed to fit into .NET ecosystem, not the other way around.
- **Rich** 💰. Provide maximum functionality. However, in addition to that, provide humanly possible way to easily extend it with new functionality, without waiting for new SDK releases.
- **Embeddable** 🔱. Has zero external dependencies, relies only on built-in .NET API. Often official SDKs have a very deep dependency tree causing a large binary sizes and endless conflicts during runtime. This one is a single .NET .dll with no dependencies whatsoever.
- **Cross Cloud** 🌥. Same API. Any cloud. Best decisions made for you. It's like iPhone vs Windows Phone.
- **Cross Tested** ❎. It's not just cross cloud but also cross tested (I don't know how to call this). It tests that all cloud providers behave absolutely the same on various method calls. They should validate arguments the same, throw same exceptions in the same situations, and support the same set of functionality. Sounds simple, but it's rare to find in a library. And it important, otherwise what's the point of a generic API if you need to write a lot of `if()`s? (or pattern matching).

# Getting Started

Right, time to gear up. We'll do it [step by step](https://www.oxfordlearnersdictionaries.com/definition/english/step_1?q=step). First, you need to [install](https://docs.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-using-the-dotnet-cli) the [![Nuget](https://img.shields.io/nuget/v/Stowage?style=social)](https://www.nuget.org/packages/Stowage) package.


Simplest case, using the local 💽 and writing text "I'm a page!!!" to a file called "pagefile.sys" at the root of disk C::

```csharp
using Stowage;

using(IFileStorage fs = Files.Of.LocalDisk("c:\\"))
{
   await fs.WriteText("pagefile.sys", "I'm a page!!!!");
}
```

This is local disk, yeah? But what about cloud storage, like Azure Blob Storage? Piece of cake:

```csharp
using Stowage;

using(IFileStorage fs = Files.Of.AzureBlobStorage("accountName", "accountKey", "containerName"))
{
   var entries = await fs.Ls();
}
```




# ♒ <span style="color:red">S</span>treaming

Streaming is a *first-class feature*. This means the streaming is real with no workarounds or in-memory buffers, so you can upload/download files of virtually unlimited sizes. Most official SDKs *do not support streaming* at all - surprisingly even the cloud leader's .NET SDK doesn't. Each requires some sort of crippled down version of stream - either knowing length beforehand, or will buffer it all in memory. We don't. We stream like a stream.

Proper streaming support also means that you can transform streams as you write to them or read from them - something that is not available in the native SDKs. For instance gzipping, encryption, anything else.

Streaming is also truly compatible with synchronous and asynchronous API.

# Extending

There are many ways to extend functionality:

1. **Documentation**. You might think it's not extending anything, however if user is not aware for some functionality it doesn't exist. Documenting it is making it available, hence extending.
2. **New functionality**. Adding utility methods like copying files inside or between accounts, automatic json serialisation etc. is always good. Look `IFileStorage` interface and `PolyfilledFileStorage`. In most cases these two files are enought to add pure business logic. Not counting unit tests.
3. **Native optimisations**. Some functionality is generic, and some depends on a specific cloud provider. For instance, one can copy a file by downloading it locally, and uploading with a new name. Or utilise a native REST call that accepts source and target file name, if it exists.

# Traits, Mixins, IoC

Core interface with basic functionality as before. Fat interface for something else. Tiny interface per functionality.