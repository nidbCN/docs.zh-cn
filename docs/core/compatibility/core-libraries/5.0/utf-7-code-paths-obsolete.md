---
title: 中断性变更：UTF-7 代码路径已过时
description: 了解核心 .NET 库中的 .NET 5 中断性变更：UTF7 和 UTF7Encoding 构造函数已过时。
ms.date: 11/01/2020
ms.openlocfilehash: 7a0ba771e0fac2908ca37f16afb10118e1537161
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256980"
---
# <a name="utf-7-code-paths-are-obsolete"></a><span data-ttu-id="cf83a-103">UTF-7 代码路径已过时</span><span class="sxs-lookup"><span data-stu-id="cf83a-103">UTF-7 code paths are obsolete</span></span>

<span data-ttu-id="cf83a-104">UTF-7 编码在应用程序中不再广泛使用，并且许多规范现在在交换中[禁止其使用](https://security.stackexchange.com/a/68609/3573)。</span><span class="sxs-lookup"><span data-stu-id="cf83a-104">The UTF-7 encoding is no longer in wide use among applications, and many specs now [forbid its use](https://security.stackexchange.com/a/68609/3573) in interchange.</span></span> <span data-ttu-id="cf83a-105">它偶尔还会在不期望遇到 UTF-7 编码数据的应用程序中[用作攻击途径](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=utf-7)。</span><span class="sxs-lookup"><span data-stu-id="cf83a-105">It's also occasionally [used as an attack vector](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=utf-7) in applications that don't anticipate encountering UTF-7-encoded data.</span></span> <span data-ttu-id="cf83a-106">Microsoft 警告不要使用 <xref:System.Text.UTF7Encoding?displayProperty=nameWithType>，因为它不提供错误检测。</span><span class="sxs-lookup"><span data-stu-id="cf83a-106">Microsoft warns against use of <xref:System.Text.UTF7Encoding?displayProperty=nameWithType> because it doesn't provide error detection.</span></span>

<span data-ttu-id="cf83a-107">因此，<xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 属性和 <xref:System.Text.UTF7Encoding.%23ctor%2A> 构造函数现已过时。</span><span class="sxs-lookup"><span data-stu-id="cf83a-107">Consequently, the <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> property and <xref:System.Text.UTF7Encoding.%23ctor%2A> constructors are now obsolete.</span></span> <span data-ttu-id="cf83a-108">此外，<xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> 和 <xref:System.Text.Encoding.GetEncodings%2A?displayProperty=nameWithType> 不再允许你指定 `UTF-7`。</span><span class="sxs-lookup"><span data-stu-id="cf83a-108">Additionally, <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> and <xref:System.Text.Encoding.GetEncodings%2A?displayProperty=nameWithType> no longer allow you to specify `UTF-7`.</span></span>

## <a name="change-description"></a><span data-ttu-id="cf83a-109">更改描述</span><span class="sxs-lookup"><span data-stu-id="cf83a-109">Change description</span></span>

<span data-ttu-id="cf83a-110">以前，你可以使用 <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> API 创建 UTF-7 编码的实例。</span><span class="sxs-lookup"><span data-stu-id="cf83a-110">Previously, you could create an instance of the UTF-7 encoding by using the <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> APIs.</span></span> <span data-ttu-id="cf83a-111">例如：</span><span class="sxs-lookup"><span data-stu-id="cf83a-111">For example:</span></span>

```csharp
Encoding enc1 = Encoding.GetEncoding("utf-7"); // By name.
Encoding enc2 = Encoding.GetEncoding(65000); // By code page.
```

<span data-ttu-id="cf83a-112">此外，表示 UTF-7 编码的实例是由 <xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> 方法枚举的，该方法枚举在系统上注册的所有 <xref:System.Text.Encoding> 实例。</span><span class="sxs-lookup"><span data-stu-id="cf83a-112">Additionally, an instance that represents the UTF-7 encoding was enumerated by the <xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> method, which enumerates all the <xref:System.Text.Encoding> instances registered on the system.</span></span>

<span data-ttu-id="cf83a-113">从 .NET 5 开始，<xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 属性和 <xref:System.Text.UTF7Encoding.%23ctor%2A> 构造函数已过时并生成警告 `SYSLIB0001`（若在预览版中，则生成 `MSLIB0001`）。</span><span class="sxs-lookup"><span data-stu-id="cf83a-113">Starting in .NET 5, the <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> property and <xref:System.Text.UTF7Encoding.%23ctor%2A> constructors are obsolete and produce warning `SYSLIB0001` (or `MSLIB0001` in preview versions).</span></span> <span data-ttu-id="cf83a-114">但是，若要减少在使用 <xref:System.Text.UTF7Encoding> 类时调用方收到的警告数，则 <xref:System.Text.UTF7Encoding> 类型本身不会标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="cf83a-114">However, to reduce the number of warnings that callers receive when using the <xref:System.Text.UTF7Encoding> class, the <xref:System.Text.UTF7Encoding> type itself is not marked obsolete.</span></span>

```csharp
// The next line generates warning SYSLIB0001.
UTF7Encoding enc = new UTF7Encoding();
// The next line does not generate a warning.
byte[] bytes = enc.GetBytes("Hello world!");
```

<span data-ttu-id="cf83a-115">此外，<xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> 方法将编码名称 `utf-7` 和代码页 `65000` 视为 `unknown`。</span><span class="sxs-lookup"><span data-stu-id="cf83a-115">Additionally, the <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> methods treat the encoding name `utf-7` and the code page `65000` as `unknown`.</span></span> <span data-ttu-id="cf83a-116">将编码视为 `unknown` 会使方法引发 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="cf83a-116">Treating the encoding as `unknown` causes the method to throw an <xref:System.ArgumentException>.</span></span>

```csharp
// Throws ArgumentException, same as calling Encoding.GetEncoding("unknown").
Encoding enc = Encoding.GetEncoding("utf-7");
```

<span data-ttu-id="cf83a-117">最后，<xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> 方法不会将 UTF-7 编码包括在它返回的 <xref:System.Text.EncodingInfo> 数组中。</span><span class="sxs-lookup"><span data-stu-id="cf83a-117">Finally, the <xref:System.Text.Encoding.GetEncodings?displayProperty=nameWithType> method doesn't include the UTF-7 encoding in the <xref:System.Text.EncodingInfo> array that it returns.</span></span> <span data-ttu-id="cf83a-118">已排除编码，因为无法对其进行实例化。</span><span class="sxs-lookup"><span data-stu-id="cf83a-118">The encoding is excluded because it cannot be instantiated.</span></span>

```csharp
foreach (EncodingInfo encInfo in Encoding.GetEncodings())
{
    // The next line would throw if GetEncodings included UTF-7.
    Encoding enc = Encoding.GetEncoding(encInfo.Name);
}
```

## <a name="reason-for-change"></a><span data-ttu-id="cf83a-119">更改原因</span><span class="sxs-lookup"><span data-stu-id="cf83a-119">Reason for change</span></span>

<span data-ttu-id="cf83a-120">许多应用程序使用由不受信任的源提供的编码名称值调用 `Encoding.GetEncoding("encoding-name")`。</span><span class="sxs-lookup"><span data-stu-id="cf83a-120">Many applications call `Encoding.GetEncoding("encoding-name")` with an encoding name value that's provided by an untrusted source.</span></span> <span data-ttu-id="cf83a-121">例如，Web 客户端或服务器可能采用 `Content-Type` 标头的 `charset` 部分，并且不进行验证将值直接传递给 `Encoding.GetEncoding`。</span><span class="sxs-lookup"><span data-stu-id="cf83a-121">For example, a web client or server might take the `charset` portion of the `Content-Type` header and pass the value directly to `Encoding.GetEncoding` without validating it.</span></span> <span data-ttu-id="cf83a-122">这可能会使恶意终结点指定 `Content-Type: ...; charset=utf-7`，而这可能会导致接收应用程序出现异常行为。</span><span class="sxs-lookup"><span data-stu-id="cf83a-122">This could allow a malicious endpoint to specify `Content-Type: ...; charset=utf-7`, which could cause the receiving application to misbehave.</span></span>

<span data-ttu-id="cf83a-123">此外，禁用 UTF-7 代码路径可以优化编译器（如 Blazor 使用的编译器），以便从生成的应用程序中完全删除这些代码路径。</span><span class="sxs-lookup"><span data-stu-id="cf83a-123">Additionally, disabling UTF-7 code paths allows optimizing compilers, such as those used by Blazor, to remove these code paths entirely from the resulting application.</span></span> <span data-ttu-id="cf83a-124">因此，已编译的应用程序会更有效地运行，且占用更少的磁盘空间。</span><span class="sxs-lookup"><span data-stu-id="cf83a-124">As a result, the compiled applications run more efficiently and take less disk space.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="cf83a-125">引入的版本</span><span class="sxs-lookup"><span data-stu-id="cf83a-125">Version introduced</span></span>

<span data-ttu-id="cf83a-126">5.0</span><span class="sxs-lookup"><span data-stu-id="cf83a-126">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="cf83a-127">建议操作</span><span class="sxs-lookup"><span data-stu-id="cf83a-127">Recommended action</span></span>

<span data-ttu-id="cf83a-128">在大多数情况下，你不必执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="cf83a-128">In most cases, you don't need to take any action.</span></span> <span data-ttu-id="cf83a-129">但是，对于以前激活了与 UTF-7 相关的代码路径的应用，请考虑下面的指南。</span><span class="sxs-lookup"><span data-stu-id="cf83a-129">However, for apps that have previously activated UTF-7-related code paths, consider the guidance that follows.</span></span>

- <span data-ttu-id="cf83a-130">如果你的应用使用不受信任的源提供的未知编码名称调用 <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType>：</span><span class="sxs-lookup"><span data-stu-id="cf83a-130">If your app calls <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> with unknown encoding names provided by an untrusted source:</span></span>

  <span data-ttu-id="cf83a-131">请改为将编码名称与可配置的允许列表进行比较。</span><span class="sxs-lookup"><span data-stu-id="cf83a-131">Instead, compare the encoding names against a configurable allow list.</span></span> <span data-ttu-id="cf83a-132">可配置的允许列表至少应包含行业标准的“utf-8”。</span><span class="sxs-lookup"><span data-stu-id="cf83a-132">The configurable allow list should at minimum include the industry-standard "utf-8".</span></span> <span data-ttu-id="cf83a-133">根据你的客户端和法规要求，你可能还需要允许特定于区域的编码，如“GB18030”。</span><span class="sxs-lookup"><span data-stu-id="cf83a-133">Depending on your clients and regulatory requirements, you may also need to allow region-specific encodings, such as "GB18030".</span></span>

  <span data-ttu-id="cf83a-134">如果未执行允许列表，<xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> 将返回内置于系统中或通过自定义 <xref:System.Text.EncodingProvider> 注册的任何 <xref:System.Text.Encoding>。</span><span class="sxs-lookup"><span data-stu-id="cf83a-134">If you don't implement an allow list, <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> will return any <xref:System.Text.Encoding> that's built into the system or that's registered via a custom <xref:System.Text.EncodingProvider>.</span></span> <span data-ttu-id="cf83a-135">审核你服务的要求以验证这是否是所需的行为。</span><span class="sxs-lookup"><span data-stu-id="cf83a-135">Audit your service's requirements to validate that this is the desired behavior.</span></span> <span data-ttu-id="cf83a-136">除非你的应用程序重新启用本文后面所述的兼容性开关，否则默认情况下将继续禁用 UTF-7。</span><span class="sxs-lookup"><span data-stu-id="cf83a-136">UTF-7 continues to be disabled by default unless your application re-enables the compatibility switch mentioned later in this article.</span></span>

- <span data-ttu-id="cf83a-137">如果在你自己的协议或文件格式中使用的是 <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 或 <xref:System.Text.UTF7Encoding>：</span><span class="sxs-lookup"><span data-stu-id="cf83a-137">If you're using <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> or <xref:System.Text.UTF7Encoding> within your own protocol or file format:</span></span>

  <span data-ttu-id="cf83a-138">切换到使用 <xref:System.Text.Encoding.UTF8?displayProperty=nameWithType> 或 <xref:System.Text.UTF8Encoding>。</span><span class="sxs-lookup"><span data-stu-id="cf83a-138">Switch to using <xref:System.Text.Encoding.UTF8?displayProperty=nameWithType> or <xref:System.Text.UTF8Encoding>.</span></span> <span data-ttu-id="cf83a-139">UTF-8 是一种行业标准，并且受到语言、操作系统和运行时的广泛支持。</span><span class="sxs-lookup"><span data-stu-id="cf83a-139">UTF-8 is an industry standard and is widely supported across languages, operating systems, and runtimes.</span></span> <span data-ttu-id="cf83a-140">使用 UTF-8 简化了代码的将来维护，并使其与生态系统的其余部分更具互操作性。</span><span class="sxs-lookup"><span data-stu-id="cf83a-140">Using UTF-8 eases future maintenance of your code and makes it more interoperable with the rest of the ecosystem.</span></span>

- <span data-ttu-id="cf83a-141">如果要将 <xref:System.Text.Encoding> 实例与 <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 进行比较：</span><span class="sxs-lookup"><span data-stu-id="cf83a-141">If you're comparing an <xref:System.Text.Encoding> instance against <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType>:</span></span>

  <span data-ttu-id="cf83a-142">可转为考虑针对众所周知的 UTF-7 代码页（即 `65000`）执行检查。</span><span class="sxs-lookup"><span data-stu-id="cf83a-142">Instead, consider performing a check against the well-known UTF-7 code page, which is `65000`.</span></span> <span data-ttu-id="cf83a-143">通过与代码页进行比较，可以避免出现警告，还可以处理一些边缘情况，例如，如果有人调用 `new UTF7Encoding()` 或子类化类型。</span><span class="sxs-lookup"><span data-stu-id="cf83a-143">By comparing against the code page, you avoid the warning and also handle some edge cases, such as if somebody called `new UTF7Encoding()` or subclassed the type.</span></span>

  ```csharp
  void DoSomething(Encoding enc)
  {
      // Don't perform the check this way.
      // It produces a warning and misses some edge cases.
      if (enc == Encoding.UTF7)
      {
          // Encoding is UTF-7.
      }

      // Instead, perform the check this way.
      if (enc != null && enc.CodePage == 65000)
      {
          // Encoding is UTF-7.
      }
  }
  ```

- <span data-ttu-id="cf83a-144">如果必须使用 <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 或 <xref:System.Text.UTF7Encoding>：</span><span class="sxs-lookup"><span data-stu-id="cf83a-144">If you must use <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> or <xref:System.Text.UTF7Encoding>:</span></span>

  <span data-ttu-id="cf83a-145">可以在代码中或在项目的 *.csproj* 文件中禁止显示 `SYSLIB0001` 警告。</span><span class="sxs-lookup"><span data-stu-id="cf83a-145">You can suppress the `SYSLIB0001` warning in code or within your project's *.csproj* file.</span></span>

  ```csharp
  #pragma warning disable SYSLIB0001 // Disable the warning.
  Encoding enc = Encoding.UTF7;
  #pragma warning restore SYSLIB0001 // Re-enable the warning.
  ```

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- NoWarn below suppresses SYSLIB0001 project-wide -->
     <NoWarn>$(NoWarn);SYSLIB0001</NoWarn>
    </PropertyGroup>
  </Project>
  ```

  > [!NOTE]
  > <span data-ttu-id="cf83a-146">禁止显示 `SYSLIB0001` 只会禁用 <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> 和 <xref:System.Text.UTF7Encoding> 过时警告。</span><span class="sxs-lookup"><span data-stu-id="cf83a-146">Suppressing `SYSLIB0001` only disables the <xref:System.Text.Encoding.UTF7?displayProperty=nameWithType> and <xref:System.Text.UTF7Encoding> obsoletion warnings.</span></span> <span data-ttu-id="cf83a-147">不会禁用任何其他警告，也不会更改 API 的行为，如 <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="cf83a-147">It doesn't disable any other warnings or change the behavior of APIs like <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType>.</span></span>

- <span data-ttu-id="cf83a-148">如果必须支持 `Encoding.GetEncoding("utf-7", ...)`：</span><span class="sxs-lookup"><span data-stu-id="cf83a-148">If you must support `Encoding.GetEncoding("utf-7", ...)`:</span></span>

  <span data-ttu-id="cf83a-149">可以通过兼容性开关重新启用对此项的支持。</span><span class="sxs-lookup"><span data-stu-id="cf83a-149">You can re-enable support for this via a compatibility switch.</span></span> <span data-ttu-id="cf83a-150">可以在应用程序的 *.csproj* 文件或 [运行时配置文件](../../../run-time-config/index.md)中指定此兼容性开关，如以下示例中所示。</span><span class="sxs-lookup"><span data-stu-id="cf83a-150">This compatibility switch can be specified in the application's *.csproj* file or in a [run-time configuration file](../../../run-time-config/index.md), as shown in the following examples.</span></span>

  <span data-ttu-id="cf83a-151">在应用程序的 *.csproj* 文件中：</span><span class="sxs-lookup"><span data-stu-id="cf83a-151">In the application's *.csproj* file:</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- Re-enable support for UTF-7 -->
     <EnableUnsafeUTF7Encoding>true</EnableUnsafeUTF7Encoding>
    </PropertyGroup>
  </Project>
  ```

  <span data-ttu-id="cf83a-152">在应用程序的 *runtimeconfig.template.json* 文件中：</span><span class="sxs-lookup"><span data-stu-id="cf83a-152">In the application's *runtimeconfig.template.json* file:</span></span>

  ```json
  {
    "configProperties": {
      "System.Text.Encoding.EnableUnsafeUTF7Encoding": true
    }
  }
  ```

  > [!TIP]
  > <span data-ttu-id="cf83a-153">如果重新启用对 UTF-7 的支持，则应对调用 <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType> 的代码执行安全检查。</span><span class="sxs-lookup"><span data-stu-id="cf83a-153">If you re-enable support for UTF-7, you should perform a security review of code that calls <xref:System.Text.Encoding.GetEncoding%2A?displayProperty=nameWithType>.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="cf83a-154">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="cf83a-154">Affected APIs</span></span>

- <xref:System.Text.Encoding.UTF7?displayProperty=fullName>
- <xref:System.Text.UTF7Encoding.%23ctor>
- <xref:System.Text.UTF7Encoding.%23ctor(System.Boolean)>
- <xref:System.Text.Encoding.GetEncoding(System.Int32)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.String)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.Int32,System.Text.EncoderFallback,System.Text.DecoderFallback)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncoding(System.String,System.Text.EncoderFallback,System.Text.DecoderFallback)?displayProperty=fullName>
- <xref:System.Text.Encoding.GetEncodings?displayProperty=fullName>

<!--

#### Category

- Core .NET libraries
- Security

### Affected APIs

- `System.Text.Encoding.UTF7`
- `System.Text.UTF7Encoding.#ctor`
- `System.Text.UTF7Encoding.#ctor(System.Boolean)`
- `System.Text.Encoding.GetEncoding(System.Int32)`
- `System.Text.Encoding.GetEncoding(System.String)`
- `System.Text.Encoding.GetEncoding(System.Int32,System.Text.EncoderFallback,System.Text.DecoderFallback)`
- `System.Text.Encoding.GetEncoding(System.String,System.Text.EncoderFallback,System.Text.DecoderFallback)`
- `System.Text.Encoding.GetEncodings`

-->
