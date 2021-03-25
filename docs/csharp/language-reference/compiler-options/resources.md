---
description: 用于控制嵌入在 dotnet 应用程序中的 Windows 资源的 C# 编译器选项。
title: C# 编译器选项 - 资源选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- Win32Resource compiler option [C#]
- Win32Icon compiler option [C#]
- Win32Manifest compiler option [C#]
- NoWin32Manifest compiler option [C#]
- Resources compiler option [C#]
- LinkResources compiler option [C#]
ms.openlocfilehash: fe2da8cae67bc597d2b84a395861a0e4c32c9d72
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482405"
---
# <a name="c-compiler-options-that-specify-resources"></a>指定资源的 C# 编译器选项

以下选项控制 C# 编译器如何创建或导入 Win32 资源。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **Win32Resource** / `-win32res`：指定 Win32 资源文件 (.res)。
- **Win32Icon** / `-win32icon`：引用来自一个或多个指定程序集文件的元数据。
- **Win32Manifest** / `-win32manifest`：指定 Win32 清单文件 (.xml)。
- **NoWin32Manifest** / `-nowin32manifest`：不包括默认的 Win32 清单。
- **Resources** / `-resource`：嵌入指定的资源（简短形式：/res）。
- **LinkResources** / `-linkresources`：将指定的资源链接到此程序集。

## <a name="win32resource"></a>Win32Resource

Win32Resource 选项会在输出文件中插入 Win32 资源。

```xml
<Win32Resource>filename</Win32Resource>
```

`filename` 是要添加到输出文件的资源文件。 Win32 资源可以包含版本或位图（图标）信息，这些信息有助于在文件资源管理器中标识您的应用程序。 如果不指定此选项，编译器将根据程序集版本生成版本信息。

## <a name="win32icon"></a>Win32Icon

Win32Icon 选项在输出文件中插入 .ico 文件，为输出文件提供在文件资源管理器中所需的外观。
  
```xml
<Win32Icon>filename</Win32Icon>
```

`filename` 是要添加到输出文件的 .ico 文件。 可以使用[资源编译器](/windows/desktop/menurc/resource-compiler)创建 .ico 文件。 在编译 Visual C++ 程序时会调用资源编译器；.ico 文件是从 .rc 文件创建的。

## <a name="win32manifest"></a>Win32Manifest

使用 Win32Manifest 选项可以指定要嵌入到项目的可移植可执行 (PE) 文件中的用户定义的 Win32 应用程序清单文件。

```xml
<Win32Manifest>filename</Win32Manifest>
```

`filename` 是自定义清单文件的名称和位置。 默认情况下，C# 编译器嵌入可指定“asInvoker”的请求执行级别的应用程序清单。 它在生成该可执行文件的同一文件夹中创建清单。 如果要提供自定义清单（例如，指定“highestAvailable”或“requireAdministrator”的请求执行级别的清单），请使用此选项指定文件名。

> [!NOTE]
> 此选项和 Win32Resources 选项是互斥的。 如果尝试在同一命令行中同时使用这两个选项，将收到一个生成错误。

如果应用程序没有用于指定请求执行级别的应用程序清单，将受到 Windows 中“用户帐户控制”功能下的文件和注册表虚拟化的影响。 有关详细信息，请参阅[用户帐户控制](/windows/access-protection/user-account-control/user-account-control-overview)。

如果满足下列任一条件，则应用程序会受到虚拟化的影响：
  
- 使用 NoWin32Manifest 选项，并且在随后的生成步骤中未提供清单，或者没有通过使用 Win32Resource 选项将其包含在 Windows 资源 (.res) 文件中。
- 提供的自定义清单未指定请求执行级别。

Visual Studio 创建默认 .manifest 文件，并将它与可执行文件一起存储在“调试”和“发布”目录中。 可以用任意文本编辑器创建一个清单，然后将该文件添加到项目中，从而添加自定义清单。 或者，也可以右键单击“解决方案资源管理器”中的“项目”图标，选择“添加新项”，然后选择“应用程序清单文件”。 添加完新的或现有清单文件后，该文件将显示在“清单”下拉列表中。 有关详细信息，请参阅[“项目设计器”->“应用程序”页 (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp)。

提供应用程序清单的操作可以作为自定义生成后步骤，也可以通过使用 NoWin32Manifest 选项作为 Win32 资源文件的组成部分。 如果希望应用程序受到 Windows Vista 的文件或注册表虚拟化的影响，请使用该选项。
  
## <a name="nowin32manifest"></a>NoWin32Manifest

使用 NoWin32Manifest 选项可指示编译器不将任何应用程序清单嵌入到可执行文件中。

```xml  
<NoWin32Manifest />
```

使用此选项时，除非在 Win32 资源文件或以后的生成步骤中提供应用程序清单，否则应用程序会受到 Windows Vista 上虚拟化的影响。

在 Visual Studio 的“应用程序属性”页中，通过在“清单”下拉列表中选择“创建不带清单的应用程序”选项来设置此选项。 有关详细信息，请参阅[“项目设计器”->“应用程序”页 (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp)。

## <a name="resources"></a>资源

将指定资源嵌入输出文件。

```xml
<Resources Include=filename>
  <LogicalName>identifier</LogicalName>
  <Access>accessibility-modifier</Access>
</Resources>
```

`filename` 是要嵌入到输出文件的 .NET 资源文件。 `identifier`（可选）是资源的逻辑名称；用于加载资源的名称。 默认值是文件的名称。 `accessibility-modifier`（可选）是资源的可访问性：public 或 private。 默认值为 public。 默认情况下，如果使用 C# 编译器创建资源，则这些资源在程序集中是公有的。 若要使资源变为私有，请将 `private` 指定为可访问性修饰符。 不允许使用 `public` 或 `private` 以外的任何其他可访问性。 例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。 输出文件中资源的顺序由项目文件中所指定的顺序决定。  
  
## <a name="linkresources"></a>LinkResources

在输出文件中创建指向 .NET 资源的链接。 不会在输出文件中添加资源文件。 LinkResources 不同于会在输出文件中嵌入资源文件的 Resource 选项。

```xml
<LinkResources Include=filename>
  <LogicalName>identifier</LogicalName>
  <Access>accessibility-modifier</Access>
</LinkResources>
```

`filename` 是要从程序集链接到的 .NET 资源文件。 `identifier`（可选）是资源的逻辑名称；用于加载资源的名称。 默认值是文件的名称。 `accessibility-modifier`（可选）是资源的可访问性：public 或 private。 默认值为 public。 默认情况下，如果使用 C# 编译器创建链接资源，则这些资源在程序集中是公有的。 若要使资源变为私有，请将 `private` 指定为可访问性修饰符。 不允许使用 `public` 或 `private` 以外的任何其他修饰符。 例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。 `filename` 中指定的文件可为任何格式。 例如，你可能希望生成程序集的本机 DLL 部分，从而可将它安装到全局程序集缓存中，并且可从该程序集中的托管代码访问它。 可在程序集链接器中执行相同的操作。 有关详细信息，请参阅 [Al.exe（程序集链接器）](../../../framework/tools/al-exe-assembly-linker.md)和[使用程序集和全局程序集缓存](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)。
