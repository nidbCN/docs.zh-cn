---
title: 指针类型 - C# 编程指南
description: 了解指针类型。 参阅不同指针的示例、代码示例，并查看其他可用资源。
ms.date: 04/20/2018
helpviewer_keywords:
- unsafe code [C#], pointers
- pointers [C#]
ms.openlocfilehash: 0e384084ef1fbb9139c11880ffa8a79bad23b32e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480578"
---
# <a name="pointer-types-c-programming-guide"></a>指针类型（C# 编程指南）

在不安全的上下文中，类型可以是指针类型、值类型或引用类型。 指针类型声明采用下列形式之一：

``` csharp
type* identifier;
void* identifier; //allowed but not recommended
```

在指针类型中的 `*` 之前指定的类型被称为“referent 类型”  。 只有[非托管类型](../../language-reference/builtin-types/unmanaged-types.md)可为引用类型。

指针类型不从[对象](../../language-reference/builtin-types/reference-types.md)继承，并且指针类型与 `object` 之间不存在转换。 此外，装箱和取消装箱不支持指针。 但是，你可在不同的指针类型之间以及指针类型和整型之间进行转换。

在同一个声明中声明多个指针时，星号 (*) 仅与基础类型一起写入；而不是用作每个指针名称的前缀。 例如：

```csharp
int* p1, p2, p3;   // Ok
int *p1, *p2, *p3;   // Invalid in C#
```

指针不能指向引用或包含引用的[结构](../../language-reference/builtin-types/struct.md)，因为无法对对象引用进行垃圾回收，即使有指针指向它也是如此。 垃圾回收器并不跟踪是否有任何类型的指针指向对象。

`myType*` 类型的指针变量的值为 `myType` 类型的变量的地址。 下面是指针类型声明的示例：

|示例|描述|
|-------------|-----------------|
|`int* p`|`p` 是指向整数的指针。|
|`int** p`|`p` 是指向整数的指针的指针。|
|`int*[] p`|`p` 是指向整数的指针的一维数组。|
|`char* p`|`p` 是指向字符的指针。|
|`void* p`|`p` 是指向未知类型的指针。|

指针间接寻址运算符 * 可用于访问位于指针变量所指向的位置的内容。 例如，请考虑以下声明：

```csharp
int* myVariable;
```

表达式 `*myVariable` 表示在 `int` 中包含的地址处找到的 `myVariable` 变量。

[fixed 语句](../../language-reference/keywords/fixed-statement.md)和[指针转换](./pointer-conversions.md)主题中有几个指针示例。 下面的示例使用 `unsafe` 关键字和 `fixed` 语句，并显示如何递增内部指针。  你可将此代码粘贴到控制台应用程序的 Main 函数中来运行它。 这些示例必须使用 [AllowUnsafeBlocks](../../language-reference/compiler-options/language.md#allowunsafeblocks) 编译器选项集进行编译。

[!code-csharp[Using pointer types](snippets/FixedKeywordExamples.cs#5)]

你无法对 `void*` 类型的指针应用间接寻址运算符。 但是，你可以使用强制转换将 void 指针转换为任何其他指针类型，反之亦然。

指针可以为 `null`。 将间接寻址运算符应用于 null 指针将导致由实现定义的行为。

在方法之间传递指针会导致未定义的行为。 考虑这种方法，该方法通过 `in`、`out` 或 `ref` 参数或作为函数结果返回一个指向局部变量的指针。 如果已在固定块中设置指针，则它指向的变量不再是固定的。

下表列出了可在不安全的上下文中对指针执行的运算符和语句：

|运算符/语句|使用|
|-------------------------|---------|
|`*`|执行指针间接寻址。|
|`->`|通过指针访问结构的成员。|
|`[]`|为指针建立索引。|
|`&`|获取变量的地址。|
|`++` 和 `--`|递增和递减指针。|
|`+` 和 `-`|执行指针算法。|
|`==`、`!=`、`<`、`>`、`<=` 和 `>=`|比较指针。|
|[`stackalloc`](../../language-reference/operators/stackalloc.md)|在堆栈上分配内存。|
|[`fixed` 语句](../../language-reference/keywords/fixed-statement.md)|临时固定变量以便找到其地址。|

要详细了解与指针相关的运算符，请参阅[与指针相关的运算符](../../language-reference/operators/pointer-related-operators.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[指针类型](~/_csharplang/spec/unsafe-code.md#pointer-types)部分。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [不安全代码和指针](index.md)
- [指针转换](pointer-conversions.md)
- [引用类型](../../language-reference/keywords/reference-types.md)
- [值类型](../../language-reference/builtin-types/value-types.md)
- [unsafe](../../language-reference/keywords/unsafe.md)
