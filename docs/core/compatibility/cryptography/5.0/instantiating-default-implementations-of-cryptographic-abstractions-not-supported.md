---
title: 中断性变更：不支持对加密抽象的默认实现进行实例化
description: 了解 .NET 5 中的中断性变更：对加密抽象的无参数 Create() 重载已过时。
ms.date: 10/16/2020
ms.openlocfilehash: b75f3568317d1db8ae1bb629f760aaec7e69776a
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256785"
---
# <a name="instantiating-default-implementations-of-cryptographic-abstractions-is-not-supported"></a><span data-ttu-id="ba962-103">不支持对加密抽象的默认实现进行实例化</span><span class="sxs-lookup"><span data-stu-id="ba962-103">Instantiating default implementations of cryptographic abstractions is not supported</span></span>

<span data-ttu-id="ba962-104">从 .NET 5.0 开始，对加密抽象的无参数 `Create()` 重载已过时，不再作为警告显示。</span><span class="sxs-lookup"><span data-stu-id="ba962-104">The parameterless `Create()` overloads on cryptographic abstractions are obsolete as warning as of .NET 5.0.</span></span>

## <a name="change-description"></a><span data-ttu-id="ba962-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="ba962-105">Change description</span></span>

<span data-ttu-id="ba962-106">在 .NET Framework 2.0 - 4.8 中，可以将抽象加密基元工厂（如 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType>）配置为返回不同的算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-106">In .NET Framework 2.0 - 4.8, abstract cryptographic primitive factories such as <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> can be configured to return different algorithms.</span></span> <span data-ttu-id="ba962-107">例如，在默认安装 .NET Framework 4.8 时，无参数的静态方法 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> 返回 SHA1 算法的实例，如以下代码片段所示。</span><span class="sxs-lookup"><span data-stu-id="ba962-107">For example, on a default install of .NET Framework 4.8, the parameterless, static method <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> returns an instance of the SHA1 algorithm, as shown in the following snippet.</span></span>

<span data-ttu-id="ba962-108">**仅限 .NET Framework**</span><span class="sxs-lookup"><span data-stu-id="ba962-108">**.NET Framework only**</span></span>

```csharp
// Return an instance of the default hash algorithm (SHA1).
HashAlgorithm alg = HashAlgorithm.Create();
// Prints 'System.Security.Cryptography.SHA1CryptoServiceProvider'.
Console.WriteLine(alg.GetType());

// Change the default algorithm to be SHA256, not SHA1.
CryptoConfig.AddAlgorithm(typeof(SHA256CryptoServiceProvider), typeof(HashAlgorithm).FullName);
alg = HashAlgorithm.Create();
// Prints 'System.Security.Cryptography.SHA256CryptoServiceProvider'.
Console.WriteLine(alg.GetType());
```

<span data-ttu-id="ba962-109">此外，你还可以使用[计算机范围的配置](../../../../framework/configure-apps/map-algorithm-names-to-cryptography-classes.md)来更改默认算法，而无需以编程方式调用 `CryptoConfig`。</span><span class="sxs-lookup"><span data-stu-id="ba962-109">You can also use [machine-wide configuration](../../../../framework/configure-apps/map-algorithm-names-to-cryptography-classes.md) to change the default algorithm without needing to call into `CryptoConfig` programmatically.</span></span>

<span data-ttu-id="ba962-110">在 .NET Core 2.0 - 3.1 中，抽象加密基元工厂（例如 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType>）始终引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="ba962-110">In .NET Core 2.0 - 3.1, abstract cryptographic primitive factories such as <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> always throw a <xref:System.PlatformNotSupportedException>.</span></span>

```csharp
// Throws PlatformNotSupportedException on .NET Core.
HashAlgorithm alg = HashAlgorithm.Create();
```

<span data-ttu-id="ba962-111">在 .NET 5 及更高版本中，抽象加密基元工厂（例如 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType>）被标记为已过时，并生成 ID 为 `SYSLIB0007` 的编译时警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-111">In .NET 5 and later versions, abstract cryptographic primitive factories such as <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> are marked obsolete and produce a compile-time warning with ID `SYSLIB0007`.</span></span> <span data-ttu-id="ba962-112">在运行时，这些方法会继续引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="ba962-112">At run time, these methods continue to throw a <xref:System.PlatformNotSupportedException>.</span></span>

```csharp
// Throws PlatformNotSupportedException.
// Also produces compile-time warning SYSLIB0007 on .NET 5+.
HashAlgorithm alg = HashAlgorithm.Create();
```

<span data-ttu-id="ba962-113">这是仅在编译时进行的更改。</span><span class="sxs-lookup"><span data-stu-id="ba962-113">This is a compile-time only change.</span></span> <span data-ttu-id="ba962-114">以前版本的 .NET Core 没有运行时更改。</span><span class="sxs-lookup"><span data-stu-id="ba962-114">There is no run-time change from previous versions of .NET Core.</span></span>

> [!NOTE]
>
> - <span data-ttu-id="ba962-115">仅 `Create()` 方法的无参数重载已过时。</span><span class="sxs-lookup"><span data-stu-id="ba962-115">Only the parameterless overloads of the `Create()` methods are obsolete.</span></span> <span data-ttu-id="ba962-116">参数化重载未过时，并且仍可按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="ba962-116">Parameterized overloads are not obsolete and still function as expected.</span></span>
>
>   ```csharp
>   // Call Create(string), providing an explicit algorithm family name.
>   // Works in .NET Framework, .NET Core, and .NET 5.0+.
>   HashAlgorithm hashAlg = HashAlgorithm.Create("SHA256");
>   ```
>
> - <span data-ttu-id="ba962-117">特定算法系列（而非抽象）的无参数重载未过时，并将继续按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="ba962-117">Parameterless overloads of *specific* algorithm families (not abstractions) are not obsolete, and will continue to function as expected.</span></span>
>
>   ```csharp
>   // Call a specific algorithm family's parameterless Create() ctor.
>   // Works in .NET Framework, .NET Core, and .NET 5.0+.
>   Aes aesAlg = Aes.Create();
>   ```

## <a name="reason-for-change"></a><span data-ttu-id="ba962-118">更改原因</span><span class="sxs-lookup"><span data-stu-id="ba962-118">Reason for change</span></span>

<span data-ttu-id="ba962-119">.NET Framework 中的加密配置系统不再存在于 .NET Core 和 .NET 5.0+ 中，因为该旧系统不允许适当的加密灵活性。</span><span class="sxs-lookup"><span data-stu-id="ba962-119">The cryptographic configuration system present in .NET Framework is no longer present in .NET Core and .NET 5.0+, since that legacy system doesn't allow for proper cryptographic agility.</span></span> <span data-ttu-id="ba962-120">.NET 的后向兼容性要求也禁止框架更新某些加密 API 以跟上加密技术的发展。</span><span class="sxs-lookup"><span data-stu-id="ba962-120">.NET's backward-compatibility requirements also prohibit the framework from updating certain cryptographic APIs to keep up with advances in cryptography.</span></span> <span data-ttu-id="ba962-121">例如，当 SHA-1 哈希算法是顶尖的算法时，.NET Framework 1.0 中引入了 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="ba962-121">For example, the <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> method was introduced in .NET Framework 1.0, when the SHA-1 hash algorithm was state-of-the-art.</span></span> <span data-ttu-id="ba962-122">20 年过去了，现在人们认为 SHA-1 已无效，但我们无法更改 <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> 来返回不同的算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-122">Twenty years have passed, and now SHA-1 is considered broken, but we cannot change <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> to return a different algorithm.</span></span> <span data-ttu-id="ba962-123">这样做会在使用的应用程序中引入不可接受的中断性变更。</span><span class="sxs-lookup"><span data-stu-id="ba962-123">Doing so would introduce an unacceptable breaking change in consuming applications.</span></span>

<span data-ttu-id="ba962-124">最佳做法表明，使用加密基元（例如 AES、SHA-\* 和 RSA）的库应完全控制它们使用这些基元的方式。</span><span class="sxs-lookup"><span data-stu-id="ba962-124">Best practice dictates that libraries that consume cryptographic primitives (such as AES, SHA-\*, and RSA) should be in full control over how they consume these primitives.</span></span> <span data-ttu-id="ba962-125">需要面向未来的应用程序应利用较高级别的库，这些库包装这些基元并添加密钥管理和加密灵活性功能。</span><span class="sxs-lookup"><span data-stu-id="ba962-125">Applications that require future-proofing should utilize higher-level libraries that wrap these primitives and add key management and cryptographic agility capabilities.</span></span> <span data-ttu-id="ba962-126">这些库通常由托管环境提供。</span><span class="sxs-lookup"><span data-stu-id="ba962-126">These libraries are often provided by the hosting environment.</span></span> <span data-ttu-id="ba962-127">例如，[ASP.NET 的数据保护库](/aspnet/core/security/data-protection/)，该库代表调用应用程序来处理这些问题。</span><span class="sxs-lookup"><span data-stu-id="ba962-127">One example is [ASP.NET's Data Protection library](/aspnet/core/security/data-protection/), which handles these concerns on behalf of the calling application.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ba962-128">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ba962-128">Version introduced</span></span>

<span data-ttu-id="ba962-129">5.0</span><span class="sxs-lookup"><span data-stu-id="ba962-129">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ba962-130">建议操作</span><span class="sxs-lookup"><span data-stu-id="ba962-130">Recommended action</span></span>

- <span data-ttu-id="ba962-131">建议采取的操作是用对特定算法（例如 <xref:System.Security.Cryptography.Aes.Create?displayProperty=nameWithType>）的工厂方法的调用替换对现已过时的 API 的调用。</span><span class="sxs-lookup"><span data-stu-id="ba962-131">The recommended course of action is to replace calls to the now-obsolete APIs with calls to factory methods for specific algorithms, for example, <xref:System.Security.Cryptography.Aes.Create?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ba962-132">这样，便可以完全控制要实例化哪些算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-132">This gives you full control over which algorithms are instantiated.</span></span>

- <span data-ttu-id="ba962-133">如果需要保持与使用现已过时的 API 的 .NET Framework 应用生成的现有有效负载的兼容性，请使用下表中建议的替换项。</span><span class="sxs-lookup"><span data-stu-id="ba962-133">If you need to maintain compatibility with existing payloads generated by .NET Framework apps that use the now-obsolete APIs, use the replacements suggested in the following table.</span></span> <span data-ttu-id="ba962-134">该表提供了从 .NET Framework 默认算法到其 .NET 5+ 等效项的映射。</span><span class="sxs-lookup"><span data-stu-id="ba962-134">The table provides a mapping from .NET Framework default algorithms to their .NET 5+ equivalents.</span></span>

  | <span data-ttu-id="ba962-135">.NET framework</span><span class="sxs-lookup"><span data-stu-id="ba962-135">.NET Framework</span></span> | <span data-ttu-id="ba962-136">.NET Core/.NET 5.0+ 兼容替换项</span><span class="sxs-lookup"><span data-stu-id="ba962-136">.NET Core / .NET 5.0+ compatible replacement</span></span> | <span data-ttu-id="ba962-137">备注</span><span class="sxs-lookup"><span data-stu-id="ba962-137">Remarks</span></span> |
  | - | - | - |
  | <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.RSA.Create?displayProperty=nameWithType> | |
  | <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.SHA1.Create?displayProperty=nameWithType> | <span data-ttu-id="ba962-138">SHA-1 算法被认为已无效。</span><span class="sxs-lookup"><span data-stu-id="ba962-138">The SHA-1 algorithm is considered broken.</span></span> <span data-ttu-id="ba962-139">如果可能，请考虑使用更强大的算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-139">Consider using a stronger algorithm if possible.</span></span> <span data-ttu-id="ba962-140">请咨询安全顾问以获取进一步的指导。</span><span class="sxs-lookup"><span data-stu-id="ba962-140">Consult your security advisor for further guidance.</span></span> |
  | <xref:System.Security.Cryptography.HMAC.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.HMACSHA1.%23ctor> | <span data-ttu-id="ba962-141">对于大多数新式应用程序，不建议使用 HMACSHA1 算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-141">The HMACSHA1 algorithm is discouraged for most modern applications.</span></span> <span data-ttu-id="ba962-142">如果可能，请考虑使用更强大的算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-142">Consider using a stronger algorithm if possible.</span></span> <span data-ttu-id="ba962-143">请咨询安全顾问以获取进一步的指导。</span><span class="sxs-lookup"><span data-stu-id="ba962-143">Consult your security advisor for further guidance.</span></span> |
  | <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.HMACSHA1.%23ctor> | <span data-ttu-id="ba962-144">对于大多数新式应用程序，不建议使用 HMACSHA1 算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-144">The HMACSHA1 algorithm is discouraged for most modern applications.</span></span> <span data-ttu-id="ba962-145">如果可能，请考虑使用更强大的算法。</span><span class="sxs-lookup"><span data-stu-id="ba962-145">Consider using a stronger algorithm if possible.</span></span> <span data-ttu-id="ba962-146">请咨询安全顾问以获取进一步的指导。</span><span class="sxs-lookup"><span data-stu-id="ba962-146">Consult your security advisor for further guidance.</span></span> |
  | <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=nameWithType> | <xref:System.Security.Cryptography.Aes.Create?displayProperty=nameWithType> |

- <span data-ttu-id="ba962-147">如果必须继续调用已过时的无参数 `Create()` 重载，则可以在代码中取消 `SYSLIB0007` 警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-147">If you must continue to call the obsolete parameterless `Create()` overloads, you can suppress the `SYSLIB0007` warning in code.</span></span>

  ```csharp
  #pragma warning disable SYSLIB0007 // Disable the warning.
  HashAlgorithm alg = HashAlgorithm.Create(); // Still throws PNSE.
  #pragma warning restore SYSLIB0007 // Re-enable the warning.
  ```

  <span data-ttu-id="ba962-148">另外，还可以在项目文件中取消该警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-148">You can also suppress the warning in your project file.</span></span> <span data-ttu-id="ba962-149">这样做会对项目中所有源文件禁用该警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-149">Doing so disables the warning for all source files within the project.</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- NoWarn below suppresses SYSLIB0007 project-wide -->
     <NoWarn>$(NoWarn);SYSLIB0007</NoWarn>
    </PropertyGroup>
  </Project>
  ```

  > [!NOTE]
  > <span data-ttu-id="ba962-150">取消 `SYSLIB0007` 仅禁用此处列出的加密 API 的过时警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-150">Suppressing `SYSLIB0007` disables only the obsoletion warnings for the cryptography APIs listed here.</span></span> <span data-ttu-id="ba962-151">它不会禁用任何其他警告。</span><span class="sxs-lookup"><span data-stu-id="ba962-151">It does not disable any other warnings.</span></span> <span data-ttu-id="ba962-152">此外，即使取消该警告，这些已过时的 API 仍会在运行时引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="ba962-152">Additionally, even if you suppress the warning, these obsoleted APIs will still throw a <xref:System.PlatformNotSupportedException> at run time.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ba962-153">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ba962-153">Affected APIs</span></span>

- <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.HMAC.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=fullName>

<!--

### Affected APIs

- `M:System.Security.Cryptography.AsymmetricAlgorithm.Create`
- `M:System.Security.Cryptography.HashAlgorithm.Create`
- `M:System.Security.Cryptography.HMAC.Create`
- `M:System.Security.Cryptography.KeyedHashAlgorithm.Create`
- `M:System.Security.Cryptography.SymmetricAlgorithm.Create`

### Category

- Cryptography

-->
