---
title: with 表达式 - C# 参考
description: 了解执行 C# 记录的非破坏性转变的 with 表达式
ms.date: 11/12/2020
f1_keywords:
- with_CSharpKeyword
helpviewer_keywords:
- with expression [C#]
- with operator [C#]
ms.openlocfilehash: 24b9fdfcef6fe042204217323bb09c047c58296c
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872530"
---
# <a name="with-expression-c-reference"></a>with 表达式（C# 参考）

`with` 表达式在 C# 9.0 及更高版本中可用，使用修改的特定属性和字段生成其[记录](../builtin-types/record.md)操作数的副本：

:::code language="csharp" source="snippets/with-expression/BasicExample.cs" :::

如上一个示例所示，你可以使用[对象初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)语法来指定要修改的成员及其新值。 在 `with` 表达式中，左侧操作数必须为记录类型。

`with` 表达式的结果与表达式的操作数具有相同的运行时类型，如以下示例所示：

:::code language="csharp" source="snippets/with-expression/InheritanceExample.cs" :::

对于引用类型成员，在复制记录时仅复制对实例的引用。 副本和原始记录都具有对同一引用类型实例的访问权限。 以下示例演示了该行为：

:::code language="csharp" source="snippets/with-expression/ExampleWithReferenceType.cs" :::

任何记录类型都具有复制构造函数。 这是一个包含记录类型的单个参数的构造函数。 它将参数的状态复制到新的记录实例。 在评估 `with` 表达式时，将调用复制构造函数，以基于原始记录实例化新记录实例化。 之后，新实例将根据指定的修改进行更新。 默认情况下，复制构造函数是隐式的，即编译器生成的。 如果需要自定义记录复制语义，请显式声明具有所需行为的复制构造函数。 下面的示例使用显式复制构造函数更新前面的示例。 复制记录时，新的复制行为是复制列表项而不是列表引用：

:::code language="csharp" source="snippets/with-expression/UserDefinedCopyConstructor.cs" :::

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅[记录功能建议说明](~/_csharplang/proposals/csharp-9.0/records.md)的以下部分：

- [`with`表达式](~/_csharplang/proposals/csharp-9.0/records.md#with-expression)
- [复制和克隆成员](~/_csharplang/proposals/csharp-9.0/records.md#copy-and-clone-members)

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [记录](../builtin-types/record.md)
