---
title: 如何使用 XML 文档功能 - C# 编程指南
description: 了解如何使用 XML 文档功能。 查看代码示例和其他可用资源。
ms.date: 06/01/2018
helpviewer_keywords:
- XML documentation [C#]
- C# language, XML documentation features
ms.assetid: 8f33917b-9577-4c9a-818a-640dbbb0b399
ms.openlocfilehash: 3a030e07e6f0e48a37c1682ef1d93b33932bbaaa
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478423"
---
# <a name="how-to-use-the-xml-documentation-features"></a>如何使用 XML 文档功能

下面的示例提供对某个已存档类型的基本概述。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#15)]

该示例生成一个包含以下内容的 .xml 文件。

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>xmlsample</name>
    </assembly>
    <members>
        <member name="T:TestClass">
            <summary>
            Class level summary documentation goes here.
            </summary>
            <remarks>
            Longer comments can be associated with a type or member through
            the remarks tag.
            </remarks>
        </member>
        <member name="F:TestClass._name">
            <summary>
            Store for the Name property.
            </summary>
        </member>
        <member name="M:TestClass.#ctor">
            <summary>
            The class constructor.
            </summary>
        </member>
        <member name="P:TestClass.Name">
            <summary>
            Name property.
            </summary>
            <value>
            A value tag is used to describe the property value.
            </value>
        </member>
        <member name="M:TestClass.SomeMethod(System.String)">
            <summary>
            Description for SomeMethod.
            </summary>
            <param name="s"> Parameter description for s goes here.</param>
            <seealso cref="T:System.String">
            You can use the cref attribute on any tag to reference a type or member
            and the compiler will check that the reference exists.
            </seealso>
        </member>
        <member name="M:TestClass.SomeOtherMethod">
            <summary>
            Some other method.
            </summary>
            <returns>
            Return values are described through the returns tag.
            </returns>
            <seealso cref="M:TestClass.SomeMethod(System.String)">
            Notice the use of the cref attribute to reference a specific method.
            </seealso>
        </member>
        <member name="M:TestClass.Main(System.String[])">
            <summary>
            The entry point for the application.
            </summary>
            <param name="args"> A list of command line arguments.</param>
        </member>
        <member name="T:TestInterface">
            <summary>
            Documentation that describes the interface goes here.
            </summary>
            <remarks>
            Details about the interface go here.
            </remarks>
        </member>
        <member name="M:TestInterface.InterfaceMethod(System.Int32)">
            <summary>
            Documentation that describes the method goes here.
            </summary>
            <param name="n">
            Parameter n requires an integer argument.
            </param>
            <returns>
            The method returns an integer.
            </returns>
        </member>
    </members>
</doc>
```

## <a name="compiling-the-code"></a>编译代码

若要编译该示例，请输入以下命令：

`csc XMLsample.cs /doc:XMLsample.xml`

此命令创建 XML 文件 XMLsample.xml，可在浏览器中或使用 `TYPE` 命令查看该文件。

## <a name="robust-programming"></a>可靠编程

XML 文档以 `///` 开头。 创建新项目时，向导会放置一些以 `///` 开头的行。 处理这些注释时存在一些限制：

- 文档必须是格式正确的 XML。 如果 XML 格式不正确，则会生成警告，并且文档文件将包含一条注释，指出遇到错误。

- 开发人员可以随意创建自己的标记集。 有一组[推荐的标记](recommended-tags-for-documentation-comments.md)。 部分建议标记具有特殊含义：

  - `<param>` 标记用于描述参数。 如果已使用，编译器会验证该参数是否存在，以及文档是否描述了所有参数。 如果验证失败，编译器会发出警告。

  - `cref` 属性可以附加到任何标记，以引用代码元素。 编译器验证此代码元素是否存在。 如果验证失败，编译器会发出警告。 编译器在查找 `cref` 属性中描述的类型时会考虑所有 `using` 语句。

  - `<summary>` 标记由 Visual Studio 中的 IntelliSense 用于显示有关某个类型或成员的附加信息。

    > [!NOTE]
    > XML 文件不提供有关该类型和成员的完整信息（例如，它不包含任何类型信息）。 若要获取有关类型或成员的完整信息，请将文档文件与对实际类型或成员的反射一起使用。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [DocumentationFile（C# 编译器选项）](../../language-reference/compiler-options/output.md#documentationfile)
- [XML 文档注释](./index.md)
- [DocFX 文档处理器](https://dotnet.github.io/docfx/)
- [Sandcastle 文档处理器](https://github.com/EWSoftware/SHFB)
