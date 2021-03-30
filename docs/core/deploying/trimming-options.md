---
title: 剪裁选项
description: 了解如何控制自包含应用的剪裁。
author: sbomer
ms.author: svbomer
ms.date: 08/25/2020
ms.openlocfilehash: 93fee991cf218a52ad1d9a2597b1c9b2d442110a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874246"
---
# <a name="trimming-options"></a>剪裁选项

以下 MSBuild 属性和项会影响[剪裁的独立部署](trim-self-contained.md)行为。 一些选项提及 `ILLink`，这是实现剪裁的基础工具的名称。 有关基础工具的详细信息，请参阅[链接器文档](https://github.com/mono/linker/tree/master/docs)。

## <a name="enable-trimming"></a>启用剪裁

- `<PublishTrimmed>true</PublishTrimmed>`

   在发布期间使用 SDK 定义的默认设置来启用剪裁。

使用 `Microsoft.NET.Sdk` 时，将对 netcoreapp 运行时包中的框架程序集执行程序集级剪裁。 而不会剪裁应用程序代码和非框架库。 其他 SDK 可定义不同的默认值。

## <a name="trimming-granularity"></a>剪裁粒度

以下粒度设置控制如何丢弃未充分利用的 IL。 这可设置为属性，也可设置为[单个程序集](#trimmed-assemblies)的元数据。

- `<TrimMode>copyused</TrimMode>`

   启用程序集级剪裁；如果使用程序集的任何部分（以静态理解的方式），则将保留整个程序集。

- `<TrimMode>link</TrimMode>`

    启用成员级剪裁，这会从类型中删除未使用的成员。

具有 `<IsTrimmable>true</IsTrimmable>` 元数据但没有显式 `TrimMode` 的程序集将使用全局 `TrimMode`。 `Microsoft.NET.Sdk` 的默认 `TrimMode` 为 `copyused`。

## <a name="trimmed-assemblies"></a>剪裁后的程序集

发布剪裁后的应用时，SDK 将计算名为 `ManagedAssemblyToLink` 的 `ItemGroup`，这表示要进行剪裁处理的文件集。 `ManagedAssemblyToLink` 可能具有控制每个程序集剪裁行为的元数据。 若要设置此元数据，请在内置的 `PrepareForILLink` 目标运行之前创建一个目标。 下面的示例演示如何启用 `MyAssembly` 的剪裁。

```xml
<Target Name="ConfigureTrimming"
        BeforeTargets="PrepareForILLink">
  <ItemGroup>
    <ManagedAssemblyToLink Condition="'%(Filename)' == 'MyAssembly'">
      <IsTrimmable>true</IsTrimmable>
    </ManagedAssemblyToLink>
  </ItemGroup>
</Target>
```

请勿在 `ManagedAssemblyToLink` 中添加或移除项，因为 SDK 会在发布过程中计算此集，并期望它没有发生更改。 支持的元数据为：

- `<IsTrimmable>true</IsTrimmable>`

  控制是否剪裁给定的程序集。

- `<TrimMode>copyused</TrimMode>` 或 `<TrimMode>link</TrimMode>`

  控制此程序集的[剪裁粒度](#trimming-granularity)。 这优先于全局 `TrimMode`。 在程序集上设置 `TrimMode` 意味着 `<IsTrimmable>true</IsTrimmable>`。

## <a name="root-assemblies"></a>根程序集

所有没有 `<IsTrimmable>true</IsTrimmable>` 的程序集都被视为分析的根，这意味着将保留它们及其所有静态理解的依赖项。 其他程序集的名称可能是根（无 `.dll` 扩展名）：

```xml
<ItemGroup>
  <TrimmerRootAssembly Include="MyAssembly" />
</ItemGroup>
```

## <a name="root-descriptors"></a>根描述符

指定分析的根的另一种方法是使用链接器[描述符格式](https://github.com/mono/linker/blob/master/docs/data-formats.md#descriptor-format)的 XML 文件。 这使你可以查找特定的成员而非整个程序集。

```xml
<ItemGroup>
  <TrimmerRootDescriptor Include="MyRoots.xml" />
</ItemGroup>
```

例如，`MyRoots.xml` 可能会查找由应用程序动态访问的特定方法：

```xml
<linker>
  <assembly fullname="MyAssembly">
    <type fullname="MyAssembly.MyClass">
      <method name="DynamicallyAccessedMethod" />
    </type>
  </assembly>
</linker>
```

## <a name="analysis-warnings"></a>分析警告

剪裁将删除不可静态访问的 IL。 使用反射或其他模式来创建动态依赖项的应用可能会因剪裁而中断。 警告此类模式：

- `<SuppressTrimAnalysisWarnings>false</SuppressTrimAnalysisWarnings>`

    启用剪裁分析警告。

这将包括有关整个应用的警告，其中包括你自己的代码、库代码以及框架代码。

## <a name="warning-versions"></a>警告版本

剪裁分析使用的 [`AnalysisLevel`](../project-sdk/msbuild-props.md#analysislevel) 属性可跨 SDK 控制分析警告版本。 还有一个可独立控制剪裁分析警告版本的属性（类似于编译器 `WarningLevel`）：

- `<ILLinkWarningLevel>5</ILLinkWarningLevel>`

    仅发出给定级别或更低级别的警告。 这可以是 `9999`，以包含所有警告版本。

## <a name="suppressing-warnings"></a>取消警告

可以使用工具链遵循的常用 MSBuild 属性来取消单个[警告代码](https://github.com/mono/linker/blob/master/docs/error-codes.md#warning-codes)，包括 `NoWarn`、`WarningsAsErrors`、`WarningsNotAsErrors` 和 `TreatWarningsAsErrors`。 还有另一个选项，可单独控制 ILLink 警告即错误的行为：

- `<ILLinkTreatWarningsAsErrors>false</ILLinkTreatWarningsAsErrors>`

    请勿将 ILLink 警告视为错误。 在全局将编译器警告视为错误时，这对于避免将剪裁分析警告转换为错误可能很有用。

## <a name="removing-symbols"></a>删除符号

通常会对符号进行剪裁，以匹配剪裁的程序集。 还可以删除所有符号：

- `<TrimmerRemoveSymbols>true</TrimmerRemoveSymbols>`

    删除剪裁后的应用程序中的符号，包括嵌入的 PDB 和单独的 PDB 文件。 这同时适用于应用程序代码以及符号附带的任何依赖项。

SDK 还可使用属性 `DebuggerSupport` 来禁用调试器支持。 禁用调试器支持时，剪裁将自动删除符号（`TrimmerRemoveSymbols` 将默认为 true）。

## <a name="trimming-framework-library-features"></a>剪裁框架库功能

框架库的一些功能区域随附有链接器指令，这些链接器指令可以删除已禁用功能的代码。

- `<DebuggerSupport>false</DebuggerSupport>`

    删除代码可提供更佳的调试体验。 此操作也会[删除符号](#removing-symbols)。

- `<EnableUnsafeBinaryFormatterSerialization>false</EnableUnsafeBinaryFormatterSerialization>`

    删除 BinaryFormatter 序列化支持。 有关详细信息，请参阅 [BinaryFormatter 序列化方法已过时](../compatibility/core-libraries/5.0/binaryformatter-serialization-obsolete.md)。

- `<EnableUnsafeUTF7Encoding>false</EnableUnsafeUTF7Encoding>`

    删除不安全的 UTF-7 编码代码。 有关详细信息，请参阅 [UTF-7 代码路径已过时](../compatibility/core-libraries/5.0/utf-7-code-paths-obsolete.md)。

- `<EventSourceSupport>false</EventSourceSupport>`

    删除与 EventSource 相关的代码或逻辑。

- `<HttpActivityPropagationSupport>false</HttpActivityPropagationSupport>`

    删除与 System.Net.Http 的诊断支持相关的代码。

- `<InvariantGlobalization>true</InvariantGlobalization>`

    删除全球化特定的代码和数据。 有关详细信息，请参阅[固定模式](../run-time-config/globalization.md#invariant-mode)。

- `<UseSystemResourceKeys>true</UseSystemResourceKeys>`

    删除 `System.*` 程序集的异常消息。 当 `System.*` 程序集中引发异常时，该消息将是简化的资源 ID，而不是完整的消息。

 这些属性将导致剪裁相关代码，同时还将通过 [runtimeconfig](../run-time-config/index.md) 文件禁用功能。 有关这些属性（包括相应的 runtimeconfig 选项）的详细信息，请参阅[功能切换](https://github.com/dotnet/runtime/blob/main/docs/workflow/trimming/feature-switches.md)。 某些 SDK 可能具有这些属性的默认值。
