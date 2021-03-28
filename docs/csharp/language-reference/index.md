---
description: C# 参考
title: C# 参考
ms.date: 01/13/2021
ms.custom: updateeachrelease
f1_keywords:
- _CSharpKeyword
helpviewer_keywords:
- Visual C#, language reference
- language reference [C#]
- Programmer's Reference for C#
- C# language, reference
- reference, C# language
ms.assetid: 06de3167-c16c-4e1a-b3c5-c27841d4569a
ms.openlocfilehash: 1e0dff24bb581a27f55b047d5f6351993ba58cdb
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637489"
---
# <a name="c-reference"></a>C# 参考

此部分收录了有关 C# 关键字、运算符、特殊字符、预处理器指令、编译器选项以及编译器错误与警告的参考资料。  
  
## <a name="in-this-section"></a>本节内容

 [C# 关键字](./keywords/index.md)  
 提供指向有关 C# 关键字和语法的信息的链接。  
  
 [C# 运算符](./operators/index.md)  
 提供指向有关 C# 运算符和语法的信息的链接。  

 [C# 特殊字符](./tokens/index.md)  
 收录了主题链接，有助于你了解 C# 中的特殊上下文字符及其用法。  

 [C# 预处理器指令](preprocessor-directives.md)  
 提供用于嵌入 C# 源代码中的编译器命令有关的信息的链接。  
  
 [C# 编译器选项](./compiler-options/index.md)  
 包括有关编译器选项以及如何使用这些选项的信息。  
  
 [C# 编译器错误](./compiler-messages/index.md)  
 包括演示原因和更正 C# 编译器错误和警告的代码片段。  
  
 [C# 语言规范](../../../_csharplang/spec/introduction.md)  
 C# 6.0 语言规范。 本文是针对 C# 6.0 语言的建议草案。 本文档将通过与 ECMA C# 标准委员会协作来进行优化。 版本 5.0 已于 2017 年 12 月发布为 [Standard ECMA-334 第 5 版](https://www.ecma-international.org/wp-content/uploads/ECMA-334_5th_edition_december_2017.pdf)文档。

已在 C# 6.0 之后的版本中实现的功能将表示在语言规范建议中。 这些文档描述了语言规范的增量，以便添加这些新功能。 这些文档位于草稿建议窗体中。 这些规范将经过优化并提交到 ECMA 标准委员会，以便正式审查并合并到 C# 标准的未来版本。

 [C# 7.0 规范建议](../../../_csharplang/proposals/csharp-7.0/pattern-matching.md)  
 C# 7.0 中实现了许多新功能。 这些功能包括：模式匹配、本地函数、out 变量声明、throw 表达式、二进制文本和数字分隔符。 此文件夹包含适用于每个功能的规范。
  
 [C# 7.1 规范建议](../../../_csharplang/proposals/csharp-7.1/async-main.md)  
 C# 7.1 中有以下新增功能。 首先，可编写返回 `Task` 或 `Task<int>` 的 `Main` 方法。 由此可将 `async` 修饰符添加到 `Main`。 可推断类型的位置中没有类型时可使用 `default` 表达式。 同时，也可推断元组成员名称。 最后，可通过泛型使用模式匹配。

 [C# 7.2 规范建议](../../../_csharplang/proposals/csharp-7.2/readonly-ref.md)  
 C#7.2 添加了大量小功能。 可使用 `in` 关键字通过只读引用传递参数。 许多低级更改可支持 `Span` 及相关类型的编译时安全性。 在某些情况下，可使用已命名参数，其中后面的参数是位置参数。 借助 `private protected` 访问修饰符，可指定调用方仅限于在同一程序集中实现的派生类型。 `?:` 运算符可解析位对变量的引用。 还可使用前导数字分隔符格式化十六进制和二进制数字。

 [C# 7.3 规范建议](../../../_csharplang/proposals/csharp-7.3/blittable.md)  
 C# 7.3 是另一个包含多个小更新的重要发布。 可对泛型类型参数使用新约束。 通过其他更改，可更轻松地使用 `fixed` 字段，包括使用 [`stackalloc`](./operators/stackalloc.md) 分配。 可以重新分配使用 `ref` 关键字声明的本地变量以引用新存储。 可将属性放置在自动实现的属性上，该属性针对编译器生成的支持字段。 表达式变量可用于初始化表达式中。 可以比较元组的相等性（或不等性）。 重载决策也有一些改进。
  
 [C# 8.0 规范建议](../../../_csharplang/proposals/csharp-8.0/nullable-reference-types.md)  
 C# 8.0 可用于 .NET Core 3.0。 这些功能包括可为空引用类型、递归模式匹配、默认接口方法、异步流、范围和索引、基于模式的 using 和 using 声明、null 合并分配以及只读实例成员。

 [C# 9.0 规范建议](../../../_csharplang/proposals/csharp-9.0/records.md)  
 C# 9.0 可用于 .NET 5.0。 这些功能包括记录、顶层语句、模式匹配增强、仅 init 资源库、目标类型的新表达式、模块初始化表达式、扩展部分方法、静态匿名函数、目标类型的条件表达式、协变返回类型、foreach 循环中的 GetEnumerator 扩展、Lambda 丢弃参数、本地函数的属性、本机大小的整数、函数指针、禁止发出 localsinit 标志和无约束类型参数注释。

## <a name="related-sections"></a>相关章节  

 [使用 Visual Studio C# 开发环境](/visualstudio/get-started/csharp)  
 提供指向介绍 IDE 和编辑器的概念性和任务主题的链接。  
  
 [C# 编程指南](../programming-guide/index.md)  
 包括有关如何使用 C# 编程语言的信息。
