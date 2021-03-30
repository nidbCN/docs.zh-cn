---
title: 使用插件创建 .NET Core 应用程序
description: 了解如何创建支持插件的 .NET Core 应用程序。
author: jkoritzinsky
ms.author: jekoritz
ms.date: 10/16/2019
ms.openlocfilehash: aef91231bd4a32937d6e3cd2cb7204777c6efe96
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873375"
---
# <a name="create-a-net-core-application-with-plugins"></a>使用插件创建 .NET Core 应用程序

本教程展示了如何创建自定义的 <xref:System.Runtime.Loader.AssemblyLoadContext> 来加载插件。 <xref:System.Runtime.Loader.AssemblyDependencyResolver> 用于解析插件的依赖项。 该教程正确地将插件依赖项与主机应用程序隔离开来。 将了解如何执行以下操作：

- 构建支持插件的项目。
- 创建自定义 <xref:System.Runtime.Loader.AssemblyLoadContext> 加载每个插件。
- 使用 <xref:System.Runtime.Loader.AssemblyDependencyResolver?displayProperty=fullName> 类型允许插件具有依赖项。
- 只需复制生成项目就可以轻松部署的作者插件。

## <a name="prerequisites"></a>系统必备

- 安装 [.NET 5 SDK](https://dotnet.microsoft.com/download) 或更高版本。

> [!NOTE]
> 示例代码针对 .NET 5，但它使用的所有功能都已在 .NET Core 3.0 中推出，并且在此后所有 .NET 版本中都可用。

## <a name="create-the-application"></a>创建应用程序

第一步是创建应用程序：

1. 创建新文件夹，并在该文件夹中运行以下命令：

    ```dotnetcli
    dotnet new console -o AppWithPlugin
    ```

2. 为了更容易生成项目，请在同一文件夹中创建一个 Visual Studio 解决方案文件。 运行以下命令：

    ```dotnetcli
    dotnet new sln
    ```

3. 运行以下命令，向解决方案添加应用项目：

    ```dotnetcli
    dotnet sln add AppWithPlugin/AppWithPlugin.csproj
    ```

现在，我们可以填写应用程序的主干。 使用下面的代码替换 AppWithPlugin/Program.cs 文件中的代码： 

```csharp
using PluginBase;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Reflection;

namespace AppWithPlugin
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                if (args.Length == 1 && args[0] == "/d")
                {
                    Console.WriteLine("Waiting for any key...");
                    Console.ReadLine();
                }

                // Load commands from plugins.

                if (args.Length == 0)
                {
                    Console.WriteLine("Commands: ");
                    // Output the loaded commands.
                }
                else
                {
                    foreach (string commandName in args)
                    {
                        Console.WriteLine($"-- {commandName} --");

                        // Execute the command with the name passed as an argument.

                        Console.WriteLine();
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex);
            }
        }
    }
}

```

## <a name="create-the-plugin-interfaces"></a>创建插件接口

使用插件生成应用的下一步是定义插件需要实现的接口。 我们建议创建类库，其中包含计划用于在应用和插件之间通信的任何类型。 此部分允许将插件接口作为包发布，而无需发布完整的应用程序。

在项目的根文件夹中，运行 `dotnet new classlib -o PluginBase`。 并运行 `dotnet sln add PluginBase/PluginBase.csproj` 向解决方案文件添加项目。 删除 `PluginBase/Class1.cs` 文件，并使用以下接口定义在名为 `ICommand.cs` 的 `PluginBase` 文件夹中创建新的文件：

[!code-csharp[the-plugin-interface](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/PluginBase/ICommand.cs)]

此 `ICommand` 接口是所有插件将实现的接口。

由于已定义 `ICommand` 接口，所以应用程序项目可以填写更多内容。 使用根文件夹中的 `dotnet add AppWithPlugin/AppWithPlugin.csproj reference PluginBase/PluginBase.csproj` 命令将引用从 `AppWithPlugin` 项目添加到 `PluginBase` 项目。

使用以下代码片段替换 `// Load commands from plugins` 注释，使其能够从给定文件路径加载插件：

```csharp
string[] pluginPaths = new string[]
{
    // Paths to plugins to load.
};

IEnumerable<ICommand> commands = pluginPaths.SelectMany(pluginPath =>
{
    Assembly pluginAssembly = LoadPlugin(pluginPath);
    return CreateCommands(pluginAssembly);
}).ToList();
```

然后用以下代码片段替换 `// Output the loaded commands` 注释：

```csharp
foreach (ICommand command in commands)
{
    Console.WriteLine($"{command.Name}\t - {command.Description}");
}
```

使用以下代码片段替换 `// Execute the command with the name passed as an argument` 注释：

```csharp
ICommand command = commands.FirstOrDefault(c => c.Name == commandName);
if (command == null)
{
    Console.WriteLine("No such command is known.");
    return;
}

command.Execute();
```

最后，将静态方法添加到名为 `LoadPlugin` 和 `CreateCommands` 的 `Program` 类，如下所示：

```csharp
static Assembly LoadPlugin(string relativePath)
{
    throw new NotImplementedException();
}

static IEnumerable<ICommand> CreateCommands(Assembly assembly)
{
    int count = 0;

    foreach (Type type in assembly.GetTypes())
    {
        if (typeof(ICommand).IsAssignableFrom(type))
        {
            ICommand result = Activator.CreateInstance(type) as ICommand;
            if (result != null)
            {
                count++;
                yield return result;
            }
        }
    }

    if (count == 0)
    {
        string availableTypes = string.Join(",", assembly.GetTypes().Select(t => t.FullName));
        throw new ApplicationException(
            $"Can't find any type which implements ICommand in {assembly} from {assembly.Location}.\n" +
            $"Available types: {availableTypes}");
    }
}
```

## <a name="load-plugins"></a>加载插件

现在，应用程序可以正确加载和实例化来自已加载的插件程序集的命令，但仍然无法加载插件程序集。 使用以下内容在 AppWithPlugin 文件夹中创建名为 PluginLoadContext.cs 的文件：  

[!code-csharp[loading-plugins](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/AppWithPlugin/PluginLoadContext.cs)]

`PluginLoadContext` 类型派生自 <xref:System.Runtime.Loader.AssemblyLoadContext>。 `AssemblyLoadContext` 类型是运行时中的特殊类型，该类型允许开发人员将已加载的程序集隔离到不同的组中，以确保程序集版本不冲突。 此外，自定义 `AssemblyLoadContext` 可以选择不同路径来加载程序集格式并重写默认行为。 `PluginLoadContext` 使用 .NET Core 3.0 中引入的 `AssemblyDependencyResolver` 类型的实例将程序集名称解析为路径。 `AssemblyDependencyResolver` 对象是使用 .NET 类库的路径构造的。 它根据类库的 .deps.json 文件（其路径传递给 `AssemblyDependencyResolver` 构造函数）将程序集和本机库解析为它们的相对路径。 自定义 `AssemblyLoadContext` 使插件能够拥有自己的依赖项，`AssemblyDependencyResolver` 使正确加载依赖项变得容易。

由于 `AppWithPlugin` 项目具有 `PluginLoadContext` 类型，所以请使用以下正文更新 `Program.LoadPlugin` 方法：

```csharp
static Assembly LoadPlugin(string relativePath)
{
    // Navigate up to the solution root
    string root = Path.GetFullPath(Path.Combine(
        Path.GetDirectoryName(
            Path.GetDirectoryName(
                Path.GetDirectoryName(
                    Path.GetDirectoryName(
                        Path.GetDirectoryName(typeof(Program).Assembly.Location)))))));

    string pluginLocation = Path.GetFullPath(Path.Combine(root, relativePath.Replace('\\', Path.DirectorySeparatorChar)));
    Console.WriteLine($"Loading commands from: {pluginLocation}");
    PluginLoadContext loadContext = new PluginLoadContext(pluginLocation);
    return loadContext.LoadFromAssemblyName(new AssemblyName(Path.GetFileNameWithoutExtension(pluginLocation)));
}
```

通过为每个插件使用不同的 `PluginLoadContext` 实例，插件可以具有不同的甚至冲突的依赖项，而不会出现问题。

## <a name="simple-plugin-with-no-dependencies"></a>不具有依赖项的简单插件

返回到根文件夹，执行以下步骤：

1. 运行以下命令，新建一个名为 `HelloPlugin` 的类库项目：

    ```dotnetcli
    dotnet new classlib -o HelloPlugin
    ```

2. 运行以下命令，将项目添加到 `AppWithPlugin` 解决方案中：

    ```dotnetcli
    dotnet sln add HelloPlugin/HelloPlugin.csproj
    ```

3. 使用以下内容将 HelloPlugin/Class1.cs 文件替换为名为 HelloCommand.cs 的文件：  

[!code-csharp[the-hello-plugin](~/samples/snippets/core/tutorials/creating-app-with-plugin-support/csharp/HelloPlugin/HelloCommand.cs)]

现在，打开 HelloPlugin.csproj 文件  。 它应类似于以下内容：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net5</TargetFramework>
  </PropertyGroup>

</Project>

```

在 `<Project>` 标记之间添加以下元素：

```xml
<ItemGroup>
    <ProjectReference Include="..\PluginBase\PluginBase.csproj">
        <Private>false</Private>
        <ExcludeAssets>runtime</ExcludeAssets>
    </ProjectReference>
</ItemGroup>
```

`<Private>false</Private>` 元素很重要。 它告知 MSBuild 不要将 PluginBase.dll 复制到 HelloPlugin 的输出目录  。 如果 PluginBase.dll 程序集出现在输出目录中，`PluginLoadContext` 将在那里查找到该程序集并在加载 HelloPlugin.dll 程序集时加载它。 此时，`HelloPlugin.HelloCommand` 类型将从 `HelloPlugin` 项目的输出目录中的 PluginBase.dll 实现 `ICommand` 接口，而不是加载到默认加载上下文中的 `ICommand` 接口。 因为运行时将这两种类型视为不同程序集的不同类型，所以 `AppWithPlugin.Program.CreateCommands` 方法找不到命令。 因此，对包含插件接口的程序集的引用需要 `<Private>false</Private>` 元数据。

同样，如果 `PluginBase` 引用其他包，则 `<ExcludeAssets>runtime</ExcludeAssets>` 元素也很重要。 此设置与 `<Private>false</Private>` 的效果相同，但适用于 `PluginBase` 项目或它的某个依赖项可能包括的包引用。

因为 `HelloPlugin` 项目已完成，所以应该更新 `AppWithPlugin` 项目，以确认可以找到 `HelloPlugin` 插件的位置。 在 `// Paths to plugins to load` 注释后，添加 `@"HelloPlugin\bin\Debug\netcoreapp3.0\HelloPlugin.dll"`（根据所使用的 .NET Core 版本，此路径可能有所不同）作为 `pluginPaths` 数组的元素。

## <a name="plugin-with-library-dependencies"></a>具有库依赖项的插件

几乎所有插件都比简单的“Hello World”更复杂，而且许多插件都具有其他库上的依赖项。 示例中的 `JsonPlugin` 和 `OldJson` 插件项目显示了具有 `Newtonsoft.Json` 上的 NuGet 包依赖项的两个插件示例。 项目文件本身没有关于项目引用的任何特殊信息，并且（在将插件路径添加到 `pluginPaths` 数组之后）即使是在 AppWithPlugin 应用的同一执行中运行，插件也能完美运行。 但是，这些项目不会将引用的程序集复制到它们的输出目录中，因此需要将这些程序集显示在用户的计算机上，以便插件能够正常工作。 解决此问题有两种方法。 第一种是使用 `dotnet publish` 命令发布类库。 或者，如果希望能够将 `dotnet build` 的输出用于插件，可以在插件的项目文件中的 `<PropertyGroup>` 标记之间添加 `<CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>` 属性。 有关示例，请参阅 `XcopyablePlugin` 插件项目。

## <a name="other-examples-in-the-sample"></a>示例中的其他示例

可以在 [dotnet/samples 存储库](https://github.com/dotnet/samples/tree/main/core/extensions/AppWithPlugin)中找到本教程的完整源代码。 完成的示例包括 `AssemblyDependencyResolver` 行为的一些其他示例。 例如，`AssemblyDependencyResolver` 对象还可以解析本机库和 NuGet 包中所包含的已本地化的附属程序集。 示例存储库中的 `UVPlugin` 和 `FrenchPlugin` 演示了这些方案。

## <a name="reference-a-plugin-interface-from-a-nuget-package"></a>从 NuGet 包引用插件接口

假设存在应用 A，它具有 NuGet 包（名为 `A.PluginBase`）中定义的插件接口。 如何在插件项目中正确引用包？ 对于项目引用，使用项目文件的 `ProjectReference` 元素上的 `<Private>false</Private>` 元数据会阻止将 dll 复制到输出。

若要正确引用 `A.PluginBase` 包，应将项目文件中的 `<PackageReference>` 元素更改为以下内容：

```xml
<PackageReference Include="A.PluginBase" Version="1.0.0">
    <ExcludeAssets>runtime</ExcludeAssets>
</PackageReference>
```

此操作会阻止将 `A.PluginBase` 程序集复制到插件的输出目录，并确保插件将使用 A 版本的 `A.PluginBase`。

## <a name="plugin-target-framework-recommendations"></a>插件目标框架建议

因为插件依赖项加载使用 .deps.json 文件，所以存在一个与插件的目标框架相关的问题  。 具体来说，插件应该以运行时为目标，比如 .NET 5，而不是某一版本的 .NET Standard。 .deps.json  文件基于项目所针对的框架生成，而且由于许多与 .NET Standard 兼容的包提供了用于针对 .NET Standard 进行生成的引用程序集和用于特定运行时的实现程序集，因此 .deps.json  可能无法正确查看实现程序集，或者它可能会获取 .NET Standard 版本的程序集，而不是期望的 .NET Core 版本的程序集。

## <a name="plugin-framework-references"></a>插件框架引用

插件当前无法向该过程引入新的框架。 例如，无法将使用 `Microsoft.AspNetCore.App` 框架的插件加载到只使用根 `Microsoft.NETCore.App` 框架的应用程序中。 主机应用程序必须声明对插件所需的全部框架的引用。
