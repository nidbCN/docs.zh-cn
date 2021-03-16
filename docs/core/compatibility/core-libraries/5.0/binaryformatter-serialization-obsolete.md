---
title: 中断性变更：BinaryFormatter 序列化方法已过时，并且已在 ASP.NET 应用中禁用
description: 了解核心 .NET 库中的 .NET 5 中断性变更：BinaryFormatter、Formatter 和 IFormatter 上的序列化和反序列化方法已过时。
ms.date: 11/01/2020
ms.openlocfilehash: 8c31a95683526ba394101d449b5ae3328f57f3c3
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257656"
---
# <a name="binaryformatter-serialization-methods-are-obsolete-and-prohibited-in-aspnet-apps"></a><span data-ttu-id="74d93-103">BinaryFormatter 序列化方法已过时，并且已在 ASP.NET 应用中禁用</span><span class="sxs-lookup"><span data-stu-id="74d93-103">BinaryFormatter serialization methods are obsolete and prohibited in ASP.NET apps</span></span>

<span data-ttu-id="74d93-104"><xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter><xref:System.Runtime.Serialization.Formatter> 和 <xref:System.Runtime.Serialization.IFormatter> 上的 `Serialize` 和 `Deserialize` 方法现已过时，不再作为警告显示。</span><span class="sxs-lookup"><span data-stu-id="74d93-104">`Serialize` and `Deserialize` methods on <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>, <xref:System.Runtime.Serialization.Formatter>, and <xref:System.Runtime.Serialization.IFormatter> are now obsolete as warning.</span></span> <span data-ttu-id="74d93-105">此外，ASP.NET 应用默认情况下禁止 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 序列化。</span><span class="sxs-lookup"><span data-stu-id="74d93-105">Additionally, <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> serialization is prohibited by default for ASP.NET apps.</span></span>

## <a name="change-description"></a><span data-ttu-id="74d93-106">更改描述</span><span class="sxs-lookup"><span data-stu-id="74d93-106">Change description</span></span>

<span data-ttu-id="74d93-107">由于 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 存在[安全漏洞](../../../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities)，因此，以下方法现已过时，并生成 ID 为 `SYSLIB0011` 的编译时警告。</span><span class="sxs-lookup"><span data-stu-id="74d93-107">Due to [security vulnerabilities](../../../../standard/serialization/binaryformatter-security-guide.md#binaryformatter-security-vulnerabilities) in <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>, the following methods are now obsolete and produce a compile-time warning with ID `SYSLIB0011`.</span></span> <span data-ttu-id="74d93-108">此外，在 ASP.NET Core 5.0 及更高版本的应用中，除非 Web 应用已重新启用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 功能，否则它们会引发 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="74d93-108">Additionally, in ASP.NET Core 5.0 and later apps, they will throw a <xref:System.NotSupportedException>, unless the web app has re-enabled <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> functionality.</span></span>

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType>

<span data-ttu-id="74d93-109">以下序列化方法现在也已过时并生成警告 `SYSLIB0011`，但没有行为变更：</span><span class="sxs-lookup"><span data-stu-id="74d93-109">The following serialization methods are also obsolete and produce warning `SYSLIB0011`, but have no behavioral changes:</span></span>

- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=nameWithType>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=nameWithType>

## <a name="version-introduced"></a><span data-ttu-id="74d93-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="74d93-110">Version introduced</span></span>

<span data-ttu-id="74d93-111">5.0</span><span class="sxs-lookup"><span data-stu-id="74d93-111">5.0</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="74d93-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="74d93-112">Reason for change</span></span>

<span data-ttu-id="74d93-113">在 .NET 生态系统中逐步停用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 的过程中，这些方法被标记为“过时”。</span><span class="sxs-lookup"><span data-stu-id="74d93-113">These methods are marked obsolete as part of an effort to wind down usage of <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> within the .NET ecosystem.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="74d93-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="74d93-114">Recommended action</span></span>

- <span data-ttu-id="74d93-115">停止在代码中使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>。</span><span class="sxs-lookup"><span data-stu-id="74d93-115">Stop using <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> in your code.</span></span> <span data-ttu-id="74d93-116">此时，考虑使用 <xref:System.Text.Json.JsonSerializer> 或 <xref:System.Xml.Serialization.XmlSerializer>。</span><span class="sxs-lookup"><span data-stu-id="74d93-116">Instead, consider using <xref:System.Text.Json.JsonSerializer> or <xref:System.Xml.Serialization.XmlSerializer>.</span></span> <span data-ttu-id="74d93-117">有关详细信息，请参阅 [BinaryFormatter 安全指南](../../../../standard/serialization/binaryformatter-security-guide.md)。</span><span class="sxs-lookup"><span data-stu-id="74d93-117">For more information, see [BinaryFormatter security guide](../../../../standard/serialization/binaryformatter-security-guide.md).</span></span>

- <span data-ttu-id="74d93-118">可以暂时取消 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 编译时警告 `SYSLIB0011`。</span><span class="sxs-lookup"><span data-stu-id="74d93-118">You can temporarily suppress the <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> compile-time warning, which is `SYSLIB0011`.</span></span> <span data-ttu-id="74d93-119">建议在选择此选项之前，全面评估代码风险。</span><span class="sxs-lookup"><span data-stu-id="74d93-119">We recommend that you thoroughly assess your code for risks before choosing this option.</span></span> <span data-ttu-id="74d93-120">取消警告的最简单方法是用 `#pragma` 指令将单个调用站点括起来。</span><span class="sxs-lookup"><span data-stu-id="74d93-120">The easiest way to suppress the warnings is to surround the individual call site with `#pragma` directives.</span></span>

  ```csharp
  // Now read the purchase order back from disk
  using (var readStream = new FileStream("myfile.bin", FileMode.Open))
  {
      var formatter = new BinaryFormatter();
  #pragma warning disable SYSLIB0011
      return (PurchaseOrder)formatter.Deserialize(readStream);
  #pragma warning restore SYSLIB0011
  }
  ```

  <span data-ttu-id="74d93-121">另外，还可以在项目文件中取消警告。</span><span class="sxs-lookup"><span data-stu-id="74d93-121">You can also suppress the warning in the project file.</span></span>

  ```xml
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
    <!-- Disable "BinaryFormatter is obsolete" warnings for entire project -->
    <NoWarn>$(NoWarn);SYSLIB0011</NoWarn>
  </PropertyGroup>
  ```

  <span data-ttu-id="74d93-122">如果在项目文件中取消警告，会取消项目中所有代码文件的警告。</span><span class="sxs-lookup"><span data-stu-id="74d93-122">If you suppress the warning in the project file, the warning is suppressed for all code files in the project.</span></span> <span data-ttu-id="74d93-123">取消 `SYSLIB0011` 不会取消因使用其他过时 API 导致的警告。</span><span class="sxs-lookup"><span data-stu-id="74d93-123">Suppressing `SYSLIB0011` does not suppress warnings caused by using other obsolete APIs.</span></span>

- <span data-ttu-id="74d93-124">若要继续在 ASP.NET 应用中使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter>，可以在项目文件中重新启用它。</span><span class="sxs-lookup"><span data-stu-id="74d93-124">To continue using <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> in ASP.NET apps, you can re-enable it in the project file.</span></span> <span data-ttu-id="74d93-125">不过，非常不建议这样做。</span><span class="sxs-lookup"><span data-stu-id="74d93-125">However, it's strongly recommended not to do this.</span></span> <span data-ttu-id="74d93-126">有关详细信息，请参阅 [BinaryFormatter 安全指南](../../../../standard/serialization/binaryformatter-security-guide.md)。</span><span class="sxs-lookup"><span data-stu-id="74d93-126">For more information, see [BinaryFormatter security guide](../../../../standard/serialization/binaryformatter-security-guide.md).</span></span>

  ```xml
  <PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <!-- Warning: Setting the following switch is *NOT* recommended in web apps. -->
    <EnableUnsafeBinaryFormatterSerialization>true</EnableUnsafeBinaryFormatterSerialization>
  </PropertyGroup>
  ```

<span data-ttu-id="74d93-127">若要详细了解建议的操作，请参阅[修复 BinaryFormatter 过时和禁用错误](../../../../standard/serialization/binaryformatter-security-guide.md)。</span><span class="sxs-lookup"><span data-stu-id="74d93-127">For more information about recommended actions, see [Resolving BinaryFormatter obsoletion and disablement errors](../../../../standard/serialization/binaryformatter-security-guide.md).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="74d93-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="74d93-128">Affected APIs</span></span>

- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)?displayProperty=fullName>
- <xref:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)?displayProperty=fullName>

<!--

#### Category

- Core .NET libraries
- ASP.NET Core

### Affected APIs

- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize`
- `Overload:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize`
- `M:System.Runtime.Serialization.Formatter.Serialize(System.IO.Stream,System.Object)`
- `M:System.Runtime.Serialization.Formatter.Deserialize(System.IO.Stream)`
- `M:System.Runtime.Serialization.IFormatter.Serialize(System.IO.Stream,System.Object)`
- `M:System.Runtime.Serialization.IFormatter.Deserialize(System.IO.Stream)`

-->
