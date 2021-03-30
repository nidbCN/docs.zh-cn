---
title: 使用可为空引用类型进行设计
description: 本高级教程介绍了可为空引用类型。 你将学习在引用值可能为 NULL 时表达你的设计意图，并在引用值不能为 NULL 时让编译器强制执行。
ms.date: 02/19/2019
ms.technology: csharp-null-safety
ms.custom: mvc
ms.openlocfilehash: 01b05c6fa71c71546109cb25474b703458eb6e22
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878897"
---
# <a name="tutorial-express-your-design-intent-more-clearly-with-nullable-and-non-nullable-reference-types"></a>教程：使用可为空和不可为空引用类型更清晰地表达设计意图

C# 8.0 引入了[可为空引用类型](../../nullable-references.md)，它们以与可为空值类型补充值类型相同的方式补充引用类型。 通过将 `?` 追加到此类型，你可以将变量声明为  可为空引用类型。 例如，`string?` 表示可为空的 `string`。 可以使用这些新类型更清楚地表达你的设计意图：某些变量  必须始终具有值，其他变量可以缺少值  。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 将可为空和不可为空引用类型合并到你的设计中
> - 在整个代码中启用可为空引用类型检查。
> - 编写编译器强制执行这些设计决策的代码。
> - 在自己的设计中使用可为空引用功能

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet/3.0) 随附 C# 8.0 编译器。

本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。

## <a name="incorporate-nullable-reference-types-into-your-designs"></a>将可为空引用类型合并到你的设计中

在本教程中，你将构建一个模拟运行调查的库。 该代码使用可为空引用类型和不可为可空引用类型来表示实际概念。 调查问题决不会为 NULL。 回应者可能不愿回答某个问题。 在这种情况下响应可能为 `null`。

你为此示例编写的代码表示该意向，并且编译器强制执行该意向。

## <a name="create-the-application-and-enable-nullable-reference-types"></a>创建应用程序并启用可为空引用类型

在 Visual Studio 中或使用 `dotnet new console` 从命令行创建新的控制台应用程序。 命名应用程序 `NullableIntroduction`。 创建应用程序后，需要指定整个项目都在启用的“可为空注释上下文”中进行编译  。 打开 .csproj 文件，并向 `PropertyGroup` 元素添加 `Nullable` 元素  。 将其值设置为 `enable`。 必须选择“可为空引用类型”功能，即使在 C# 8.0 项目中也是如此  。 这是因为，一旦启用该功能，现有的引用变量声明将成为不可为空引用类型  。 尽管该决定将有助于发现现有代码可能不具有适当的 NULL 检查的问题，但它可能无法准确反映你的原始设计意图：

```xml
<Nullable>enable</Nullable>
```

### <a name="design-the-types-for-the-application"></a>设计应用程序的类型

此调查应用程序需要创建许多类：

- 建模问题列表的类。
- 建模为调查所联系的人员列表的类。
- 建模来自参加调查人员的答案的类。

这些类型将使用可为空和不可为空引用类型来表示哪些成员是必需的，哪些成员是可选的。 可为空引用类型清楚地传达了设计意图：

- 调查中的问题不可为 null：提出空问题没有任何意义。
- 回应者永远不能为 NULL。 你需要跟踪所联系的人员，即便回应者拒绝参与也是如此。
- 对某个问题的任何响应都可能为 NULL。 回应者可拒绝回答部分或全部问题。

如果你使用 C# 编程，则可能已经习惯于允许 `null` 值的引用类型，但你可能错过了其他声明不可为空实例的机会：

- 问题集合应不可为空。
- 回应者集合应不可为空。

在编写代码时，你将看到不可为空引用类型作为引用的默认值，可避免可能导致 <xref:System.NullReferenceException> 的常见错误。 从本教程得出的一个经验是，你应决定哪些变量可为或不可为 `null`。 该语言未提供表达这些决定的语法。 现在它可提供此项功能。

你构建的应用程序将执行以下步骤：

1. 创建调查并向其添加问题。
1. 为调查创建一组伪随机回应者。
1. 联系回应者，直到已完成的调查规模达到目标数量。
1. 写出有关调查响应的重要统计数据。

## <a name="build-the-survey-with-nullable-and-non-nullable-reference-types"></a>使用可为 null 和不可为 null 引用类型构建调查

你将编写的第一个代码创建调查。 你将编写类来为调查问题和调查运行建模。 调查有三种类型的问题，通过答案格式进行区分：答案为“是”/“否”、答案为数字以及答案为文本。 创建 `public SurveyQuestion` 类：

```csharp
namespace NullableIntroduction
{
    public class SurveyQuestion
    {
    }
}
```

编译器将在启用的可为空的注释上下文中的代码的每个引用类型变量声明解释为“不可为空”引用类型  。 你可以通过添加问题文本的属性和问题类型来查看第一个警告，如以下代码所示：

```csharp
namespace NullableIntroduction
{
    public enum QuestionType
    {
        YesNo,
        Number,
        Text
    }

    public class SurveyQuestion
    {
        public string QuestionText { get; }
        public QuestionType TypeOfQuestion { get; }
    }
}
```

因为尚未初始化 `QuestionText`，所以编译器会发出警告，指出尚未初始化不可为空属性。 设计要求问题文本为非空，因此需要添加构造函数来初始化它以及 `QuestionType` 值。 已完成类定义类似于以下代码：

[!code-csharp[DefineQuestion](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyQuestion.cs)]

添加构造函数会删除警告。 构造函数参数也是不可为空引用类型，因此编译器不会发出任何警告。

接下来，创建一个名为 `SurveyRun` 的 `public` 类。 此类包含 `SurveyQuestion` 对象的列表以及向调查添加问题的方法，如以下代码所示：

```csharp
using System.Collections.Generic;

namespace NullableIntroduction
{
    public class SurveyRun
    {
        private List<SurveyQuestion> surveyQuestions = new List<SurveyQuestion>();

        public void AddQuestion(QuestionType type, string question) =>
            AddQuestion(new SurveyQuestion(type, question));
        public void AddQuestion(SurveyQuestion surveyQuestion) => surveyQuestions.Add(surveyQuestion);
    }
}
```

和以前一样，你必须将列表对象初始化为非空值，否则编译器会发出警告。 在 `AddQuestion` 的第二次重载中没有 NULL 检查，因为不需要进行二次检查：已声明该变量不可为空。 其值不可为 `null`。

切换到编辑器中的 Program.cs  ，并使用以下代码行替换 `Main` 的内容：

[!code-csharp[AddQuestions](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#AddQuestions)]

由于整个项目处于启用的可为空的注释上下文中，因此将 `null` 传递给任何应为不可为空引用类型的方法时，将收到警告。 通过将以下行添加到 `Main` 进行尝试：

```csharp
surveyRun.AddQuestion(QuestionType.Text, default);
```

## <a name="create-respondents-and-get-answers-to-the-survey"></a>创建回应者并获取调查答案

接下来，编写生成调查答案的代码。 此过程涉及到多个小型任务：

1. 构建一个生成回应者对象的方法。 这些对象表示要求填写调查的人员。
1. 生成逻辑以模拟向回应者询问问题并收集答案，或者注意到回应者没有回答。
1. 重复以上过程，直到有足够的回应者回答此调查。

你需要一个表示调查响应的类，所以现在就添加它。 启用可为空支持。 添加初始化它的 `Id` 属性和构造函数，如以下代码所示：

```csharp
namespace NullableIntroduction
{
    public class SurveyResponse
    {
        public int Id { get; }

        public SurveyResponse(int id) => Id = id;
    }
}
```

接下来，通过生成随机 ID 添加 `static` 方法来创建新参与者：

[!code-csharp[GenerateRespondents](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#Random)]

该类的主要职责是为调查中问题的参与者生成问题答案。 实现此职责有几个步骤：

1. 要求参加这项调查。 如果此人不同意，则返回缺失（或 NULL）响应。
1. 询问每个问题并记录答案。 每个答案也可能会缺失（或 NULL）。

将以下代码添加到 `SurveyResponse` 类：

[!code-csharp[AnswerSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#AnswerSurvey)]

调查答案的存储空间为 `Dictionary<int, string>?`，表示它可能为 NULL。 你正在使用新的语言功能向编译器和稍后阅读你的代码的任何人声明你的设计意图。 如果在不首先检查是否为 `null` 值的情况下取消引用 `surveyResponses`，则会收到编译器警告。 你没有在 `AnswerSurvey` 方法中收到警告，因为编译器可以确定 `surveyResponses` 变量已设置为上述非空值。

对缺少的答案使用 `null` 强调了处理可为空引用类型的一个关键点：目标不是从程序中删除所有 `null` 值。 而是确保编写的代码表达设计意图。 缺失值是在代码中进行表达的一个必需概念。 `null` 值是表示这些缺失值的一种明确方法。 尝试删除所有 `null` 值只会导致定义一些其他方法来在没有 `null` 的情况下表示缺失值。

接下来，你需要在 `SurveyRun` 类中编写 `PerformSurvey` 方法。 将下面的代码添加到 `SurveyRun` 类中：

[!code-csharp[PerformSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#PerformSurvey)]

同样，你选择的可为空 `List<SurveyResponse>?` 指示响应可能为 NULL。 这表明尚未向任何回应者提供调查。 请注意，在同意参与调查的回应者未达到足够数量之前，不会添加回应者。

运行调查的最后一步是添加一个调用，从而可在 `Main` 方法结束时执行此调查：

[!code-csharp[RunSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#RunSurvey)]

## <a name="examine-survey-responses"></a>检查调查响应

最后一步是显示调查结果。 将代码添加到你所编写的诸多类。 此代码演示了区分可为空和不可为空引用类型的值。 首先将以下两个表达式形式成员添加到 `SurveyResponse` 类：

[!code-csharp[ReportResponses](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#SurveyStatus)]

因为 `surveyResponses` 是一个不可为空引用，所以在取消引用之前不需要输入任何检查。 `Answer` 方法返回不可为空的字符串，因此我们必须使用 null 合并运算符来涵盖缺少答案的情况。

接下来，将这三个表达式形式成员添加到 `SurveyRun` 类：

[!code-csharp[ReportResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#RunReport)]

`AllParticipants` 成员必须考虑 `respondents` 变量可能为 NULL 但返回值不能为 NULL 的情况。 如果通过删除后面的 `??` 和空序列来更改该表达式，则编译器会警告你方法可能返回 `null` 并且其返回签名返回不可为空类型。

最后，在 `Main` 方法的底部添加以下循环：

[!code-csharp[DisplaySurveyResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#WriteAnswers)]

你不需要在此代码中执行任何 `null` 检查，因为你已设计了基础接口，这样它们均返回不可为空引用类型。

## <a name="get-the-code"></a>获取代码

你可以从 [csharp/NullableIntroduction](https://github.com/dotnet/samples/tree/main/csharp/NullableIntroduction) 文件夹中的[示例](https://github.com/dotnet/samples)存储库获取已完成教程的代码。

通过更改可为空和不可为空引用类型之间的类型声明进行试验。 了解如何生成不同的警告以确保不会意外取消引用 `null`。

## <a name="next-steps"></a>后续步骤

要了解更多信息，请迁移现有应用程序以使用可为空引用类型：
> [!div class="nextstepaction"]
> [升级应用程序以使用可为空引用类型](upgrade-to-nullable-references.md)

了解如何在使用实体框架时使用可为空引用类型：
> [Entity Framework Core 基础知识：使用可为空引用类型](/ef/core/miscellaneous/nullable-reference-types)
