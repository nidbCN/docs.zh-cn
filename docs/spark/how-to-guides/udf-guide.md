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
# <a name="create-user-defined-functions-udf-in-net-for-apache-spark"></a>在 .NET for Apache Spark 中创建用户定义的函数 (UDF)

本文介绍如何在 .NET for Apache Spark 中使用用户定义的函数 (UDF)。 [UDF](https://spark.apache.org/docs/latest/api/java/org/apache/spark/sql/expressions/UserDefinedFunction.html) 是一项 Spark 功能，该功能允许使用自定义函数扩展系统的内置功能。 UDF 根据 UDF 中定义的逻辑转换表中单行的值，使每行生成一个相应的输出值。

## <a name="define-udfs"></a>定义 UDF

查看以下 UDF 定义：

```csharp
string s1 = "hello";
Func<Column, Column> udf = Udf<string, string>(
    str => $"{s1} {str}");
```

UDF 将 `string` 以[数据帧](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/DataFrame.cs#L24)的[列](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Column.cs#L14)的形式作为输入，并返回在该输入之前追加 `hello` 的 `string`。

以下数据帧 `df` 包含名称列表：

```text
+-------+
|   name|
+-------+
|Michael|
|   Andy|
| Justin|
+-------+
```

现在，将上面定义的 `udf` 应用于数据帧 `df`：

```csharp
DataFrame udfResult = df.Select(udf(df["name"]));
```

以下数据帧 `udfResult` 是 UDF 的结果：

```text
+-------------+
|         name|
+-------------+
|hello Michael|
|   hello Andy|
| hello Justin|
+-------------+
```

为更好地了解如何实现 UDF，请在 GitHub 上查看 [UDF 帮助程序函数](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/Functions.cs#L3616)和[示例](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark.E2ETest/UdfTests/UdfSimpleTypesTests.cs#L49)。

## <a name="udf-serialization"></a>UDF 序列化

由于 UDF 函数需要在辅助角色上运行，因此必须将其序列化并作为有效负载的一部分从驱动程序发送到辅助角色。 需要将[委托](../../csharp/programming-guide/delegates/index.md)（即对方法的引用）及其[目标](xref:System.Delegate.Target%2A)（即当前委托在其中调用实例方法的类实例）序列化。 查看此处的 [GitHub 中的代码示例](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Utils/CommandSerDe.cs#L149)，更好地了解如何实现 UDF 序列化。

.NET for Apache Spark 使用不支持序列化委托的 .NET Core。 相反，反射用于序列化定义委托的目标。 在公共范围内定义多个委托时，这些委托将具有一个共享闭包，该闭包会成为用于序列化的反射的目标。

### <a name="serialization-example"></a>序列化示例

以下代码片段定义了两个函数委托中引用的两个字符串变量，这两个委托返回各自的字符串作为结果：

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

上述 C# 代码从编译器生成以下 C# 反汇编（信用来源：[sharplab.io](https://sharplab.io)）代码：

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

`func` 和 `func2` 共享同一闭包 `<>c__DisplayClass0_0`，该闭包是序列化委托 `func` 和 `func2` 时要序列化的目标。 当字节发送到辅助角色时，即使 `Func<string, string> a` 仅引用 `s1`，`s2` 也会进行序列化。

这可能会在运行时（例如，使用[广播变量](broadcast-guide.md)时）期间导致某些意外行为，因此建议将函数所用变量的可见性限制在该函数范围内。

建议使用以下代码片段实现所需的序列化行为：

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

上述 C# 代码从编译器生成以下 C# 反汇编（信用来源：[sharplab.io](https://sharplab.io)）代码：

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

请注意，`func` 和 `func2` 不再共享闭包，它们各自具有独立闭包 `<>c__DisplayClass0_0` 和 `<>c__DisplayClass0_1`。 用作实现序列化的目标时，仅引用的变量会针对委托进行序列化。 在公共范围内实现多个 UDF 时，请务必牢记这一行为。

## <a name="some-spark-udf-caveats"></a>部分 Spark UDF 注意事项

* UDF 中的 Null 值可能引发异常。 开发人员有责任对其进行处理。
* UDF 不使用 Spark 内置函数提供的优化功能，因此建议尽可能使用内置函数。

## <a name="faqs"></a>常见问题解答

尝试使用 `ArrayType``MapType``ArrayList` 或 `HashTable` 作为参数或返回类型调用 UDF 时，为什么会收到错误 `System.NotImplementedException: The method or operation is not implemented.` 或 `System.InvalidCastException: Unable to cast object of type 'System.Collections.Hashtable' to type 'System.Collections.Generic.IDictionary`？  
对 `ArrayType` 和 `MapType` 的支持直到 [v1.0](https://github.com/dotnet/spark/releases/tag/v1.0.0) 才提供，因此如果使用之前的 .NET for Apache Spark 版本，并尝试将这些类型作为参数传递给 UDF 或作为返回类型，则会出现此错误。
不支持将 `ArrayList` 和 `HashTable` 类型作为 UDF 的返回类型，因为它们是非泛型集合，因此无法向 Spark 提供其元素类型定义。

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [在 Windows 上调试 .NET for Apache Spark 应用程序](debug.md)
