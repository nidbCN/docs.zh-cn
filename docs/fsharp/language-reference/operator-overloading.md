---
title: 运算符重载
description: '了解如何在 F # 中的类或记录类型和全局级别重载算术运算符。'
ms.date: 08/15/2020
ms.openlocfilehash: 64ff87a8706e6a05754b5328a7d95cd6a2b360c1
ms.sourcegitcommit: 80f38cb67bd02f51d5722fa13d0ea207e3b14a8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105610845"
---
# <a name="operator-overloading"></a>运算符重载

本主题介绍如何在类或记录类型以及全局级别重载算术运算符。

## <a name="syntax"></a>语法

```fsharp
// Overloading an operator as a class or record member.
static member (operator-symbols) (parameter-list) =
    method-body
// Overloading an operator at the global level
let [inline] (operator-symbols) parameter-list = function-body
```

## <a name="remarks"></a>备注

在前面的语法中， *运算符符号* 是 `+` 、、、、等之一 `-` `*` `/` `=` 。 *参数列表* 按操作数在该运算符的常用语法中出现的顺序指定它们。 *方法体* 构造生成的值。

运算符的运算符重载必须是静态的。 一元运算符（如和）的运算符 `+` 重载 `-` 必须使用 `~` *运算符符号* 中的波形符 () ，以指示该运算符是一元运算符而不是二元运算符，如下面的声明所示。

```fsharp
static member (~-) (v : Vector)
```

下面的代码演示一个仅包含两个运算符的矢量类，其中的一个运算符用于一元负运算，而另一个运算符用于标量乘法运算。 在此示例中，需要使用两种标量乘法重载，因为无论矢量和标量出现的顺序如何，运算符都必须有效。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4001.fs)]

## <a name="creating-new-operators"></a>创建新运算符

您可以重载所有标准运算符，但也可以创建新的运算符，而不是特定字符的序列。 允许的运算符字符包括 `!` 、 `$` 、、 `%` `&` 、、 `*` 、 `+` `-` `.` `/` `<` `=` `>` `?` `@` `^` `|` `~` 、、、、、、、、、和。 该 `~` 字符具有使运算符成为一元运算符的特殊含义，并且不是运算符字符序列的一部分。 并非所有运算符都可以成为一元运算符。

根据所使用的确切字符顺序，运算符将具有一定优先级和相关性。 结合性可以从左到右或从右到左，在具有相同优先级的运算符出现在不带括号的序列中时使用。

运算符字符不 `.` 会影响优先级，因此，例如，如果要定义自己的乘法，它具有与普通乘法相同的优先级和相关性，则可以创建等运算符 `.*` 。

仅操作员 `?` 和 `?<-` 可以从开始 `?` 。

`$`运算符必须独立，无需其他符号。

可在 [符号和运算符引用](./symbol-and-operator-reference/index.md)中找到一个表，其中显示 F # 中所有运算符的优先级。

## <a name="overloaded-operator-names"></a>重载运算符名称

当 F # 编译器编译运算符表达式时，它将生成一个方法，该方法具有该运算符的编译器生成的名称。 这是在 Microsoft 中间语言 (MSIL) 为方法以及在反射和 IntelliSense 中显示的名称。 通常不需要在 F # 代码中使用这些名称。

下表显示标准运算符及其对应的生成名称。

|操作员|生成的名称|
|--------|--------------|
|`[]`|`op_Nil`|
|`::`|`op_Cons`|
|`+`|`op_Addition`|
|`-`|`op_Subtraction`|
|`*`|`op_Multiply`|
|`/`|`op_Division`|
|`@`|`op_Append`|
|`^`|`op_Concatenate`|
|`%`|`op_Modulus`|
|`&&&`|`op_BitwiseAnd`|
|<code>&#124;&#124;&#124;</code>|`op_BitwiseOr`|
|`^^^`|`op_ExclusiveOr`|
|`<<<`|`op_LeftShift`|
|`~~~`|`op_LogicalNot`|
|`>>>`|`op_RightShift`|
|`~+`|`op_UnaryPlus`|
|`~-`|`op_UnaryNegation`|
|`=`|`op_Equality`|
|`<=`|`op_LessThanOrEqual`|
|`>=`|`op_GreaterThanOrEqual`|
|`<`|`op_LessThan`|
|`>`|`op_GreaterThan`|
|`?`|`op_Dynamic`|
|`?<-`|`op_DynamicAssignment`|
|<code>&#124;></code>|`op_PipeRight`|
|<code><&#124;</code>|`op_PipeLeft`|
|`!`|`op_Dereference`|
|`>>`|`op_ComposeRight`|
|`<<`|`op_ComposeLeft`|
|`<@ @>`|`op_Quotation`|
|`<@@ @@>`|`op_QuotationUntyped`|
|`+=`|`op_AdditionAssignment`|
|`-=`|`op_SubtractionAssignment`|
|`*=`|`op_MultiplyAssignment`|
|`/=`|`op_DivisionAssignment`|
|`..`|`op_Range`|
|`.. ..`|`op_RangeStep`|

请注意， `not` F # 中的运算符不发出， `op_Inequality` 因为它不是符号运算符。 它是发出对布尔表达式求反的 IL 的函数。

此处未列出的运算符字符的其他组合可用作运算符，其名称由下表中各个字符的名称连接而成。 例如 +！  变为 `op_PlusBang`。

|运算符字符|名称|
|------------------|----|
|`>`|`Greater`|
|`<`|`Less`|
|`+`|`Plus`|
|`-`|`Minus`|
|`*`|`Multiply`|
|`/`|`Divide`|
|`=`|`Equals`|
|`~`|`Twiddle`|
|`$`|`Dollar`|
|`%`|`Percent`|
|`.`|`Dot`|
|`&`|`Amp`|
|<code>&#124;</code>|`Bar`|
|`@`|`At`|
|`^`|`Hat`|
|`!`|`Bang`|
|`?`|`Qmark`|
|`(`|`LParen`|
|`,`|`Comma`|
|`)`|`RParen`|
|`[`|`LBrack`|
|`]`|`RBrack`|

## <a name="prefix-and-infix-operators"></a>前缀和中缀运算符

*前缀* 运算符应放置在一个或多个操作数前面，这与函数非常类似。 应将中 *缀* 运算符放置在两个操作数之间。

只有某些运算符可以用作前缀运算符。 一些运算符始终是前缀运算符，其他运算符可以是中缀或前缀，其余运算符始终是中缀运算符。 除了 `!` 之外，以 `!=` 开头的运算符以及运算符 `~` 或 `~` 的重复序列始终为前缀运算符。 运算符、、、、、、 `+` `-` `+.` `-.` `&` `&&` `%` 和 `%%` 可以是前缀运算符或中缀运算符。 在定义时，可以通过 `~` 在前缀运算符开头添加来区分这些运算符的前缀版本。 `~`仅当定义了运算符时，才使用。

## <a name="example"></a>示例

下面的代码演示如何使用运算符重载来实现分数类型。 分数由分子和分母表示。 函数 `hcf` 用于确定最大公因数，用来减少分数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4002.fs)]

**输出：**

```console
3/4 + 1/2 = 5/4
3/4 - 1/2 = 1/4
3/4 * 1/2 = 3/8
3/4 / 1/2 = 3/2
3/4 + 1 = 7/4
```

## <a name="operators-at-the-global-level"></a>全局级别的运算符

您还可以在全局级别定义运算符。 下面的代码定义运算符 `+?` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4003.fs)]

上述代码的输出为 `12` 。

可以通过这种方式重定义常规算术运算符，因为 F # 的作用域规则规定新定义的运算符优先于内置运算符。

关键字 `inline` 通常与全局运算符一起使用，全局运算符通常是最能集成到调用代码中的小函数。 通过使 operator 函数成为内联函数，它们可以使用静态解析的类型参数来生成静态解析的泛型代码。 有关详细信息，请参阅 [内联函数](./functions/inline-functions.md) 和 [静态解析的类型参数](./generics/statically-resolved-type-parameters.md)。

## <a name="see-also"></a>另请参阅

- [成员](./members/index.md)
