---
title: 在 .NET for Apache Spark 中创建用户定义的函数 (UDF)
description: 了解如何在 .NET for Apache Spark 应用程序中实现用户定义的函数 (UDF)。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 13898bdbd9522730c8f0cd19bd13d4e6f3a6a10e
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874064"
---
# <a name="create-user-defined-functions-udf-in-net-for-apache-spark"></a><span data-ttu-id="d4648-103">在 .NET for Apache Spark 中创建用户定义的函数 (UDF)</span><span class="sxs-lookup"><span data-stu-id="d4648-103">Create user-defined functions (UDF) in .NET for Apache Spark</span></span>

<span data-ttu-id="d4648-104">本文介绍如何在 .NET for Apache Spark 中使用用户定义的函数 (UDF)。</span><span class="sxs-lookup"><span data-stu-id="d4648-104">In this article, you learn how to use user-defined functions (UDF) in .NET for Apache Spark.</span></span> <span data-ttu-id="d4648-105">[UDF](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/expressions/UserDefinedFunction.html) 是一项 Spark 功能，该功能允许使用自定义函数扩展系统的内置功能。</span><span class="sxs-lookup"><span data-stu-id="d4648-105">[UDFs)](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/expressions/UserDefinedFunction.html) are a Spark feature that allow you to use custom functions to extend the system's built-in functionality.</span></span> <span data-ttu-id="d4648-106">UDF 根据 UDF 中定义的逻辑转换表中单行的值，使每行生成一个相应的输出值。</span><span class="sxs-lookup"><span data-stu-id="d4648-106">UDFs transform values from a single row within a table to produce a single corresponding output value per row based on the logic defined in the UDF.</span></span>

## <a name="define-udfs"></a><span data-ttu-id="d4648-107">定义 UDF</span><span class="sxs-lookup"><span data-stu-id="d4648-107">Define UDFs</span></span>

<span data-ttu-id="d4648-108">查看以下 UDF 定义：</span><span class="sxs-lookup"><span data-stu-id="d4648-108">Review the following UDF definition:</span></span>

```csharp
string s1 = "hello";
Func<Column, Column> udf = Udf<string, string>(
    str => $"{s1} {str}");
```

<span data-ttu-id="d4648-109">UDF 将 `string` 以[数据帧](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/DataFrame.cs#L24)的[列](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Column.cs#L14)的形式作为输入，并返回在该输入之前追加 `hello` 的 `string`。</span><span class="sxs-lookup"><span data-stu-id="d4648-109">The UDF takes a `string` as an input in the form of a [Column](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Column.cs#L14) of a [Dataframe](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/DataFrame.cs#L24)) and returns a `string` with `hello` appended in front of the input.</span></span>

<span data-ttu-id="d4648-110">以下数据帧 `df` 包含名称列表：</span><span class="sxs-lookup"><span data-stu-id="d4648-110">The following DataFrame `df` contains a list of names:</span></span>

```text
+-------+
|   name|
+-------+
|Michael|
|   Andy|
| Justin|
+-------+
```

<span data-ttu-id="d4648-111">现在，将上面定义的 `udf` 应用于数据帧 `df`：</span><span class="sxs-lookup"><span data-stu-id="d4648-111">Now let's apply the above defined `udf` to the DataFrame `df`:</span></span>

```csharp
DataFrame udfResult = df.Select(udf(df["name"]));
```

<span data-ttu-id="d4648-112">以下数据帧 `udfResult` 是 UDF 的结果：</span><span class="sxs-lookup"><span data-stu-id="d4648-112">The following DataFrame `udfResult` is the result of the UDF:</span></span>

```text
+-------------+
|         name|
+-------------+
|hello Michael|
|   hello Andy|
| hello Justin|
+-------------+
```

<span data-ttu-id="d4648-113">为更好地了解如何实现 UDF，请在 GitHub 上查看 [UDF 帮助程序函数](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Functions.cs#L3616)和[示例](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark.E2ETest/UdfTests/UdfSimpleTypesTests.cs#L49)。</span><span class="sxs-lookup"><span data-stu-id="d4648-113">To better understand how to implement UDFs, review the [UDF helper functions](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Functions.cs#L3616) and [examples](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark.E2ETest/UdfTests/UdfSimpleTypesTests.cs#L49) on GitHub.</span></span>

## <a name="udf-serialization"></a><span data-ttu-id="d4648-114">UDF 序列化</span><span class="sxs-lookup"><span data-stu-id="d4648-114">UDF serialization</span></span>

<span data-ttu-id="d4648-115">由于 UDF 函数需要在辅助角色上运行，因此必须将其序列化并作为有效负载的一部分从驱动程序发送到辅助角色。</span><span class="sxs-lookup"><span data-stu-id="d4648-115">Because UDFs are functions that need to be executed on workers, they have to be serialized and sent to the workers as part of the payload from the driver.</span></span> <span data-ttu-id="d4648-116">需要将[委托](../../csharp/programming-guide/delegates/index.md)（即对方法的引用）及其[目标](xref:System.Delegate.Target%2A)（即当前委托在其中调用实例方法的类实例）序列化。</span><span class="sxs-lookup"><span data-stu-id="d4648-116">The [delegate](../../csharp/programming-guide/delegates/index.md), which is a reference to the method, needs to be serialized as well as its [target](xref:System.Delegate.Target%2A), which is the class instance on which the current delegate invokes the instance method.</span></span> <span data-ttu-id="d4648-117">查看此处的 [GitHub 中的代码示例](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Utils/CommandSerDe.cs#L149)，更好地了解如何实现 UDF 序列化。</span><span class="sxs-lookup"><span data-stu-id="d4648-117">Review this [code example in GitHub](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Utils/CommandSerDe.cs#L149) to get a better understanding of how UDF serialization is being done.</span></span>

<span data-ttu-id="d4648-118">.NET for Apache Spark 使用不支持序列化委托的 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="d4648-118">.NET for Apache Spark uses .NET Core, which doesn't support serializing delegates.</span></span> <span data-ttu-id="d4648-119">相反，反射用于序列化定义委托的目标。</span><span class="sxs-lookup"><span data-stu-id="d4648-119">Instead, reflection is used to serialize the target where the delegate is defined.</span></span> <span data-ttu-id="d4648-120">在公共范围内定义多个委托时，这些委托将具有一个共享闭包，该闭包会成为用于序列化的反射的目标。</span><span class="sxs-lookup"><span data-stu-id="d4648-120">When multiple delegates are defined in a common scope, they have a shared closure that becomes the target of reflection for serialization.</span></span>

### <a name="serialization-example"></a><span data-ttu-id="d4648-121">序列化示例</span><span class="sxs-lookup"><span data-stu-id="d4648-121">Serialization example</span></span>

<span data-ttu-id="d4648-122">以下代码片段定义了两个函数委托中引用的两个字符串变量，这两个委托返回各自的字符串作为结果：</span><span class="sxs-lookup"><span data-stu-id="d4648-122">The following code snippet defines two string variables that are being referenced in two function delegates that return the respective strings as a result:</span></span>

```csharp
using System;

public class C {
    public void M() {
        string s1 = "s1";
        string s2 = "s2";
        Func<string, string> a = str => s1;
        Func<string, string> b = str => s2;
    }
}
```

<span data-ttu-id="d4648-123">上述 C# 代码从编译器生成以下 C# 反汇编（信用来源：[sharplab.io](https://sharplab.io)）代码：</span><span class="sxs-lookup"><span data-stu-id="d4648-123">The above C# code generates the following C# disassembly (credit source: [sharplab.io](https://sharplab.io)) code from the compiler:</span></span>

```csharp
public class C
{
    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_0
    {
        public string s1;

        public string s2;

        internal string <M>b__0(string str)
        {
            return s1;
        }

        internal string <M>b__1(string str)
        {
            return s2;
        }
    }

    public void M()
    {
        <>c__DisplayClass0_0 <>c__DisplayClass0_ = new <>c__DisplayClass0_0();
        <>c__DisplayClass0_.s1 = "s1";
        <>c__DisplayClass0_.s2 = "s2";
        Func<string, string> func = new Func<string, string>(<>c__DisplayClass0_.<M>b__0);
        Func<string, string> func2 = new Func<string, string>(<>c__DisplayClass0_.<M>b__1);
    }
}
```

<span data-ttu-id="d4648-124">`func` 和 `func2` 共享同一闭包 `<>c__DisplayClass0_0`，该闭包是序列化委托 `func` 和 `func2` 时要序列化的目标。</span><span class="sxs-lookup"><span data-stu-id="d4648-124">Both `func` and `func2` share the same closure `<>c__DisplayClass0_0`, which is the target that is serialized when serializing the delegates `func` and `func2`.</span></span> <span data-ttu-id="d4648-125">当字节发送到辅助角色时，即使 `Func<string, string> a` 仅引用 `s1`，`s2` 也会进行序列化。</span><span class="sxs-lookup"><span data-stu-id="d4648-125">Even though `Func<string, string> a` is only referencing `s1`, `s2` is also serialized when the bytes are sent to the workers.</span></span>

<span data-ttu-id="d4648-126">这可能会在运行时（例如，使用[广播变量](broadcast-guide.md)时）期间导致某些意外行为，因此建议将函数所用变量的可见性限制在该函数范围内。</span><span class="sxs-lookup"><span data-stu-id="d4648-126">This can lead to some unexpected behaviors at runtime (like in the case of using [broadcast variables](broadcast-guide.md)), which is why we recommend that you restrict the visibility of the variables used in a function to that function's scope.</span></span>

<span data-ttu-id="d4648-127">建议使用以下代码片段实现所需的序列化行为：</span><span class="sxs-lookup"><span data-stu-id="d4648-127">The following code snippet is the recommended way to implement the desired serialization behavior:</span></span>

```csharp
using System;

public class C {
    public void M() {
        {
            string s1 = "s1";
            Func<string, string> a = str => s1;
        }
        {
            string s2 = "s2";
            Func<string, string> b = str => s2;
        }
    }
}
```

<span data-ttu-id="d4648-128">上述 C# 代码从编译器生成以下 C# 反汇编（信用来源：[sharplab.io](https://sharplab.io)）代码：</span><span class="sxs-lookup"><span data-stu-id="d4648-128">The above C# code generates the following C# disassembly (credit source: [sharplab.io](https://sharplab.io)) code from the compiler:</span></span>

```csharp
public class C
{
    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_0
    {
        public string s1;

        internal string <M>b__0(string str)
        {
            return s1;
        }
    }

    [CompilerGenerated]
    private sealed class <>c__DisplayClass0_1
    {
        public string s2;

        internal string <M>b__1(string str)
        {
            return s2;
        }
    }

    public void M()
    {
        <>c__DisplayClass0_0 <>c__DisplayClass0_ = new <>c__DisplayClass0_0();
        <>c__DisplayClass0_.s1 = "s1";
        Func<string, string> func = new Func<string, string>(<>c__DisplayClass0_.<M>b__0);
        <>c__DisplayClass0_1 <>c__DisplayClass0_2 = new <>c__DisplayClass0_1();
        <>c__DisplayClass0_2.s2 = "s2";
        Func<string, string> func2 = new Func<string, string>(<>c__DisplayClass0_2.<M>b__1);
    }
}
```

<span data-ttu-id="d4648-129">请注意，`func` 和 `func2` 不再共享闭包，它们各自具有独立闭包 `<>c__DisplayClass0_0` 和 `<>c__DisplayClass0_1`。</span><span class="sxs-lookup"><span data-stu-id="d4648-129">Notice that `func` and `func2` no longer share a closure, and they have their own separate closures `<>c__DisplayClass0_0` and `<>c__DisplayClass0_1` respectively.</span></span> <span data-ttu-id="d4648-130">用作实现序列化的目标时，仅引用的变量会针对委托进行序列化。</span><span class="sxs-lookup"><span data-stu-id="d4648-130">When used as the target for serialization, nothing other than the referenced variables will get serialized for the delegate.</span></span> <span data-ttu-id="d4648-131">在公共范围内实现多个 UDF 时，请务必牢记这一行为。</span><span class="sxs-lookup"><span data-stu-id="d4648-131">This behavior is important to keep in mind while implementing multiple UDFs in a common scope.</span></span>

## <a name="some-spark-udf-caveats"></a><span data-ttu-id="d4648-132">部分 Spark UDF 注意事项</span><span class="sxs-lookup"><span data-stu-id="d4648-132">Some Spark UDF caveats</span></span>

* <span data-ttu-id="d4648-133">UDF 中的 Null 值可能引发异常。</span><span class="sxs-lookup"><span data-stu-id="d4648-133">Null values in UDFs can throw exceptions.</span></span> <span data-ttu-id="d4648-134">开发人员有责任对其进行处理。</span><span class="sxs-lookup"><span data-stu-id="d4648-134">It's the responsibility of the developer to handle them.</span></span>
* <span data-ttu-id="d4648-135">UDF 不使用 Spark 内置函数提供的优化功能，因此建议尽可能使用内置函数。</span><span class="sxs-lookup"><span data-stu-id="d4648-135">UDFs don't leverage the optimizations provided by Spark's built-in functions, so it's recommended to use built-in functions where possible.</span></span>

## <a name="faqs"></a><span data-ttu-id="d4648-136">常见问题解答</span><span class="sxs-lookup"><span data-stu-id="d4648-136">FAQs</span></span>

<span data-ttu-id="d4648-137">尝试使用 `ArrayType``MapType``ArrayList` 或 `HashTable` 作为参数或返回类型调用 UDF 时，为什么会收到错误 `System.NotImplementedException: The method or operation is not implemented.` 或 `System.InvalidCastException: Unable to cast object of type 'System.Collections.Hashtable' to type 'System.Collections.Generic.IDictionary`？</span><span class="sxs-lookup"><span data-stu-id="d4648-137">**Why do I get the error `System.NotImplementedException: The method or operation is not implemented.` or `System.InvalidCastException: Unable to cast object of type 'System.Collections.Hashtable' to type 'System.Collections.Generic.IDictionary` when trying to call a UDF with `ArrayType`, `MapType`, `ArrayList`, or `HashTable` as argument or return type?**</span></span>  
<span data-ttu-id="d4648-138">对 `ArrayType` 和 `MapType` 的支持直到 [v1.0](https://github.com/dotnet/spark/releases/tag/v1.0.0) 才提供，因此如果使用之前的 .NET for Apache Spark 版本，并尝试将这些类型作为参数传递给 UDF 或作为返回类型，则会出现此错误。</span><span class="sxs-lookup"><span data-stu-id="d4648-138">Support for `ArrayType` and `MapType` is not provided until [v1.0](https://github.com/dotnet/spark/releases/tag/v1.0.0), and so you would get this error if using a .NET for Apache Spark version prior to that, and trying to pass these types either as arguments to the UDF or as a return type.</span></span>
<span data-ttu-id="d4648-139">不支持将 `ArrayList` 和 `HashTable` 类型作为 UDF 的返回类型，因为它们是非泛型集合，因此无法向 Spark 提供其元素类型定义。</span><span class="sxs-lookup"><span data-stu-id="d4648-139">`ArrayList` and `HashTable` types cannot be supported as return types of a UDF as they are non-generic collections and hence their element type definitions cannot be provided to Spark.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d4648-140">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d4648-140">Next steps</span></span>

* [<span data-ttu-id="d4648-141">.NET for Apache Spark 入门</span><span class="sxs-lookup"><span data-stu-id="d4648-141">Get started with .NET for Apache Spark</span></span>](../tutorials/get-started.md)
* [<span data-ttu-id="d4648-142">在 Windows 上调试 .NET for Apache Spark 应用程序</span><span class="sxs-lookup"><span data-stu-id="d4648-142">Debug a .NET for Apache Spark application on Windows</span></span>](debug.md)
