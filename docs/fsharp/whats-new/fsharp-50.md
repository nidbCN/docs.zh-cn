---
title: 'F # 5.0 中的新增功能-F # 指南'
description: '获取 F # 5.0 中提供的新功能的概述。'
ms.date: 11/06/2020
ms.openlocfilehash: c686bcf5df18d24ac35bbafb2b2d90f768ef7947
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876703"
---
# <a name="whats-new-in-f-50"></a>F# 5.0 中的新增功能

F # 5.0 向 F # 语言和 F# 交互窗口增加了几项改进。 它随 **.net 5** 一起发布。

可以从 [.NET 下载页](https://dotnet.microsoft.com/download)下载最新 .NET SDK。

## <a name="get-started"></a>入门

F # 5.0 适用于所有 .NET Core 分发版和 Visual Studio 工具。 有关详细信息，请参阅 [F # 入门](../get-started/index.md) 以了解详细信息。

## <a name="package-references-in-f-scripts"></a>F # 脚本中的包引用

F # 5 通过语法为 F # 脚本中的包引用提供支持 `#r "nuget:..."` 。 例如，请考虑下面的包引用：

```fsharp
#r "nuget: Newtonsoft.Json"

open Newtonsoft.Json

let o = {| X = 2; Y = "Hello" |}

printfn $"{JsonConvert.SerializeObject o}"
```

你还可以在包名称后提供显式版本，如下所示：

```fsharp
#r "nuget: Newtonsoft.Json,11.0.1"
```

包引用支持具有本机依赖项的包，如 ML.NET。

包引用还支持具有有关引用依赖项的特殊要求的包 `.dll` 。 例如， [FParsec](https://www.nuget.org/packages/FParsec/) 包，用于要求用户 `FParsecCS.dll` 在 F# 交互窗口引用之前手动确保引用其依赖项 `FParsec.dll` 。 不再需要此操作，你可以引用包，如下所示：

```fsharp
#r "nuget: FParsec"

open FParsec

let test p str =
    match run p str with
    | Success(result, _, _)   -> printfn $"Success: {result}"
    | Failure(errorMsg, _, _) -> printfn $"Failure: {errorMsg}"

test pfloat "1.234"
```

此功能实现 [F # 工具 RFC FST-1027](https://github.com/fsharp/fslang-design/blob/master/tooling/FST-1027-fsi-references.md)。 有关包引用的详细信息，请参阅 [F# 交互窗口](../tools/fsharp-interactive/index.md) 教程。

## <a name="string-interpolation"></a>字符串内插

F # 内插字符串非常类似于 c # 或 JavaScript 内插字符串，因为它们允许您在字符串文字中的 "洞" 中编写代码。 下面是一个基本示例：

```fsharp
let name = "Phillip"
let age = 29
printfn $"Name: {name}, Age: {age}"

printfn $"I think {3.0 + 0.14} is close to {System.Math.PI}!"
```

但是，F # 内插字符串也允许类型化的内插，就像 `sprintf` 函数一样，强制插入上下文内的表达式符合特定类型。 它使用相同的格式说明符。

```fsharp
let name = "Phillip"
let age = 29

printfn $"Name: %s{name}, Age: %d{age}"

// Error: type mismatch
printfn $"Name: %s{age}, Age: %d{name}"
```

在前面的类型化内插示例中， `%s` 要求内插为类型 `string` ，而 `%d` 需要将内插成为 `integer` 。

此外，任何 (或表达式) 任意 F # 表达式都可以置于内插上下文的一边。 甚至可以编写更复杂的表达式，如下所示：

```fsharp
let str =
    $"""The result of squaring each odd item in {[1..10]} is:
{
    let square x = x * x
    let isOdd x = x % 2 <> 0
    let oddSquares xs =
        xs
        |> List.filter isOdd
        |> List.map square
    oddSquares [1..10]
}
"""
```

尽管我们不建议在实践中执行此操作。

此功能实现 [F # RFC FS-1001](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1001-StringInterpolation.md)。

## <a name="support-for-nameof"></a>对 nameof 的支持

F # 5 支持 `nameof` 运算符，该运算符解析其正用于的符号，并在 F # 源中生成其名称。 这在各种情况下非常有用，例如日志记录，并可防止日志记录在源代码中发生更改。

```fsharp
let months =
    [
        "January"; "February"; "March"; "April";
        "May"; "June"; "July"; "August"; "September";
        "October"; "November"; "December"
    ]

let lookupMonth month =
    if (month > 12 || month < 1) then
        invalidArg (nameof month) (sprintf "Value passed in was %d." month)

    months.[month-1]

printfn $"{lookupMonth 12}"
printfn $"{lookupMonth 1}"
printfn $"{lookupMonth 13}"
```

最后一行将引发异常，并且错误消息中将显示 "month"。

你可以创建几乎每个 F # 构造的名称：

```fsharp
module M =
    let f x = nameof x

printfn $"{M.f 12}"
printfn $"{nameof M}"
printfn $"{nameof M.f}"
```

三个最终添加操作对运算符的工作方式进行了更改：添加了 `nameof<'type-parameter>` 泛型类型参数的窗体，以及在 `nameof` 模式匹配表达式中用作模式的功能。

采用运算符的名称将提供其源字符串。 如果需要编译的窗体，请使用运算符的编译名称：

```fsharp
nameof(+) // "+"
nameof op_Addition // "op_Addition"
```

采用类型参数的名称需要略有不同的语法：

```fsharp
type C<'TType> =
    member _.TypeName = nameof<'TType>
```

这类似于 `typeof<'T>` 和 `typedefof<'T>` 运算符。

F # 5 还添加了对 `nameof` 可在表达式中使用的模式的支持 `match` ：

```fsharp
[<Struct; IsByRefLike>]
type RecordedEvent = { EventType: string; Data: ReadOnlySpan<byte> }

type MyEvent =
    | AData of int
    | BData of string

let deserialize (e: RecordedEvent) : MyEvent =
    match e.EventType with
    | nameof AData -> AData (JsonSerializer.Deserialize<int> e.Data)
    | nameof BData -> BData (JsonSerializer.Deserialize<string> e.Data)
    | t -> failwithf "Invalid EventType: %s" t
```

前面的代码使用 "nameof" 而不是 match 表达式中的字符串文字。

此功能实现 [F # RFC FS-1003](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1003-nameof-operator.md)。

## <a name="open-type-declarations"></a>开放式类型声明

F # 5 还添加了对开放类型声明的支持。 开放式类型声明类似于打开 c # 中的静态类，但有一些不同的语法和一些略有不同的行为，以适合 F # 语义。

利用开放类型声明，你可以在 `open` 任何类型中公开其静态内容。 此外，还可以通过 `open` F # 定义的联合和记录来公开其内容。 例如，如果在模块中定义了联合并想要访问其事例，但又不希望打开整个模块，则这会很有用。

```fsharp
open type System.Math

let x = Min(1.0, 2.0)

module M =
    type DU = A | B | C

    let someOtherFunction x = x + 1

// Open only the type inside the module
open type M.DU

printfn $"{A}"
```

与 c # 不同，当你 `open type` 在两个公开同名成员的类型上时，最后一个类型中的成员将 `open` 隐藏另一个名称。 这与围绕已存在的隐藏的 F # 语义一致。

此功能实现 [F # RFC FS-1068](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1068-open-type-declaration.md)。

## <a name="consistent-slicing-behavior-for-built-in-data-types"></a>内置数据类型的一致切片行为

用于切分内置数据类型的行为， `FSharp.Core` (数组、列表、string、二维数组、三维数组、4d 数组) 在 F # 5 之前用于不一致。 有些边缘情况行为引发了异常，有些则是。 在 F # 5 中，所有内置类型现在为无法生成的切片返回空切片：

```fsharp
let l = [ 1..10 ]
let a = [| 1..10 |]
let s = "hello!"

// Before: would return empty list
// F# 5: same
let emptyList = l.[-2..(-1)]

// Before: would throw exception
// F# 5: returns empty array
let emptyArray = a.[-2..(-1)]

// Before: would throw exception
// F# 5: returns empty string
let emptyString = s.[-2..(-1)]
```

此功能实现 [F # RFC FS-1077](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1077-tolerant-slicing.md)。

## <a name="fixed-index-slices-for-3d-and-4d-arrays-in-fsharpcore"></a>为 Fsharp.core 中的3D 和4D 阵列固定索引切片

F # 5.0 为内置3D 和4D 数组类型中具有固定索引的切片提供支持。

为了说明这一点，请考虑下面的三维数组：

*z = 0*

| x\y   | 0 | 1 |
|-------|---|---|
| **0** | 0 | 1 |
| **1** | 2 | 3 |

*z = 1*

| x\y   | 0 | 1 |
|-------|---|---|
| **0** | 4 | 5 |
| **1** | 6 | 7 |

如果要从数组中提取切片，该怎么办 `[| 4; 5 |]` ？ 这现在非常简单！

```fsharp
// First, create a 3D array to slice

let dim = 2
let m = Array3D.zeroCreate<int> dim dim dim

let mutable count = 0

for z in 0..dim-1 do
    for y in 0..dim-1 do
        for x in 0..dim-1 do
            m.[x,y,z] <- count
            count <- count + 1

// Now let's get the [4;5] slice!
m.[*, 0, 1]
```

此功能实现 [F # RFC FS-1077b](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1077-3d-4d-fixed-index-slicing.md)。

## <a name="f-quotations-improvements"></a>F # 报价改进

F # [代码引用](../language-reference/code-quotations.md) 现在能够保留类型约束信息。 请看下面的示例：

```fsharp
open FSharp.Linq.RuntimeHelpers

let eval q = LeafExpressionConverter.EvaluateQuotation q

let inline negate x = -x
// val inline negate: x: ^a ->  ^a when  ^a : (static member ( ~- ) :  ^a ->  ^a)

<@ negate 1.0 @>  |> eval
```

函数生成的约束 `inline` 保留在代码引用中。 `negate`现在可以计算该函数的 quotated 窗体。

此功能实现 [F # RFC FS-1071](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1071-witness-passing-quotations.md)。

## <a name="applicative-computation-expressions"></a>Applicative 计算表达式

目前使用[CEs)  (CEs 的计算表达式](../language-reference/computation-expressions.md)，用于对 "上下文计算" 进行建模，或使用更多功能编程友好术语一元计算。

F # 5 引入了 applicative CEs，后者提供不同的计算模型。 Applicative CEs 允许进行更高效的计算，前提是每个计算都是独立的，并且其结果在结束时累计。 当计算彼此独立时，它们也是完全可并行化，允许 CE 作者编写更高效的库。 不过，这种优势是一项限制：不允许使用依赖于以前计算的值的计算。

下面的示例显示了类型的基本 applicative CE `Result` 。

```fsharp
// First, define a 'zip' function
module Result =
    let zip x1 x2 =
        match x1,x2 with
        | Ok x1res, Ok x2res -> Ok (x1res, x2res)
        | Error e, _ -> Error e
        | _, Error e -> Error e

// Next, define a builder with 'MergeSources' and 'BindReturn'
type ResultBuilder() =
    member _.MergeSources(t1: Result<'T,'U>, t2: Result<'T1,'U>) = Result.zip t1 t2
    member _.BindReturn(x: Result<'T,'U>, f) = Result.map f x

let result = ResultBuilder()

let run r1 r2 r3 =
    // And here is our applicative!
    let res1: Result<int, string> =
        result {
            let! a = r1
            and! b = r2
            and! c = r3
            return a + b - c
        }

    match res1 with
    | Ok x -> printfn $"{nameof res1} is: %d{x}"
    | Error e -> printfn $"{nameof res1} is: {e}"

let printApplicatives () =
    let r1 = Ok 2
    let r2 = Ok 3 // Error "fail!"
    let r3 = Ok 4

    run r1 r2 r3
    run r1 (Error "failure!") r3
```

如果你是目前在其库中公开 CEs 的库作者，需要注意一些其他注意事项。

此功能实现 [F # RFC FS-1063](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1063-support-letbang-andbang-for-applicative-functors.md)。

## <a name="interfaces-can-be-implemented-at-different-generic-instantiations"></a>可在不同的泛型实例化实现接口

你现在可以在不同的泛型实例化上实现相同的接口：

```fsharp
type IA<'T> =
    abstract member Get : unit -> 'T

type MyClass() =
    interface IA<int> with
        member x.Get() = 1
    interface IA<string> with
        member x.Get() = "hello"

let mc = MyClass()
let iaInt = mc :> IA<int>
let iaString = mc :> IA<string>

iaInt.Get() // 1
iaString.Get() // "hello"
```

此功能实现 [F # RFC FS-1031](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1031-Allow%20implementing%20the%20same%20interface%20at%20different%20generic%20instantiations%20in%20the%20same%20type.md)。

## <a name="default-interface-member-consumption"></a>默认接口成员消耗

F # 5 允许使用 [具有默认实现的接口](../../csharp/whats-new/tutorials/default-interface-methods-versions.md)。

请考虑 c # 中定义的接口，如下所示：

```csharp
using System;

namespace CSharp
{
    public interface MyDimasd
    {
        public int Z => 0;
    }
}
```

可以在 F # 中使用它来实现接口的任何标准方法：

```fsharp
open CSharp

// You can implement the interface via a class
type MyType() =
    member _.M() = ()

    interface MyDim

let md = MyType() :> MyDim
printfn $"DIM from C#: %d{md.Z}"

// You can also implement it via an object expression
let md' = { new MyDim }
printfn $"DIM from C# but via Object Expression: %d{md'.Z}"
```

这使你能够在预期用户能够使用默认实现时，安全地利用在现代 c # 中编写的 c # 代码和 .NET 组件。

此功能实现 [F # RFC FS-1074](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1074-default-interface-member-consumption.md)。

## <a name="simplified-interop-with-nullable-value-types"></a>具有可以为 null 的值类型的简化互操作

[可以为 null 的 (值) 类型](/dotnet/api/system.nullable-1) (早于 F # 支持的名称) 可以为 null 的类型，但与它们进行交互几乎有点困难，因为 `Nullable` `Nullable<SomeType>` 每次要传递值时都必须构造或包装。 现在， `Nullable<ThatValueType>` 如果目标类型匹配，则编译器会将值类型隐式转换为。 现在可以执行以下代码：

```fsharp
#r "nuget: Microsoft.Data.Analysis"

open Microsoft.Data.Analysis

let dateTimes = PrimitiveDataFrameColumn<DateTime>("DateTimes")

// The following line used to fail to compile
dateTimes.Append(DateTime.Parse("2019/01/01"))

// The previous line is now equivalent to this line
dateTimes.Append(Nullable<DateTime>(DateTime.Parse("2019/01/01")))
```

此功能实现 [F # RFC FS-1075](https://github.com/fsharp/fslang-design/blob/master/FSharp-5.0/FS-1075-nullable-interop.md)。

## <a name="preview-reverse-indexes"></a>预览：反向索引

F # 5 还引入了允许反向索引的预览。 语法为 `^idx`。 下面介绍了从列表末尾的元素1值：

```fsharp
let xs = [1..10]

// Get element 1 from the end:
xs.[^1]

// From the end slices

let lastTwoOldStyle = xs.[(xs.Length-2)..]

let lastTwoNewStyle = xs.[^1..]

lastTwoOldStyle = lastTwoNewStyle // true
```

你还可以为自己的类型定义反向索引。 为此，需要实现以下方法：

```fsharp
GetReverseIndex: dimension: int -> offset: int
```

下面是该类型的示例 `Span<'T>` ：

```fsharp
open System

type Span<'T> with
    member sp.GetSlice(startIdx, endIdx) =
        let s = defaultArg startIdx 0
        let e = defaultArg endIdx sp.Length
        sp.Slice(s, e - s)

    member sp.GetReverseIndex(_, offset: int) =
        sp.Length - offset

let printSpan (sp: Span<int>) =
    let arr = sp.ToArray()
    printfn $"{arr}"

let run () =
    let sp = [| 1; 2; 3; 4; 5 |].AsSpan()

    // Pre-# 5.0 slicing on a Span<'T>
    printSpan sp.[0..] // [|1; 2; 3; 4; 5|]
    printSpan sp.[..3] // [|1; 2; 3|]
    printSpan sp.[1..3] // |2; 3|]

    // Same slices, but only using from-the-end index
    printSpan sp.[..^0] // [|1; 2; 3; 4; 5|]
    printSpan sp.[..^2] // [|1; 2; 3|]
    printSpan sp.[^4..^2] // [|2; 3|]

run() // Prints the same thing twice
```

此功能实现 [F # RFC FS-1076](https://github.com/fsharp/fslang-design/blob/master/preview/FS-1076-from-the-end-slicing.md)。

## <a name="preview-overloads-of-custom-keywords-in-computation-expressions"></a>预览：计算表达式中的自定义关键字的重载

计算表达式是库和框架作者的强大功能。 它们允许您定义已知成员并为正在处理的域构建 DSL，从而极大地提高组件的表现力。

F # 5 为计算表达式中的重载自定义操作添加了预览支持。 它允许编写和使用以下代码：

```fsharp
open System

type InputKind =
    | Text of placeholder:string option
    | Password of placeholder: string option

type InputOptions =
  { Label: string option
    Kind : InputKind
    Validators : (string -> bool) array }

type InputBuilder() =
    member t.Yield(_) =
      { Label = None
        Kind = Text None
        Validators = [||] }

    [<CustomOperation("text")>]
    member this.Text(io, ?placeholder) =
        { io with Kind = Text placeholder }

    [<CustomOperation("password")>]
    member this.Password(io, ?placeholder) =
        { io with Kind = Password placeholder }

    [<CustomOperation("label")>]
    member this.Label(io, label) =
        { io with Label = Some label }

    [<CustomOperation("with_validators")>]
    member this.Validators(io, [<ParamArray>] validators) =
        { io with Validators = validators }

let input = InputBuilder()

let name =
    input {
    label "Name"
    text
    with_validators
        (String.IsNullOrWhiteSpace >> not)
    }

let email =
    input {
    label "Email"
    text "Your email"
    with_validators
        (String.IsNullOrWhiteSpace >> not)
        (fun s -> s.Contains "@")
    }

let password =
    input {
    label "Password"
    password "Must contains at least 6 characters, one number and one uppercase"
    with_validators
        (String.exists Char.IsUpper)
        (String.exists Char.IsDigit)
        (fun s -> s.Length >= 6)
    }
```

在进行此更改之前，可以编写 `InputBuilder` 类型，但无法像在示例中使用它那样使用它。 由于允许使用重载、可选参数和 now `System.ParamArray` 类型，因此一切都按您预期的方式工作。

此功能实现 [F # RFC FS-1056](https://github.com/fsharp/fslang-design/blob/master/preview/FS-1056-allow-custom-operation-overloads.md)。
