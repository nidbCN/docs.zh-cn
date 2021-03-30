---
title: 演练：使用 C# 保留对象
description: 本示例使用 C# 创建一个基本 Loan 对象，并将其数据保存到文件中，然后使用该文件中的数据创建一个新的对象。
ms.date: 04/26/2018
ms.openlocfilehash: 0fc733982b7800653a3c8c283bd0af8384d72168
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876443"
---
# <a name="walkthrough-persisting-an-object-using-c"></a>演练：使用 C\# 保留对象

可使用序列化在实例之间保持对象的数据，以便可存储值并在下次实例化对象时检索这些值。

本演练将创建一个基础的 `Loan` 对象，并将其值保留在文件中。 当重新创建该对象时，将检索文件中的数据。

> [!IMPORTANT]
> 如果此文件尚不存在，本示例将新建文件。 如果应用程序必须创建文件，则该应用程序必须对文件夹具有 `Create` 权限。 可使用访问控制列表设置权限。 如果文件已存在，则该应用程序只需要 `Write` 权限（这是较弱的权限）。 如有可能，较安全的做法是在部署过程中创建文件并仅向单个文件授予 `Read` 权限（而不是授予文件夹的“创建”权限）。 此外，较安全的做法是将数据写入用户文件夹，而不是根文件夹或 Program Files 文件夹。

> [!IMPORTANT]
> 此示例将数据存储为二进制格式的文件。 不应将这些格式用于敏感数据，如密码或信用卡信息。

## <a name="prerequisites"></a>先决条件

- 若要生成并运行，请安装 [.NET Core SDK](https://dotnet.microsoft.com/download)。

- 安装常用的代码编辑器（如果尚未安装）。

> [!TIP]
> 需要安装代码编辑器？ 试用 [Visual Studio](https://visualstudio.com/downloads)！

- 该示例需要 C# 7.3。 请参阅[选择 C# 语言版本](../../../language-reference/configure-language-version.md)

可在 [.NET 示例 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/serialization)在线检查示例代码。

## <a name="creating-the-loan-object"></a>创建 Loan 对象

第一步是创建 `Loan` 类和使用该类的控制台应用程序：

1. 创建一个新的应用程序。 键入 `dotnet new console -o serialization`，在名为 `serialization` 的子目录下创建新的控制台应用程序。
1. 在编辑器中打开应用程序，然后添加名为 `Loan.cs` 的新类。
1. 将以下代码添加到 `Loan` 类：

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#1)]

还需要创建一个使用 `Loan` 类的应用程序。

## <a name="serialize-the-loan-object"></a>串行化 Loan 对象

1. 打开 `Program.cs`。 添加以下代码：

[!code-csharp[Create a loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#1)]

添加 `PropertyChanged` 事件的事件处理程序和几行以修改 `Loan` 对象并显示此更改。 你可以在下列代码中查看添加项：

[!code-csharp[Listening for the PropertyChanged event](../../../../../samples/snippets/csharp/serialization/Program.cs#2)]

现在，可运行该代码，并查看当前输出：

```console
New customer value: Henry Clay
7.5
7.1
```

重复运行此应用程序始终编写相同值。 每次运行该程序都会创建新的 Loan 对象。 在现实生活中，利率会定期更改，但不必在每次运行应用程序时都更改利率。 序列化代码表示在应用程序的实例之间保存最新的利率。 下一步是通过向 Loan 类添加序列化来执行此操作。

## <a name="using-serialization-to-persist-the-object"></a>使用序列化保持对象

为了保持 Loan 类的值，必须首先使用 `Serializable` 属性标记该类。 将下面的代码添加到 Loan 类声明的上方：

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#2)]

<xref:System.SerializableAttribute> 通知编译器可将类中的所有内容保留到文件中。 因为 `PropertyChanged` 事件不表示应该存储的对象图的部分，所以它不应序列化。 执行此操作可能将所有附加到该事件的对象序列化。 可将 <xref:System.NonSerializedAttribute> 添加到 `PropertyChanged` 事件处理程序的字段声明。

[!code-csharp[Disable serialization for the event handler](../../../../../samples/snippets/csharp/serialization/Loan.cs#3)]

从 C# 7.3 开始，可使用 `field` 目标值将特性附加到自动实现的属性的支持字段。 以下代码添加 `TimeLastLoaded` 属性并将其标记为不可序列化：

[!code-csharp[Disable serialization for an auto-implemented property](../../../../../samples/snippets/csharp/serialization/Loan.cs#4)]

下一步是向 LoanApp 应用程序添加序列化代码。 为了将该类序列化并将其写入到文件，可使用 <xref:System.IO> 和 <xref:System.Runtime.Serialization.Formatters.Binary> 命名空间。 为了避免键入完全限定的名称，可以添加对必要命名空间的引用，如以下代码所示：

[!code-csharp[Adding namespaces for serialization](../../../../../samples/snippets/csharp/serialization/Program.cs#3)]

下一步是添加代码，在创建对象时对文件中的对象进行反序列化。 向序列化数据的文件名的类中添加一个常量，如以下代码所示：

[!code-csharp[Define the name of the saved file](../../../../../samples/snippets/csharp/serialization/Program.cs#4)]

接下来，在创建 `TestLoan` 对象的行后添加以下代码：

[!code-csharp[Read from a file if it exists](../../../../../samples/snippets/csharp/serialization/Program.cs#5)]

首先必须检查该文件是否存在。 如果存在，则创建 <xref:System.IO.Stream> 类来读取二进制文件和 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 类，以转换该文件。 还需将流类型转换为 Loan 对象类型。

然后必须添加代码以将该类序列化到文件中。 以 `Main` 方式在现有代码后添加以下代码：

[!code-csharp[Save the existing Loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#6)]

此时可再次生成并运行应用程序。 首次运行时，请注意起始利率为 7.5，然后更改为 7.1. 关闭该应用程序，然后重新运行。 现在，应用程序打印已读取所保存文件的消息，即使在代码更改它之前，利率也是 7.1。

## <a name="see-also"></a>请参阅

- [序列化 (C#)](index.md)
- [C# 编程指南](../../index.md)
