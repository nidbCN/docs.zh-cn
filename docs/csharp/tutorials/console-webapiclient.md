---
title: 使用 .NET Core 创建 REST 客户端
description: 此教程介绍 .NET Core 和 C# 语言的一些功能。
ms.date: 01/09/2020
ms.assetid: 51033ce2-7a53-4cdd-966d-9da15c8204d2
ms.openlocfilehash: 4d36cdafd232de9bbd0fac12e894f905b4808419
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876144"
---
# <a name="rest-client"></a>REST 客户端

此教程将介绍 .NET Core 和 C# 语言的许多功能。 你将了解：

* .NET Core CLI 的基础知识。
* C# 语言功能概述。
* 如何使用 NuGet 管理依赖项
* HTTP 通信
* 如何处理 JSON 信息
* 如何管理含特性的配置。

将生成一个应用程序，向 GitHub 上的 REST 服务发出 HTTP 请求。 需要读取 JSON 格式的信息，并将相应的 JSON 数据包转换成 C# 对象。 最后将了解如何使用 C# 对象。

此教程中介绍了多项功能。 我们将逐个生成这些功能。

如果想要按照用于本文的[最终示例](https://github.com/dotnet/samples/tree/main/csharp/getting-started/console-webapiclient)操作，可以下载它。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .Net Core。 有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。 可以在 Windows、Linux、macOS 或 Docker 容器中运行此应用程序。
需要安装常用的代码编辑器。 在以下说明中，我们使用的是开放源代码跨平台编辑器 [Visual Studio Code](https://code.visualstudio.com/)。 不过，你可以使用习惯使用的任意工具。

## <a name="create-the-application"></a>创建应用程序

第一步是新建应用程序。 打开命令提示符，然后新建应用程序的目录。 将新建的目录设为当前目录。 在控制台窗口中输入以下命令：

```dotnetcli
dotnet new console --name WebAPIClient
```

这将为基本的“Hello World”应用程序创建起始文件。 项目名称为“WebAPIClient”。 这是一个新项目，因此没有部署任何依赖项。 第一次运行时将下载 .NET Core 框架、安装开发证书并运行 NuGet 包管理器来还原缺少的依赖项。

开始修改之前，使用 `cd` 转到“WebAPIClient”目录，然后在命令提示符下键入 `dotnet run`（[参见注释](#dotnet-restore-note)）以运行应用程序。 如果环境缺少依赖项，则 `dotnet run` 会自动执行 `dotnet restore`。 如果需要重新生成应用程序，它还会执行 `dotnet build`。
初始设置完成后，只需在对项目有意义的情况下运行 `dotnet restore` 或 `dotnet build`。

## <a name="adding-new-dependencies"></a>添加新的依赖项

.NET Core 的主要设计目标之一是最小化 .NET 安装的大小。 如果应用程序需要使用其他库来生成某些功能，请将这些依赖项添加到 C# 项目 (\*.csproj) 文件中。 对于我们的示例，将需要添加 `System.Runtime.Serialization.Json` 包，以便应用程序可以处理 JSON 响应。

你将需要此应用程序的 `System.Runtime.Serialization.Json` 包。 通过运行以下 [.NET CLI](../../core/tools/dotnet-add-package.md) 命令，将其添加到项目：

```dotnetcli
dotnet add package System.Text.Json
```

## <a name="making-web-requests"></a>发出 Web 请求

现在，可以开始检索 Web 数据了。 在此应用程序中，需要读取 [GitHub API](https://developer.github.com/v3/) 返回的信息。 让我们在 [.NET Foundation](https://www.dotnetfoundation.org/) 的保护下读取项目信息。 先向 GitHub API 发出请求，以检索项目信息。 将使用终结点 <https://api.github.com/orgs/dotnet/repos>。 由于要检索这些项目的所有信息，因此将发出 HTTP GET 请求。
此外，浏览器也使用 HTTP GET 请求，以便你可以将相应的 URL 粘贴到浏览器，查看将要收到并处理的信息。

使用 <xref:System.Net.Http.HttpClient> 类发出 Web 请求。 与所有新式 .NET API 一样，<xref:System.Net.Http.HttpClient> 只支持长时间运行 API 的异步方法。
从异步方法入手。 将一边填充实现代码，一边生成应用程序功能。 首先，打开项目目录中的 `program.cs` 文件，然后向 `Program` 类添加以下方法：

```csharp
private static async Task ProcessRepositories()
{
}
```

需要在 `Main` 方法的最上面添加 `using` 指令，以便 C# 编译器能够识别 <xref:System.Threading.Tasks.Task> 类型：

```csharp
using System.Threading.Tasks;
```

如果此时生成项目，将会看到系统针对此方法生成的警告，因为其中不含任何 `await` 运算符，将以同步方式运行。 暂且忽略此警告；将在填充方法时添加 `await` 运算符。

接下来，更新 `Main` 方法以调用 `ProcessRepositories` 方法。 `ProcessRepositories` 方法会返回一个任务，不应在此任务完成前退出程序。 因此，必须更改 `Main` 的签名。 添加 `async` 修饰符，并将返回类型更改为 `Task`。 然后，在方法的主体中，添加对 `ProcessRepositories` 的调用。 向该方法调用添加 `await` 关键字：

```csharp
static async Task Main(string[] args)
{
    await ProcessRepositories();
}
```

现在，生成了一个不执行任何操作但具备异步功能的程序。 现在进行改进。

首先需要一个能从 Web 检索数据的对象；可使用 <xref:System.Net.Http.HttpClient> 执行此操作。 此对象负责处理请求和响应。 在 Program.cs 文件内的 `Program` 类中初始化该类型的一个实例。

```csharp
namespace WebAPIClient
{
    class Program
    {
        private static readonly HttpClient client = new HttpClient();

        static async Task Main(string[] args)
        {
            //...
        }
    }
}
```

让我们回到 `ProcessRepositories` 方法，并填充它的第一个版本：

```csharp
private static async Task ProcessRepositories()
{
    client.DefaultRequestHeaders.Accept.Clear();
    client.DefaultRequestHeaders.Accept.Add(
        new MediaTypeWithQualityHeaderValue("application/vnd.github.v3+json"));
    client.DefaultRequestHeaders.Add("User-Agent", ".NET Foundation Repository Reporter");

    var stringTask = client.GetStringAsync("https://api.github.com/orgs/dotnet/repos");

    var msg = await stringTask;
    Console.Write(msg);
}
```

此外，为了让编译能够顺利进行，还需要在文件的最上面添加两个新的 `using` 指令：

```csharp
using System.Net.Http;
using System.Net.Http.Headers;
```

第一个版本发出 Web 请求来读取 .NET Foundation 组织下的所有存储库列表。 （.NET Foundation 的 GitHub ID 为 `dotnet`。）前几行代码针对该请求设置 <xref:System.Net.Http.HttpClient>。 在第一行中，它被配置为接受 GitHub JSON 响应。
此格式仅为 JSON。 下一代码行将用户代理标头添加到此对象发出的所有请求中。 这两个标头均由 GitHub 服务器代码进行检查，必须使用它们，才能检索 GitHub 中的信息。

配置 <xref:System.Net.Http.HttpClient> 后，发出 Web 请求并检索响应。 在此第一个版本中，将使用 <xref:System.Net.Http.HttpClient.GetStringAsync(System.String)?displayProperty=nameWithType> 便捷方法。 此便捷方法先执行发出 Web 请求的任务，然后当返回请求时读取响应流，并从流中提取内容。 响应正文以 <xref:System.String> 的形式返回。 此字符串在任务完成时可用。

此方法的最后两行代码用于等待任务完成，然后在控制台中打印输出响应。
生成并运行应用程序。 此时，生成警告不再显示，因为 `ProcessRepositories` 现在的确包含 `await` 运算符。 将看到很长的 JSON 格式文本。

## <a name="processing-the-json-result"></a>处理 JSON 结果

此时，你已编写用于检索来自 Web 服务器的响应，并显示响应文本的代码。 接下来，让我们将相应的 JSON 响应转换成 C# 对象。

<xref:System.Text.Json.JsonSerializer?displayProperty=nameWithType> 类将对象序列化为 JSON，并将 JSON 反序列化为对象。 首先定义一个类，用于表示从 GitHub API 返回的 `repo` JSON 对象：

```csharp
using System;

namespace WebAPIClient
{
    public class Repository
    {
        public string name { get; set; }
    }
}
```

将以上代码添加到“repo.cs”新文件中。 此版本的类表示处理 JSON 数据的最简单路径。 类名和成员名称与 JSON 数据包中使用的名称一致，而不是遵循以下 C# 约定。 稍后将通过提供某些配置特性进行修复。 此类展示了 JSON 序列化和反序列化的另一个重要功能：并非 JSON 数据包中的所有字段都属于此类。
JSON 序列化程序将忽略所使用的类类型未包含的信息。
借助此功能，可更轻松地创建仅处理一部分 JSON 数据包字段的类型。

至此，你已创建类型，让我们来进行反序列化吧。

接下来，使用序列化程序将 JSON 转换成 C# 对象。 使用以下行替换 `ProcessRepositories` 方法中对 <xref:System.Net.Http.HttpClient.GetStringAsync(System.String)> 的调用：

```csharp
var streamTask = client.GetStreamAsync("https://api.github.com/orgs/dotnet/repos");
var repositories = await JsonSerializer.DeserializeAsync<List<Repository>>(await streamTask);
```

你使用的是新命名空间，因此，还需要将其添加到文件顶部：

```csharp
using System.Collections.Generic;
using System.Text.Json;
```

请注意，现使用 <xref:System.Net.Http.HttpClient.GetStreamAsync(System.String)>，而不是 <xref:System.Net.Http.HttpClient.GetStringAsync(System.String)>。 序列化程序使用流（而不是字符串）作为其源。 让我们来看看前面第二行代码段中所使用的多项 C# 语言功能。 <xref:System.Text.Json.JsonSerializer.DeserializeAsync%60%601(System.IO.Stream,System.Text.Json.JsonSerializerOptions,System.Threading.CancellationToken)?displayProperty=nameWithType> 的第一个自变量是 `await` 表达式。 （其他两个参数为可选，并在代码段中省略。）Await 表达式可以出现在代码中的几乎任何位置，尽管到目前为止，你只在赋值语句中看到过它们。 `Deserialize` 方法为泛型，这意味着必须为应从 JSON 文本创建的对象的类型提供类型参数。 在此示例中，你要反序列化到 `List<Repository>`，这是另一个泛型对象，即 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>。 `List<>` 类存储对象的集合。 类型参数声明存储在 `List<>` 中的对象的类型。 JSON 文本表示存储库对象的集合，因此类型参数为 `Repository`。

即将完成此部分的操作。 至此，你已将 JSON 数据转换成 C# 对象，让我们来显示每个存储库的名称。 将以下代码行：

```csharp
var msg = await stringTask;   //**Deleted this
Console.Write(msg);
```

替换为：

```csharp
foreach (var repo in repositories)
    Console.WriteLine(repo.name);
```

编译并运行该应用程序。 将打印属于 .NET Foundation 的存储库的名称。

## <a name="controlling-serialization"></a>控制序列化

在添加更多功能之前，让我们通过使用 `[JsonPropertyName]` 特性来处理 `name` 属性。 对 repo.cs 中的 `name` 字段声明执行以下更改：

```csharp
[JsonPropertyName("name")]
public string Name { get; set; }
```

若要使用 `[JsonPropertyName]` 属性，需要将 <xref:System.Text.Json.Serialization> 命名空间添加到 `using` 指令：

```csharp
using System.Text.Json.Serialization;
```

此更改意味着需要更改用于在 program.cs 中写入每个存储库名称的代码：

```csharp
Console.WriteLine(repo.Name);
```

执行 `dotnet run`，以确保生成正确映射。 应能看到与之前一样的输出。

让我们在添加新功能前执行另一项更改。 `ProcessRepositories` 方法可以执行异步工作，并返回一组存储库。 我们将使用此方法返回 `List<Repository>`，并将用于写入信息的代码移到 `Main` 方法中。

更改 `ProcessRepositories` 的签名，以返回可生成 `Repository` 对象列表的任务：

```csharp
private static async Task<List<Repository>> ProcessRepositories()
```

然后，在处理 JSON 响应后仅返回存储库：

```csharp
var streamTask = client.GetStreamAsync("https://api.github.com/orgs/dotnet/repos");
var repositories = await JsonSerializer.DeserializeAsync<List<Repository>>(await streamTask);
return repositories;
```

编译器将生成返回内容的 `Task<T>` 对象，因为你已将此方法标记为 `async`。
然后，我们将把 `Main` 方法修改为，捕获这些结果并将每个存储库名称写入控制台。 现在，`Main` 方法如下所示：

```csharp
public static async Task Main(string[] args)
{
    var repositories = await ProcessRepositories();

    foreach (var repo in repositories)
        Console.WriteLine(repo.Name);
}
```

## <a name="reading-more-information"></a>读取详细信息

最后，让我们来处理 GitHub API 发送的 JSON 数据包中的其他一些属性。 虽然你不想面面俱到，但添加其他一些属性将展示更多的 C# 语言功能。

首先，将其他一些简单类型添加到 `Repository` 类定义中。 将这些属性添加到此类：

```csharp
[JsonPropertyName("description")]
public string Description { get; set; }

[JsonPropertyName("html_url")]
public Uri GitHubHomeUrl { get; set; }

[JsonPropertyName("homepage")]
public Uri Homepage { get; set; }

[JsonPropertyName("watchers")]
public int Watchers { get; set; }
```

这些属性内置转换功能，可从字符串类型（即 JSON 数据包所含）转换成目标类型。 你可能是刚开始接触 <xref:System.Uri> 类型。 它代表 URI（或在此示例中，代表 URL）。 在有 `Uri` 和 `int` 类型的情况下，如果 JSON 数据包内的数据未转换成目标类型，那么序列化操作将会抛出异常。

添加这些元素后，将 `Main` 方法更新为显示它们：

```csharp
foreach (var repo in repositories)
{
    Console.WriteLine(repo.Name);
    Console.WriteLine(repo.Description);
    Console.WriteLine(repo.GitHubHomeUrl);
    Console.WriteLine(repo.Homepage);
    Console.WriteLine(repo.Watchers);
    Console.WriteLine();
}
```

最后一步，让我们添加最后一次推送操作的信息。 此信息按如下方式在 JSON 响应中进行格式化：

```json
2016-02-08T21:27:00Z
```

该格式为协调世界时 (UTC)，因此，你将获得一个 <xref:System.DateTime> 值，该值的 <xref:System.DateTime.Kind%2A> 属性为 <xref:System.DateTimeKind.Utc>。 如果你倾向于以时区表示的日期，则需要编写自定义转换方法。 首先，定义一个 `public` 属性，该属性将保存 `Repository` 类中日期和时间的 UTC 表示形式，并定义一个 `LastPush` `readonly` 属性，该属性返回转换为本地时间的日期：

```csharp
[JsonPropertyName("pushed_at")]
public DateTime LastPushUtc { get; set; }

public DateTime LastPush => LastPushUtc.ToLocalTime();
```

让我们来看一下刚定义的新构造。 `LastPush` 属性使用 `get` 访问器的 expression-bodied member 进行定义。 不存在 `set` 访问器。 省略 `set` 访问器就是在 C# 中定义只读属性的方式。 （是的，可以在 C# 中创建 *只写* 属性，但属性值受限。）

最后，在控制台中再添加一个输出语句，然后就可以再次生成并运行此应用程序：

```csharp
Console.WriteLine(repo.LastPush);
```

你的版本现在应与[已完成的示例](https://github.com/dotnet/samples/tree/main/csharp/getting-started/console-webapiclient)匹配。

## <a name="conclusion"></a>结束语

此教程介绍了如何发出 Web 请求、分析结果，以及如何显示这些结果的属性。 你也已经在项目中将新的包添加为依赖项， 并已了解一些支持面向对象的技术的 C# 语言功能。

<a name="dotnet-restore-note"></a>

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
