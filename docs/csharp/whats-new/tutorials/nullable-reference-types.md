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
# <a name="tutorial-express-your-design-intent-more-clearly-with-nullable-and-non-nullable-reference-types"></a><span data-ttu-id="be2a8-104">教程：使用可为空和不可为空引用类型更清晰地表达设计意图</span><span class="sxs-lookup"><span data-stu-id="be2a8-104">Tutorial: Express your design intent more clearly with nullable and non-nullable reference types</span></span>

<span data-ttu-id="be2a8-105">C# 8.0 引入了[可为空引用类型](../../nullable-references.md)，它们以与可为空值类型补充值类型相同的方式补充引用类型。</span><span class="sxs-lookup"><span data-stu-id="be2a8-105">C# 8.0 introduces [nullable reference types](../../nullable-references.md), which complement reference types the same way nullable value types complement value types.</span></span> <span data-ttu-id="be2a8-106">通过将 `?` 追加到此类型，你可以将变量声明为  可为空引用类型。</span><span class="sxs-lookup"><span data-stu-id="be2a8-106">You declare a variable to be a **nullable reference type** by appending a `?` to the type.</span></span> <span data-ttu-id="be2a8-107">例如，`string?` 表示可为空的 `string`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-107">For example, `string?` represents a nullable `string`.</span></span> <span data-ttu-id="be2a8-108">可以使用这些新类型更清楚地表达你的设计意图：某些变量  必须始终具有值，其他变量可以缺少值  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-108">You can use these new types to more clearly express your design intent: some variables *must always have a value*, others *may be missing a value*.</span></span>

<span data-ttu-id="be2a8-109">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="be2a8-109">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="be2a8-110">将可为空和不可为空引用类型合并到你的设计中</span><span class="sxs-lookup"><span data-stu-id="be2a8-110">Incorporate nullable and non-nullable reference types into your designs</span></span>
> - <span data-ttu-id="be2a8-111">在整个代码中启用可为空引用类型检查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-111">Enable nullable reference type checks throughout your code.</span></span>
> - <span data-ttu-id="be2a8-112">编写编译器强制执行这些设计决策的代码。</span><span class="sxs-lookup"><span data-stu-id="be2a8-112">Write code where the compiler enforces those design decisions.</span></span>
> - <span data-ttu-id="be2a8-113">在自己的设计中使用可为空引用功能</span><span class="sxs-lookup"><span data-stu-id="be2a8-113">Use the nullable reference feature in your own designs</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be2a8-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="be2a8-114">Prerequisites</span></span>

<span data-ttu-id="be2a8-115">需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。</span><span class="sxs-lookup"><span data-stu-id="be2a8-115">You'll need to set up your machine to run .NET Core, including the C# 8.0 compiler.</span></span> <span data-ttu-id="be2a8-116">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet/3.0) 随附 C# 8.0 编译器。</span><span class="sxs-lookup"><span data-stu-id="be2a8-116">The C# 8.0 compiler is available with [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), or [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet/3.0).</span></span>

<span data-ttu-id="be2a8-117">本教程假设你熟悉 C# 和 .NET，包括 Visual Studio 或 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="be2a8-117">This tutorial assumes you're familiar with C# and .NET, including either Visual Studio or the .NET Core CLI.</span></span>

## <a name="incorporate-nullable-reference-types-into-your-designs"></a><span data-ttu-id="be2a8-118">将可为空引用类型合并到你的设计中</span><span class="sxs-lookup"><span data-stu-id="be2a8-118">Incorporate nullable reference types into your designs</span></span>

<span data-ttu-id="be2a8-119">在本教程中，你将构建一个模拟运行调查的库。</span><span class="sxs-lookup"><span data-stu-id="be2a8-119">In this tutorial, you'll build a library that models running a survey.</span></span> <span data-ttu-id="be2a8-120">该代码使用可为空引用类型和不可为可空引用类型来表示实际概念。</span><span class="sxs-lookup"><span data-stu-id="be2a8-120">The code uses both nullable reference types and non-nullable reference types to represent the real-world concepts.</span></span> <span data-ttu-id="be2a8-121">调查问题决不会为 NULL。</span><span class="sxs-lookup"><span data-stu-id="be2a8-121">The survey questions can never be null.</span></span> <span data-ttu-id="be2a8-122">回应者可能不愿回答某个问题。</span><span class="sxs-lookup"><span data-stu-id="be2a8-122">A respondent might prefer not to answer a question.</span></span> <span data-ttu-id="be2a8-123">在这种情况下响应可能为 `null`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-123">The responses might be `null` in this case.</span></span>

<span data-ttu-id="be2a8-124">你为此示例编写的代码表示该意向，并且编译器强制执行该意向。</span><span class="sxs-lookup"><span data-stu-id="be2a8-124">The code you'll write for this sample expresses that intent, and the compiler enforces that intent.</span></span>

## <a name="create-the-application-and-enable-nullable-reference-types"></a><span data-ttu-id="be2a8-125">创建应用程序并启用可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="be2a8-125">Create the application and enable nullable reference types</span></span>

<span data-ttu-id="be2a8-126">在 Visual Studio 中或使用 `dotnet new console` 从命令行创建新的控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="be2a8-126">Create a new console application either in Visual Studio or from the command line using `dotnet new console`.</span></span> <span data-ttu-id="be2a8-127">命名应用程序 `NullableIntroduction`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-127">Name the application `NullableIntroduction`.</span></span> <span data-ttu-id="be2a8-128">创建应用程序后，需要指定整个项目都在启用的“可为空注释上下文”中进行编译  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-128">Once you've created the application, you'll need to specify that the entire project compiles in an enabled **nullable annotation context**.</span></span> <span data-ttu-id="be2a8-129">打开 .csproj 文件，并向 `PropertyGroup` 元素添加 `Nullable` 元素  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-129">Open the *.csproj* file and add a `Nullable` element to the `PropertyGroup` element.</span></span> <span data-ttu-id="be2a8-130">将其值设置为 `enable`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-130">Set its value to `enable`.</span></span> <span data-ttu-id="be2a8-131">必须选择“可为空引用类型”功能，即使在 C# 8.0 项目中也是如此  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-131">You must opt into the **nullable reference types** feature, even in C# 8.0 projects.</span></span> <span data-ttu-id="be2a8-132">这是因为，一旦启用该功能，现有的引用变量声明将成为不可为空引用类型  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-132">That's because once the feature is turned on, existing reference variable declarations become **non-nullable reference types**.</span></span> <span data-ttu-id="be2a8-133">尽管该决定将有助于发现现有代码可能不具有适当的 NULL 检查的问题，但它可能无法准确反映你的原始设计意图：</span><span class="sxs-lookup"><span data-stu-id="be2a8-133">While that decision will help find issues where existing code may not have proper null-checks, it may not accurately reflect your original design intent:</span></span>

```xml
<Nullable>enable</Nullable>
```

### <a name="design-the-types-for-the-application"></a><span data-ttu-id="be2a8-134">设计应用程序的类型</span><span class="sxs-lookup"><span data-stu-id="be2a8-134">Design the types for the application</span></span>

<span data-ttu-id="be2a8-135">此调查应用程序需要创建许多类：</span><span class="sxs-lookup"><span data-stu-id="be2a8-135">This survey application requires creating a number of classes:</span></span>

- <span data-ttu-id="be2a8-136">建模问题列表的类。</span><span class="sxs-lookup"><span data-stu-id="be2a8-136">A class that models the list of questions.</span></span>
- <span data-ttu-id="be2a8-137">建模为调查所联系的人员列表的类。</span><span class="sxs-lookup"><span data-stu-id="be2a8-137">A class that models a list of people contacted for the survey.</span></span>
- <span data-ttu-id="be2a8-138">建模来自参加调查人员的答案的类。</span><span class="sxs-lookup"><span data-stu-id="be2a8-138">A class that models the answers from a person that took the survey.</span></span>

<span data-ttu-id="be2a8-139">这些类型将使用可为空和不可为空引用类型来表示哪些成员是必需的，哪些成员是可选的。</span><span class="sxs-lookup"><span data-stu-id="be2a8-139">These types will make use of both nullable and non-nullable reference types to express which members are required and which members are optional.</span></span> <span data-ttu-id="be2a8-140">可为空引用类型清楚地传达了设计意图：</span><span class="sxs-lookup"><span data-stu-id="be2a8-140">Nullable reference types communicate that design intent clearly:</span></span>

- <span data-ttu-id="be2a8-141">调查中的问题不可为 null：提出空问题没有任何意义。</span><span class="sxs-lookup"><span data-stu-id="be2a8-141">The questions that are part of the survey can never be null: It makes no sense to ask an empty question.</span></span>
- <span data-ttu-id="be2a8-142">回应者永远不能为 NULL。</span><span class="sxs-lookup"><span data-stu-id="be2a8-142">The respondents can never be null.</span></span> <span data-ttu-id="be2a8-143">你需要跟踪所联系的人员，即便回应者拒绝参与也是如此。</span><span class="sxs-lookup"><span data-stu-id="be2a8-143">You'll want to track people you contacted, even respondents that declined to participate.</span></span>
- <span data-ttu-id="be2a8-144">对某个问题的任何响应都可能为 NULL。</span><span class="sxs-lookup"><span data-stu-id="be2a8-144">Any response to a question may be null.</span></span> <span data-ttu-id="be2a8-145">回应者可拒绝回答部分或全部问题。</span><span class="sxs-lookup"><span data-stu-id="be2a8-145">Respondents can decline to answer some or all questions.</span></span>

<span data-ttu-id="be2a8-146">如果你使用 C# 编程，则可能已经习惯于允许 `null` 值的引用类型，但你可能错过了其他声明不可为空实例的机会：</span><span class="sxs-lookup"><span data-stu-id="be2a8-146">If you've programmed in C#, you may be so accustomed to reference types that allow `null` values that you may have missed other opportunities to declare non-nullable instances:</span></span>

- <span data-ttu-id="be2a8-147">问题集合应不可为空。</span><span class="sxs-lookup"><span data-stu-id="be2a8-147">The collection of questions should be non-nullable.</span></span>
- <span data-ttu-id="be2a8-148">回应者集合应不可为空。</span><span class="sxs-lookup"><span data-stu-id="be2a8-148">The collection of respondents should be non-nullable.</span></span>

<span data-ttu-id="be2a8-149">在编写代码时，你将看到不可为空引用类型作为引用的默认值，可避免可能导致 <xref:System.NullReferenceException> 的常见错误。</span><span class="sxs-lookup"><span data-stu-id="be2a8-149">As you write the code, you'll see that a non-nullable reference type as the default for references avoids common mistakes that could lead to <xref:System.NullReferenceException>s.</span></span> <span data-ttu-id="be2a8-150">从本教程得出的一个经验是，你应决定哪些变量可为或不可为 `null`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-150">One lesson from this tutorial is that you made decisions about which variables could or could not be `null`.</span></span> <span data-ttu-id="be2a8-151">该语言未提供表达这些决定的语法。</span><span class="sxs-lookup"><span data-stu-id="be2a8-151">The language didn't provide syntax to express those decisions.</span></span> <span data-ttu-id="be2a8-152">现在它可提供此项功能。</span><span class="sxs-lookup"><span data-stu-id="be2a8-152">Now it does.</span></span>

<span data-ttu-id="be2a8-153">你构建的应用程序将执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="be2a8-153">The app you'll build does the following steps:</span></span>

1. <span data-ttu-id="be2a8-154">创建调查并向其添加问题。</span><span class="sxs-lookup"><span data-stu-id="be2a8-154">Creates a survey and adds questions to it.</span></span>
1. <span data-ttu-id="be2a8-155">为调查创建一组伪随机回应者。</span><span class="sxs-lookup"><span data-stu-id="be2a8-155">Creates a pseudo-random set of respondents for the survey.</span></span>
1. <span data-ttu-id="be2a8-156">联系回应者，直到已完成的调查规模达到目标数量。</span><span class="sxs-lookup"><span data-stu-id="be2a8-156">Contacts respondents until the completed survey size reaches the goal number.</span></span>
1. <span data-ttu-id="be2a8-157">写出有关调查响应的重要统计数据。</span><span class="sxs-lookup"><span data-stu-id="be2a8-157">Writes out important statistics on the survey responses.</span></span>

## <a name="build-the-survey-with-nullable-and-non-nullable-reference-types"></a><span data-ttu-id="be2a8-158">使用可为 null 和不可为 null 引用类型构建调查</span><span class="sxs-lookup"><span data-stu-id="be2a8-158">Build the survey with nullable and non-nullable reference types</span></span>

<span data-ttu-id="be2a8-159">你将编写的第一个代码创建调查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-159">The first code you'll write creates the survey.</span></span> <span data-ttu-id="be2a8-160">你将编写类来为调查问题和调查运行建模。</span><span class="sxs-lookup"><span data-stu-id="be2a8-160">You'll write classes to model a survey question and a survey run.</span></span> <span data-ttu-id="be2a8-161">调查有三种类型的问题，通过答案格式进行区分：答案为“是”/“否”、答案为数字以及答案为文本。</span><span class="sxs-lookup"><span data-stu-id="be2a8-161">Your survey has three types of questions, distinguished by the format of the answer: Yes/No answers, number answers, and text answers.</span></span> <span data-ttu-id="be2a8-162">创建 `public SurveyQuestion` 类：</span><span class="sxs-lookup"><span data-stu-id="be2a8-162">Create a `public SurveyQuestion` class:</span></span>

```csharp
namespace NullableIntroduction
{
    public class SurveyQuestion
    {
    }
}
```

<span data-ttu-id="be2a8-163">编译器将在启用的可为空的注释上下文中的代码的每个引用类型变量声明解释为“不可为空”引用类型  。</span><span class="sxs-lookup"><span data-stu-id="be2a8-163">The compiler interprets every reference type variable declaration as a **non-nullable** reference type for code in an enabled nullable annotation context.</span></span> <span data-ttu-id="be2a8-164">你可以通过添加问题文本的属性和问题类型来查看第一个警告，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="be2a8-164">You can see your first warning by adding properties for the question text and the type of question, as shown in the following code:</span></span>

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

<span data-ttu-id="be2a8-165">因为尚未初始化 `QuestionText`，所以编译器会发出警告，指出尚未初始化不可为空属性。</span><span class="sxs-lookup"><span data-stu-id="be2a8-165">Because you haven't initialized `QuestionText`, the compiler issues a warning that a non-nullable property hasn't been initialized.</span></span> <span data-ttu-id="be2a8-166">设计要求问题文本为非空，因此需要添加构造函数来初始化它以及 `QuestionType` 值。</span><span class="sxs-lookup"><span data-stu-id="be2a8-166">Your design requires the question text to be non-null, so you add a constructor to initialize it and the `QuestionType` value as well.</span></span> <span data-ttu-id="be2a8-167">已完成类定义类似于以下代码：</span><span class="sxs-lookup"><span data-stu-id="be2a8-167">The finished class definition looks like the following code:</span></span>

[!code-csharp[DefineQuestion](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyQuestion.cs)]

<span data-ttu-id="be2a8-168">添加构造函数会删除警告。</span><span class="sxs-lookup"><span data-stu-id="be2a8-168">Adding the constructor removes the warning.</span></span> <span data-ttu-id="be2a8-169">构造函数参数也是不可为空引用类型，因此编译器不会发出任何警告。</span><span class="sxs-lookup"><span data-stu-id="be2a8-169">The constructor argument is also a non-nullable reference type, so the compiler doesn't issue any warnings.</span></span>

<span data-ttu-id="be2a8-170">接下来，创建一个名为 `SurveyRun` 的 `public` 类。</span><span class="sxs-lookup"><span data-stu-id="be2a8-170">Next, create a `public` class named `SurveyRun`.</span></span> <span data-ttu-id="be2a8-171">此类包含 `SurveyQuestion` 对象的列表以及向调查添加问题的方法，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="be2a8-171">This class contains a list of `SurveyQuestion` objects and methods to add questions to the survey, as shown in the following code:</span></span>

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

<span data-ttu-id="be2a8-172">和以前一样，你必须将列表对象初始化为非空值，否则编译器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="be2a8-172">As before, you must initialize the list object to a non-null value or the compiler issues a warning.</span></span> <span data-ttu-id="be2a8-173">在 `AddQuestion` 的第二次重载中没有 NULL 检查，因为不需要进行二次检查：已声明该变量不可为空。</span><span class="sxs-lookup"><span data-stu-id="be2a8-173">There are no null checks in the second overload of `AddQuestion` because they aren't needed: You've declared that variable to be non-nullable.</span></span> <span data-ttu-id="be2a8-174">其值不可为 `null`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-174">Its value can't be `null`.</span></span>

<span data-ttu-id="be2a8-175">切换到编辑器中的 Program.cs  ，并使用以下代码行替换 `Main` 的内容：</span><span class="sxs-lookup"><span data-stu-id="be2a8-175">Switch to *Program.cs* in your editor and replace the contents of `Main` with the following lines of code:</span></span>

[!code-csharp[AddQuestions](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#AddQuestions)]

<span data-ttu-id="be2a8-176">由于整个项目处于启用的可为空的注释上下文中，因此将 `null` 传递给任何应为不可为空引用类型的方法时，将收到警告。</span><span class="sxs-lookup"><span data-stu-id="be2a8-176">Because the entire project is in an enabled nullable annotation context, you'll get warnings when you pass `null` to any method expecting a non-nullable reference type.</span></span> <span data-ttu-id="be2a8-177">通过将以下行添加到 `Main` 进行尝试：</span><span class="sxs-lookup"><span data-stu-id="be2a8-177">Try it by adding the following line to `Main`:</span></span>

```csharp
surveyRun.AddQuestion(QuestionType.Text, default);
```

## <a name="create-respondents-and-get-answers-to-the-survey"></a><span data-ttu-id="be2a8-178">创建回应者并获取调查答案</span><span class="sxs-lookup"><span data-stu-id="be2a8-178">Create respondents and get answers to the survey</span></span>

<span data-ttu-id="be2a8-179">接下来，编写生成调查答案的代码。</span><span class="sxs-lookup"><span data-stu-id="be2a8-179">Next, write the code that generates answers to the survey.</span></span> <span data-ttu-id="be2a8-180">此过程涉及到多个小型任务：</span><span class="sxs-lookup"><span data-stu-id="be2a8-180">This process involves several small tasks:</span></span>

1. <span data-ttu-id="be2a8-181">构建一个生成回应者对象的方法。</span><span class="sxs-lookup"><span data-stu-id="be2a8-181">Build a method that generates respondent objects.</span></span> <span data-ttu-id="be2a8-182">这些对象表示要求填写调查的人员。</span><span class="sxs-lookup"><span data-stu-id="be2a8-182">These represent people asked to fill out the survey.</span></span>
1. <span data-ttu-id="be2a8-183">生成逻辑以模拟向回应者询问问题并收集答案，或者注意到回应者没有回答。</span><span class="sxs-lookup"><span data-stu-id="be2a8-183">Build logic to simulate asking the questions to a respondent and collecting answers or noting that a respondent didn't answer.</span></span>
1. <span data-ttu-id="be2a8-184">重复以上过程，直到有足够的回应者回答此调查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-184">Repeat until enough respondents have answered the survey.</span></span>

<span data-ttu-id="be2a8-185">你需要一个表示调查响应的类，所以现在就添加它。</span><span class="sxs-lookup"><span data-stu-id="be2a8-185">You'll need a class to represent a survey response, so add that now.</span></span> <span data-ttu-id="be2a8-186">启用可为空支持。</span><span class="sxs-lookup"><span data-stu-id="be2a8-186">Enable nullable support.</span></span> <span data-ttu-id="be2a8-187">添加初始化它的 `Id` 属性和构造函数，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="be2a8-187">Add an `Id` property and a constructor that initializes it, as shown in the following code:</span></span>

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

<span data-ttu-id="be2a8-188">接下来，通过生成随机 ID 添加 `static` 方法来创建新参与者：</span><span class="sxs-lookup"><span data-stu-id="be2a8-188">Next, add a `static` method to create new participants by generating a random ID:</span></span>

[!code-csharp[GenerateRespondents](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#Random)]

<span data-ttu-id="be2a8-189">该类的主要职责是为调查中问题的参与者生成问题答案。</span><span class="sxs-lookup"><span data-stu-id="be2a8-189">The main responsibility of this class is to generate the responses for a participant to the questions in the survey.</span></span> <span data-ttu-id="be2a8-190">实现此职责有几个步骤：</span><span class="sxs-lookup"><span data-stu-id="be2a8-190">This responsibility has a few steps:</span></span>

1. <span data-ttu-id="be2a8-191">要求参加这项调查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-191">Ask for participation in the survey.</span></span> <span data-ttu-id="be2a8-192">如果此人不同意，则返回缺失（或 NULL）响应。</span><span class="sxs-lookup"><span data-stu-id="be2a8-192">If the person doesn't consent, return a missing (or null) response.</span></span>
1. <span data-ttu-id="be2a8-193">询问每个问题并记录答案。</span><span class="sxs-lookup"><span data-stu-id="be2a8-193">Ask each question and record the answer.</span></span> <span data-ttu-id="be2a8-194">每个答案也可能会缺失（或 NULL）。</span><span class="sxs-lookup"><span data-stu-id="be2a8-194">Each answer may also be missing (or null).</span></span>

<span data-ttu-id="be2a8-195">将以下代码添加到 `SurveyResponse` 类：</span><span class="sxs-lookup"><span data-stu-id="be2a8-195">Add the following code to your `SurveyResponse` class:</span></span>

[!code-csharp[AnswerSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#AnswerSurvey)]

<span data-ttu-id="be2a8-196">调查答案的存储空间为 `Dictionary<int, string>?`，表示它可能为 NULL。</span><span class="sxs-lookup"><span data-stu-id="be2a8-196">The storage for the survey answers is a `Dictionary<int, string>?`, indicating that it may be null.</span></span> <span data-ttu-id="be2a8-197">你正在使用新的语言功能向编译器和稍后阅读你的代码的任何人声明你的设计意图。</span><span class="sxs-lookup"><span data-stu-id="be2a8-197">You're using the new language feature to declare your design intent, both to the compiler and to anyone reading your code later.</span></span> <span data-ttu-id="be2a8-198">如果在不首先检查是否为 `null` 值的情况下取消引用 `surveyResponses`，则会收到编译器警告。</span><span class="sxs-lookup"><span data-stu-id="be2a8-198">If you ever dereference `surveyResponses` without checking for the `null` value first, you'll get a compiler warning.</span></span> <span data-ttu-id="be2a8-199">你没有在 `AnswerSurvey` 方法中收到警告，因为编译器可以确定 `surveyResponses` 变量已设置为上述非空值。</span><span class="sxs-lookup"><span data-stu-id="be2a8-199">You don't get a warning in the `AnswerSurvey` method because the compiler can determine the `surveyResponses` variable was set to a non-null value above.</span></span>

<span data-ttu-id="be2a8-200">对缺少的答案使用 `null` 强调了处理可为空引用类型的一个关键点：目标不是从程序中删除所有 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="be2a8-200">Using `null` for missing answers highlights a key point for working with nullable reference types: your goal isn't to remove all `null` values from your program.</span></span> <span data-ttu-id="be2a8-201">而是确保编写的代码表达设计意图。</span><span class="sxs-lookup"><span data-stu-id="be2a8-201">Rather, your goal is to ensure that the code you write expresses the intent of your design.</span></span> <span data-ttu-id="be2a8-202">缺失值是在代码中进行表达的一个必需概念。</span><span class="sxs-lookup"><span data-stu-id="be2a8-202">Missing values are a necessary concept to express in your code.</span></span> <span data-ttu-id="be2a8-203">`null` 值是表示这些缺失值的一种明确方法。</span><span class="sxs-lookup"><span data-stu-id="be2a8-203">The `null` value is a clear way to express those missing values.</span></span> <span data-ttu-id="be2a8-204">尝试删除所有 `null` 值只会导致定义一些其他方法来在没有 `null` 的情况下表示缺失值。</span><span class="sxs-lookup"><span data-stu-id="be2a8-204">Trying to remove all `null` values only leads to defining some other way to express those missing values without `null`.</span></span>

<span data-ttu-id="be2a8-205">接下来，你需要在 `SurveyRun` 类中编写 `PerformSurvey` 方法。</span><span class="sxs-lookup"><span data-stu-id="be2a8-205">Next, you need to write the `PerformSurvey` method in the `SurveyRun` class.</span></span> <span data-ttu-id="be2a8-206">将下面的代码添加到 `SurveyRun` 类中：</span><span class="sxs-lookup"><span data-stu-id="be2a8-206">Add the following code in the `SurveyRun` class:</span></span>

[!code-csharp[PerformSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#PerformSurvey)]

<span data-ttu-id="be2a8-207">同样，你选择的可为空 `List<SurveyResponse>?` 指示响应可能为 NULL。</span><span class="sxs-lookup"><span data-stu-id="be2a8-207">Here again, your choice of a nullable `List<SurveyResponse>?` indicates the response may be null.</span></span> <span data-ttu-id="be2a8-208">这表明尚未向任何回应者提供调查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-208">That indicates the survey hasn't been given to any respondents yet.</span></span> <span data-ttu-id="be2a8-209">请注意，在同意参与调查的回应者未达到足够数量之前，不会添加回应者。</span><span class="sxs-lookup"><span data-stu-id="be2a8-209">Notice that respondents are added until enough have consented.</span></span>

<span data-ttu-id="be2a8-210">运行调查的最后一步是添加一个调用，从而可在 `Main` 方法结束时执行此调查：</span><span class="sxs-lookup"><span data-stu-id="be2a8-210">The last step to run the survey is to add a call to perform the survey at the end of the `Main` method:</span></span>

[!code-csharp[RunSurvey](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#RunSurvey)]

## <a name="examine-survey-responses"></a><span data-ttu-id="be2a8-211">检查调查响应</span><span class="sxs-lookup"><span data-stu-id="be2a8-211">Examine survey responses</span></span>

<span data-ttu-id="be2a8-212">最后一步是显示调查结果。</span><span class="sxs-lookup"><span data-stu-id="be2a8-212">The last step is to display survey results.</span></span> <span data-ttu-id="be2a8-213">将代码添加到你所编写的诸多类。</span><span class="sxs-lookup"><span data-stu-id="be2a8-213">You'll add code to many of the classes you've written.</span></span> <span data-ttu-id="be2a8-214">此代码演示了区分可为空和不可为空引用类型的值。</span><span class="sxs-lookup"><span data-stu-id="be2a8-214">This code demonstrates the value of distinguishing nullable and non-nullable reference types.</span></span> <span data-ttu-id="be2a8-215">首先将以下两个表达式形式成员添加到 `SurveyResponse` 类：</span><span class="sxs-lookup"><span data-stu-id="be2a8-215">Start by adding the following two expression-bodied members to the `SurveyResponse` class:</span></span>

[!code-csharp[ReportResponses](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyResponse.cs#SurveyStatus)]

<span data-ttu-id="be2a8-216">因为 `surveyResponses` 是一个不可为空引用，所以在取消引用之前不需要输入任何检查。</span><span class="sxs-lookup"><span data-stu-id="be2a8-216">Because `surveyResponses` is a nullable reference type, null checks are necessary before de-referencing it.</span></span> <span data-ttu-id="be2a8-217">`Answer` 方法返回不可为空的字符串，因此我们必须使用 null 合并运算符来涵盖缺少答案的情况。</span><span class="sxs-lookup"><span data-stu-id="be2a8-217">The `Answer` method returns a non-nullable string, so we have to cover the case of a missing answer by using the null-coalescing operator.</span></span>

<span data-ttu-id="be2a8-218">接下来，将这三个表达式形式成员添加到 `SurveyRun` 类：</span><span class="sxs-lookup"><span data-stu-id="be2a8-218">Next, add these three expression-bodied members to the `SurveyRun` class:</span></span>

[!code-csharp[ReportResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/SurveyRun.cs#RunReport)]

<span data-ttu-id="be2a8-219">`AllParticipants` 成员必须考虑 `respondents` 变量可能为 NULL 但返回值不能为 NULL 的情况。</span><span class="sxs-lookup"><span data-stu-id="be2a8-219">The `AllParticipants` member must take into account that the `respondents` variable might be null, but the return value can't be null.</span></span> <span data-ttu-id="be2a8-220">如果通过删除后面的 `??` 和空序列来更改该表达式，则编译器会警告你方法可能返回 `null` 并且其返回签名返回不可为空类型。</span><span class="sxs-lookup"><span data-stu-id="be2a8-220">If you change that expression by removing the `??` and the empty sequence that follows, the compiler warns you the method might return `null` and its return signature returns a non-nullable type.</span></span>

<span data-ttu-id="be2a8-221">最后，在 `Main` 方法的底部添加以下循环：</span><span class="sxs-lookup"><span data-stu-id="be2a8-221">Finally, add the following loop at the bottom of the `Main` method:</span></span>

[!code-csharp[DisplaySurveyResults](~/samples/snippets/csharp/NullableIntroduction/NullableIntroduction/Program.cs#WriteAnswers)]

<span data-ttu-id="be2a8-222">你不需要在此代码中执行任何 `null` 检查，因为你已设计了基础接口，这样它们均返回不可为空引用类型。</span><span class="sxs-lookup"><span data-stu-id="be2a8-222">You don't need any `null` checks in this code because you've designed the underlying interfaces so that they all return non-nullable reference types.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="be2a8-223">获取代码</span><span class="sxs-lookup"><span data-stu-id="be2a8-223">Get the code</span></span>

<span data-ttu-id="be2a8-224">你可以从 [csharp/NullableIntroduction](https://github.com/dotnet/samples/tree/main/csharp/NullableIntroduction) 文件夹中的[示例](https://github.com/dotnet/samples)存储库获取已完成教程的代码。</span><span class="sxs-lookup"><span data-stu-id="be2a8-224">You can get the code for the finished tutorial from our [samples](https://github.com/dotnet/samples) repository in the [csharp/NullableIntroduction](https://github.com/dotnet/samples/tree/main/csharp/NullableIntroduction) folder.</span></span>

<span data-ttu-id="be2a8-225">通过更改可为空和不可为空引用类型之间的类型声明进行试验。</span><span class="sxs-lookup"><span data-stu-id="be2a8-225">Experiment by changing the type declarations between nullable and non-nullable reference types.</span></span> <span data-ttu-id="be2a8-226">了解如何生成不同的警告以确保不会意外取消引用 `null`。</span><span class="sxs-lookup"><span data-stu-id="be2a8-226">See how that generates different warnings to ensure you don't accidentally dereference a `null`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be2a8-227">后续步骤</span><span class="sxs-lookup"><span data-stu-id="be2a8-227">Next steps</span></span>

<span data-ttu-id="be2a8-228">要了解更多信息，请迁移现有应用程序以使用可为空引用类型：</span><span class="sxs-lookup"><span data-stu-id="be2a8-228">Learn more by migrating an existing application to use nullable reference types:</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="be2a8-229">升级应用程序以使用可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="be2a8-229">Upgrade an application to use nullable reference types</span></span>](upgrade-to-nullable-references.md)

<span data-ttu-id="be2a8-230">了解如何在使用实体框架时使用可为空引用类型：</span><span class="sxs-lookup"><span data-stu-id="be2a8-230">Learn how to use nullable reference type when using Entity Framework:</span></span>
> [<span data-ttu-id="be2a8-231">Entity Framework Core 基础知识：使用可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="be2a8-231">Entity Framework Core Fundamentals: Working with Nullable Reference Types</span></span>](/ef/core/miscellaneous/nullable-reference-types)
