---
title: 中断性变更：CA1416：平台兼容性
description: 了解 .NET 5 中因启用代码分析规则 CA1416 而导致的中断性变更。
ms.date: 09/29/2020
ms.openlocfilehash: fa03e1f0bfa8438b3b2899aaf7c97f42533c7c02
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477546"
---
# <a name="warning-ca1416-platform-compatibility"></a>警告 CA1416：平台兼容性

从 .NET 5.0 开始，默认启用 .NET 代码分析器规则 [CA1416](/visualstudio/code-quality/ca1416)。 它会针对从未验证操作系统的调用站点调用特定于平台的 API 生成一个生成警告。

## <a name="change-description"></a>更改说明

从 .NET 5 开始，.NET SDK 包括 [.NET 源代码分析器](../../../../fundamentals/code-analysis/overview.md)。 其中一些规则会默认启用，包括 [CA1416](/visualstudio/code-quality/ca1416)。 如果项目包含违反此规则的代码，并已被配置为将警告视为错误，则此更改可能会中断生成。 当你在平台上下文未验证的位置使用特定于平台的 API 时，规则 CA1416 会通知你。

规则 [CA1416](/visualstudio/code-quality/ca1416)（平台兼容性分析器）与 .NET 5.0 中新增的一些其他功能配合工作。 .NET 5 引入了 <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> 和 <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute>，可用于指定支持或不支持 API 的平台 。 如果没有这些属性，则假定所有平台都支持 API。 这些属性已应用于核心 .NET 库中特定于平台的 API。

在针对其所使用的 API 不可用的平台的项目中，规则 [CA1416](/visualstudio/code-quality/ca1416) 会标记任何未验证平台上下文的特定于平台的 API 调用。 现在，在不受支持的操作系统上调用 API 时，大多数用 <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> 和 <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> 属性修饰的 API 都会引发 <xref:System.PlatformNotSupportedException> 异常。 这些 API 被标记为特定于平台的 API 之后，规则 [CA1416](/visualstudio/code-quality/ca1416) 可以通过将 OS 检查添加到调用站点来帮助防止运行时 <xref:System.PlatformNotSupportedException> 异常。

## <a name="examples"></a>示例

- <xref:System.Console.Beep(System.Int32,System.Int32)?displayProperty=nameWithType> 方法仅在 Windows 上受支持，并且该方法修饰有 `[SupportedOSPlatform("windows")]`。 如果项目[目标](../../../../standard/frameworks.md)是 `net5.0`（但不是 `net5.0-windows`），则以下代码将在生成时生成 CA1416 警告。 有关为避免该警告可以执行的操作，请参阅[建议的操作](#recommended-action)。

  ```csharp
  public void PlayCMajor()
  {
      Console.Beep(261, 1000);
  }
  ```

- 浏览器不支持 <xref:System.Drawing.Image.FromFile(System.String)?displayProperty=nameWithType> 方法，并且该方法修饰有 `[UnsupportedOSPlatform("browser")]`。 如果项目支持浏览器平台，则以下代码将在生成时生成 CA1416 警告。

  ```csharp
  public void CreateImage()
  {
      Image newImage = Image.FromFile("SampImag.jpg");
  }
  ```

  > [!TIP]
  >
  > - Blazor WebAssembly 项目和 Razor 类库项目自动提供浏览器支持。
  > - 若要手动添加浏览器作为项目的支持平台，请将以下项添加到项目文件中：
  >
  >  ```xml
  >  <ItemGroup>
  >    <SupportedPlatform Include="browser" />
  >  </ItemGroup>
  >  ```

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="recommended-action"></a>建议操作

确保仅当代码在适当的平台上运行时，才调用特定于平台的 API。 在调用特定于平台的 API 之前，可以使用 <xref:System.OperatingSystem?displayProperty=nameWithType> 类中的 `Is<Platform>` 方法之一（例如 <xref:System.OperatingSystem.IsWindows?displayProperty=nameWithType>）检查当前操作系统。

可以在 `if` 语句条件下使用 `Is<Platform>` 方法之一：

```csharp
public void PlayCMajor()
{
    if (OperatingSystem.IsWindows())
    {
        Console.Beep(261, 1000);
    }
}
```

或者，如果不希望在运行时增加额外的 `if` 语句，请改为调用 <xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType>：

```csharp
public void PlayCMajor()
{
    Debug.Assert(OperatingSystem.IsWindows());
    Console.Beep(261, 1000);
}
```

如果你正在创作库，可以将 API 标记为平台专用。 在这种情况下，检查要求的负担就落在了调用方身上。 可以标记特定方法或类型，也可以标记整个程序集。

```csharp
[SupportedOSPlatform("windows")]
public void PlayCMajor()
{
    Console.Beep(261, 1000);
}
```

如果不想修复所有调用站点，则可以选择以下某个选项以禁止显示警告：

- 若要禁止显示规则 CA1416，可以使用 `#pragma` 或 [DisabledWarnings](../../../../csharp/language-reference/compiler-options/errors-warnings.md#disabledwarnings) 编译器标志来实现此目的，也可以通过在 .editorconfig 文件中[设置规则的严重性](../../../../fundamentals/code-analysis/configuration-options.md#severity-level)，将值设置为 `none` 来实现此目的。

  ```csharp
  public void PlayCMajor()
  {
  #pragma warning disable CA1416
      Console.Beep(261, 1000);
  #pragma warning restore CA1416
  }
  ```

- 若要完全禁用代码分析，请在项目文件中将 `EnableNETAnalyzers` 设置为 `false`。 有关详细信息，请参阅 [EnableNETAnalyzers](../../../project-sdk/msbuild-props.md#enablenetanalyzers)。

## <a name="affected-apis"></a>受影响的 API

对于 Windows 平台：

- <https://github.com/dotnet/designs/blob/master/accepted/2020/windows-specific-apis/windows-specific-apis.md> 上列出的所有 API。
- <xref:System.Security.Cryptography.DSAOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.ECDiffieHellmanOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.ECDsaOpenSsl?displayProperty=fullName>
- <xref:System.Security.Cryptography.RSAOpenSsl?displayProperty=fullName>

对于 Blazor WebAssembly 平台：

- <https://github.com/dotnet/runtime/issues/41087> 上列出的所有 API。

<!--

### Affected APIs

- ``

### Category

- Code analysis
- Core .NET libraries

-->

## <a name="see-also"></a>另请参阅

- [CA1416：验证平台兼容性](/visualstudio/code-quality/ca1416)
- [.NET API 分析器](../../../../standard/analyzers/api-analyzer.md)
