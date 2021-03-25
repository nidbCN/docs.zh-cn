---
title: 处理 XML 文件 - C# 编程指南
description: 了解如何在 C# 编程中处理 XML 文件。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- XML processing [C#]
- XML [C#], processing
ms.assetid: 60c71193-9dac-4cd3-98c5-100bd0edcc42
ms.openlocfilehash: 5ce0f311ccc16b4eca8d499578c217d93cc93165
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477865"
---
# <a name="process-the-xml-file-c-programming-guide"></a>处理 XML 文件（C# 编程指南）

编译器为代码（已标记以生成文档）中的每个构造生成一个 ID 字符串。 （有关如何标记代码的信息，请参阅[文档注释的建议标记](./recommended-tags-for-documentation-comments.md)。）ID 字符串唯一标识构造。 处理 XML 文件的程序可以使用 ID 字符串来标识文档应用于的相应 .NET 元数据或反射项目。

## <a name="id-strings"></a>ID 字符串

XML 文件不是代码的层次结构表示形式。 它是一个简单列表，其中每个元素都有一个生成 ID。

编译器在生成 ID 字符串时应遵循以下规则：

- 字符串不得包含空白符。

- 该字符串的第一部分使用单个字符后跟冒号来标识成员类型。 使用下面的成员类型：

    |字符|成员类型|说明|
    |---------------|-----------------|-|
    |N|namespace|无法将文档注释添加到命名空间中，但可以在支持的情况下对它们进行 cref 引用。|
    |T|type|类型可能是类、接口、结构、枚举或委托。|
    |F|Field — 字段|
    |P|property|包括索引器或其他索引属性。|
    |M|method|包括特殊方法，如构造函数和运算符。|
    |E|event|
    |!|错误字符串|字符串的其余部分提供有关错误的信息。 C# 编译器将生成无法解析的链接的错误信息。|

- 该字符串的第二部分是项目的完全限定名称，从命名空间的根开始。 用句点分隔项目名称、其封闭类型和命名空间。 如果项目名称本身包含句点，会将其替换为哈希符号 ('#')。 假定没有名称中恰好包含哈希符号的项目。 例如，String 构造函数的完全限定名称是“System.String.#ctor”。

- 对于属性和方法，括号内的参数列表如下。 如果没有任何参数，则不会出现括号。 这些参数以逗号分隔。 每个参数的编码直接遵循它在 .NET 签名中的编码方式：

  - 基类型。 常规的类型（ELEMENT_TYPE_CLASS 或 ELEMENT_TYPE_VALUETYPE）表示为该类型的完全限定名称。

  - 内部类型（例如，ELEMENT_TYPE_I4、ELEMENT_TYPE_OBJECT、ELEMENT_TYPE_STRING、ELEMENT_TYPE_TYPEDBYREF 和 ELEMENT_TYPE_VOID）表示为相应完整类型的完全限定名称。 例如，System.Int32 或 System.TypedReference。

  - ELEMENT_TYPE_PTR 表示为修改类型之后的“\*”。

  - ELEMENT_TYPE_BYREF 表示为修改类型之后的“\@”。

  - ELEMENT_TYPE_PINNED 表示为修改类型之后的“^”。 C# 编译器不会生成此类型。

  - ELEMENT_TYPE_CMOD_REQ 表示为“&#124;”和修饰符类的完全限定名称，前面是修改类型。 C# 编译器不会生成此类型。

  - ELEMENT_TYPE_CMOD_OPT 表示为“!”和修饰符类的完全限定名称，前面是修改类型。

  - ELEMENT_TYPE_SZARRAY 表示为“[]”，前面是数组的元素类型。

  - ELEMENT_TYPE_GENERICARRAY 表示为“[?]”，前面是数组的元素类型。 C# 编译器不会生成此类型。

  - ELEMENT_TYPE_ARRAY 表示为 [*lowerbound*:`size`,*lowerbound*:`size`]，其中逗号的数量是秩 - 1，每个维度的下限和大小（如果已知）以十进制形式表示。 如果未指定下限或大小，则将其省略。 如果省略某个特定维度的下限和大小，也会省略“:”。 例如，以 1 作为下限并且未指定大小的二维数组是 [1:,1:]。

  - ELEMENT_TYPE_FNPTR 表示为“=FUNC:`type`(*signature*)”，其中 `type` 是返回类型，*signature* 是方法的自变量。 如果没有任何自变量，则省略括号。 C# 编译器不会生成此类型。

  不表示下列签名组件，因为它们不会用于区分重载方法：

  - 调用约定

  - 返回类型

  - ELEMENT_TYPE_SENTINEL

- 仅针对转换运算符（`op_Implicit` 和 `op_Explicit`），该方法的返回值被编码为“~”，后面是返回类型。

- 对于泛型类型，类型名称后跟反引号，然后是指示泛型类型参数数量的一个数字。 例如：

     ``<member name="T:SampleClass`2">`` 是定义为 `public class SampleClass<T, U>` 的类型的标记。

     对于将泛型类型用作参数的方法，泛型类型参数被指定为前面加上反引号的数字（例如 \`0、\`1）。 每个数字表示该类型的泛型参数的从零开始的数组表示法。

## <a name="examples"></a>示例

下面的示例演示如何为类及其成员生成 ID 字符串：

[!code-csharp[csProgGuidePointers#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuidePointers/CS/Pointers.cs#21)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [DocumentationFile（C# 编译器选项）](../../language-reference/compiler-options/output.md#documentationfile)
- [XML 文档注释](./index.md)
