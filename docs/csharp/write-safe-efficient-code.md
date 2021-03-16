---
title: 编写安全有效的 C# 代码
description: 通过 C# 语言最新增强功能，可以编写可验证的安全代码，这些代码的性能以前与不安全代码相关联。
ms.date: 03/17/2020
ms.technology: csharp-advanced-concepts
ms.custom: mvc
ms.openlocfilehash: b739a4ce1f723798cbe50ef9eae673494996751c
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102106613"
---
# <a name="write-safe-and-efficient-c-code"></a><span data-ttu-id="687e1-103">编写安全有效的 C# 代码</span><span class="sxs-lookup"><span data-stu-id="687e1-103">Write safe and efficient C# code</span></span>

<span data-ttu-id="687e1-104">借助 C# 中的新增功能可编写性能更好的可验证安全代码。</span><span class="sxs-lookup"><span data-stu-id="687e1-104">New features in C# enable you to write verifiable safe code with better performance.</span></span> <span data-ttu-id="687e1-105">若仔细地应用这些技术，则需要不安全代码的方案更少。</span><span class="sxs-lookup"><span data-stu-id="687e1-105">If you carefully apply these techniques, fewer scenarios require unsafe code.</span></span> <span data-ttu-id="687e1-106">利用这些功能，可更轻易地将对值类型的引用用作方法参数和方法返回。</span><span class="sxs-lookup"><span data-stu-id="687e1-106">These features make it easier to use references to value types as method arguments and method returns.</span></span> <span data-ttu-id="687e1-107">安全完成后，这些技术可以最大程度地减少值类型的复制操作。</span><span class="sxs-lookup"><span data-stu-id="687e1-107">When done safely, these techniques minimize copying value types.</span></span> <span data-ttu-id="687e1-108">通过使用值类型，可以使分配和垃圾回收过程的数量降至最低。</span><span class="sxs-lookup"><span data-stu-id="687e1-108">By using value types, you can minimize the number of allocations and garbage collection passes.</span></span>

<span data-ttu-id="687e1-109">本文中的很多示例代码都使用了 C# 7.2 中增加的功能。</span><span class="sxs-lookup"><span data-stu-id="687e1-109">Much of the sample code in this article uses features added in C# 7.2.</span></span> <span data-ttu-id="687e1-110">要使用这些功能，必须将项目配置为使用 C# 7.2 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="687e1-110">To use those features, you must configure your project to use C# 7.2 or later.</span></span> <span data-ttu-id="687e1-111">有关设置语言版本的详细信息，请参阅[配置语言版本](language-reference/configure-language-version.md)。</span><span class="sxs-lookup"><span data-stu-id="687e1-111">For more information on setting the language version, see [configure the language version](language-reference/configure-language-version.md).</span></span>

<span data-ttu-id="687e1-112">本文重点介绍有效资源管理的技术。</span><span class="sxs-lookup"><span data-stu-id="687e1-112">This article focuses on techniques for efficient resource management.</span></span> <span data-ttu-id="687e1-113">使用值类型的优点之一是通常可避免堆分配。</span><span class="sxs-lookup"><span data-stu-id="687e1-113">One advantage to using value types is that they often avoid heap allocations.</span></span> <span data-ttu-id="687e1-114">缺点是它们按值进行复制。</span><span class="sxs-lookup"><span data-stu-id="687e1-114">The disadvantage is that they're copied by value.</span></span> <span data-ttu-id="687e1-115">由于存在这种折衷，因此难以优化针对大量数据执行的算法。</span><span class="sxs-lookup"><span data-stu-id="687e1-115">This trade-off makes it harder to optimize algorithms that operate on large amounts of data.</span></span> <span data-ttu-id="687e1-116">C# 7.2 中新增的语言功能提供了可使用对值类型的引用来实现安全高效代码的机制。</span><span class="sxs-lookup"><span data-stu-id="687e1-116">New language features in C# 7.2 provide mechanisms that enable safe efficient code using references to value types.</span></span> <span data-ttu-id="687e1-117">请恰当地使用这些功能，以最大程度地减少分配和复制操作。</span><span class="sxs-lookup"><span data-stu-id="687e1-117">Use these features wisely to minimize both allocations and copy operations.</span></span> <span data-ttu-id="687e1-118">本文将介绍这些新功能。</span><span class="sxs-lookup"><span data-stu-id="687e1-118">This article explores those new features.</span></span>

<span data-ttu-id="687e1-119">本文重点介绍以下资源管理技术：</span><span class="sxs-lookup"><span data-stu-id="687e1-119">This article focuses on the following resource management techniques:</span></span>

- <span data-ttu-id="687e1-120">声明一个 [`readonly struct`](language-reference/builtin-types/struct.md#readonly-struct) 以表示类型是不可变的  。</span><span class="sxs-lookup"><span data-stu-id="687e1-120">Declare a [`readonly struct`](language-reference/builtin-types/struct.md#readonly-struct) to express that a type is **immutable**.</span></span> <span data-ttu-id="687e1-121">这使编译器可以在使用 [`in`](language-reference/keywords/in-parameter-modifier.md) 参数时保存防御性副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-121">That enables the compiler to save defensive copies when using [`in`](language-reference/keywords/in-parameter-modifier.md) parameters.</span></span>
- <span data-ttu-id="687e1-122">如果类型是可变的，请声明 `struct` 成员 [`readonly`](language-reference/builtin-types/struct.md#readonly-instance-members)，以指示该成员不修改状态。</span><span class="sxs-lookup"><span data-stu-id="687e1-122">If a type can't be immutable, declare `struct` members [`readonly`](language-reference/builtin-types/struct.md#readonly-instance-members) to indicate that the member doesn't modify state.</span></span>
- <span data-ttu-id="687e1-123">当返回值 `struct` 大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 且存储生存期大于返回值的方法时，请使用 [`ref readonly`](language-reference/keywords/ref.md#reference-return-values) 返回。</span><span class="sxs-lookup"><span data-stu-id="687e1-123">Use a [`ref readonly`](language-reference/keywords/ref.md#reference-return-values) return when the return value is a `struct` larger than <xref:System.IntPtr.Size?displayProperty=nameWithType> and the storage lifetime is greater than the method returning the value.</span></span>
- <span data-ttu-id="687e1-124">当 `readonly struct` 的大小大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 时，出于性能原因，应将其作为 `in` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="687e1-124">When the size of a `readonly struct` is bigger than <xref:System.IntPtr.Size?displayProperty=nameWithType>, you should pass it as an `in` parameter for performance reasons.</span></span>
- <span data-ttu-id="687e1-125">除非使用 `readonly` 修饰符声明 `struct`或方法仅调用该结构的 `readonly` 成员，否则切勿将其作为 `in` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="687e1-125">Never pass a `struct` as an `in` parameter unless it's declared with the `readonly` modifier or the method calls only `readonly` members of the struct.</span></span> <span data-ttu-id="687e1-126">不遵守该指南可能会对性能产生负面影响，并可能导致不明确的行为。</span><span class="sxs-lookup"><span data-stu-id="687e1-126">Violating this guidance may negatively affect performance and could lead to an obscure behavior.</span></span>
- <span data-ttu-id="687e1-127">使用 [`ref struct`](language-reference/builtin-types/struct.md#ref-struct) 或 `readonly ref struct`（例如 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601>）将内存用作字节序列。</span><span class="sxs-lookup"><span data-stu-id="687e1-127">Use a [`ref struct`](language-reference/builtin-types/struct.md#ref-struct), or a `readonly ref struct` such as <xref:System.Span%601> or <xref:System.ReadOnlySpan%601> to work with memory as a sequence of bytes.</span></span>

<span data-ttu-id="687e1-128">这些技术迫使你在“引用”和“值”方面平衡两个相互竞争的目标   。</span><span class="sxs-lookup"><span data-stu-id="687e1-128">These techniques force you to balance two competing goals with regard to **references** and **values**.</span></span> <span data-ttu-id="687e1-129">属于[引用类型](programming-guide/types/index.md#reference-types)的变量包含对内存中位置的引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-129">Variables that are [reference types](programming-guide/types/index.md#reference-types) hold a reference to the location in memory.</span></span> <span data-ttu-id="687e1-130">属于[值类型](programming-guide/types/index.md#value-types)的变量直接包含它们的值。</span><span class="sxs-lookup"><span data-stu-id="687e1-130">Variables that are [value types](programming-guide/types/index.md#value-types) directly contain their value.</span></span> <span data-ttu-id="687e1-131">这些差异突出了对管理内存资源非常重要的关键差异。</span><span class="sxs-lookup"><span data-stu-id="687e1-131">These differences highlight the key differences that are important for managing memory resources.</span></span> <span data-ttu-id="687e1-132">通常在将“值类型”传递给方法或从方法返回时将其复制  。</span><span class="sxs-lookup"><span data-stu-id="687e1-132">**Value types** are typically copied when passed to a method or returned from a method.</span></span> <span data-ttu-id="687e1-133">此行为包括在调用值类型的成员时复制 `this` 的值。</span><span class="sxs-lookup"><span data-stu-id="687e1-133">This behavior includes copying the value of `this` when calling members of a value type.</span></span> <span data-ttu-id="687e1-134">副本的成本与类型的大小有关。</span><span class="sxs-lookup"><span data-stu-id="687e1-134">The cost of the copy is related to the size of the type.</span></span> <span data-ttu-id="687e1-135">托管堆上分配了“引用类型”  。</span><span class="sxs-lookup"><span data-stu-id="687e1-135">**Reference types** are allocated on the managed heap.</span></span> <span data-ttu-id="687e1-136">每个新对象都需要一个新的分配，并且随后必须回收。</span><span class="sxs-lookup"><span data-stu-id="687e1-136">Each new object requires a new allocation, and subsequently must be reclaimed.</span></span> <span data-ttu-id="687e1-137">这两种操作都需要花些时间。</span><span class="sxs-lookup"><span data-stu-id="687e1-137">Both these operations take time.</span></span> <span data-ttu-id="687e1-138">将引用类型作为参数传递给方法或从方法返回时，将复制引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-138">The reference is copied when a reference type is passed as an argument to a method or returned from a method.</span></span>

<span data-ttu-id="687e1-139">本文使用以下三维点结构的示例概念来解释这些建议：</span><span class="sxs-lookup"><span data-stu-id="687e1-139">This article uses the following example concept of the 3D-point structure to explain these recommendations:</span></span>

```csharp
public struct Point3D
{
    public double X;
    public double Y;
    public double Z;
}
```

<span data-ttu-id="687e1-140">不同的示例使用该概念的不同实现。</span><span class="sxs-lookup"><span data-stu-id="687e1-140">Different examples use different implementations of this concept.</span></span>

## <a name="declare-readonly-structs-for-immutable-value-types"></a><span data-ttu-id="687e1-141">声明不可变值类型的只读结构</span><span class="sxs-lookup"><span data-stu-id="687e1-141">Declare readonly structs for immutable value types</span></span>

<span data-ttu-id="687e1-142">使用 `readonly` 修饰符声明 `struct` 将通知编译器你的意图是创建不可变类型。</span><span class="sxs-lookup"><span data-stu-id="687e1-142">Declaring a `struct` using the `readonly` modifier informs the compiler that your intent is to create an immutable type.</span></span> <span data-ttu-id="687e1-143">编译器使用以下规则强制执行该设计决策：</span><span class="sxs-lookup"><span data-stu-id="687e1-143">The compiler enforces that design decision with the following rules:</span></span>

- <span data-ttu-id="687e1-144">所有字段成员必须为 `readonly`</span><span class="sxs-lookup"><span data-stu-id="687e1-144">All field members must be `readonly`</span></span>
- <span data-ttu-id="687e1-145">所有属性都必须是只读的，包括自动实现的属性。</span><span class="sxs-lookup"><span data-stu-id="687e1-145">All properties must be read-only, including auto-implemented properties.</span></span>

<span data-ttu-id="687e1-146">这两个规则足以确保 `readonly struct` 的任何成员都不会修改该结构的状态。</span><span class="sxs-lookup"><span data-stu-id="687e1-146">These two rules are sufficient to ensure that no member of a `readonly struct` modifies the state of that struct.</span></span> <span data-ttu-id="687e1-147">`struct` 是不可变的。</span><span class="sxs-lookup"><span data-stu-id="687e1-147">The `struct` is immutable.</span></span> <span data-ttu-id="687e1-148">`Point3D` 结构可以定义为不可变结构，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="687e1-148">The `Point3D` structure could be defined as an immutable struct as shown in the following example:</span></span>

```csharp
readonly public struct ReadonlyPoint3D
{
    public ReadonlyPoint3D(double x, double y, double z)
    {
        this.X = x;
        this.Y = y;
        this.Z = z;
    }

    public double X { get; }
    public double Y { get; }
    public double Z { get; }
}
```

<span data-ttu-id="687e1-149">只要你的设计意图是创建不可变值类型，就请遵循此建议。</span><span class="sxs-lookup"><span data-stu-id="687e1-149">Follow this recommendation whenever your design intent is to create an immutable value type.</span></span> <span data-ttu-id="687e1-150">任何性能改进都是额外权益。</span><span class="sxs-lookup"><span data-stu-id="687e1-150">Any performance improvements are an added benefit.</span></span> <span data-ttu-id="687e1-151">`readonly struct` 清楚地表达了你的设计意图。</span><span class="sxs-lookup"><span data-stu-id="687e1-151">The `readonly struct` clearly expresses your design intent.</span></span>

## <a name="declare-readonly-members-when-a-struct-cant-be-immutable"></a><span data-ttu-id="687e1-152">结构可变时声明 readonly 成员</span><span class="sxs-lookup"><span data-stu-id="687e1-152">Declare readonly members when a struct can't be immutable</span></span>

<span data-ttu-id="687e1-153">在 C# 8.0 及更高版本中，结构类型为可变类型时，应将不会引起变化的成员声明为 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="687e1-153">In C# 8.0 and later, when a struct type is mutable, you should declare members that don't cause mutation to be `readonly`.</span></span> <span data-ttu-id="687e1-154">请考虑其他需要三维点结构的应用程序，但必须支持可变性。</span><span class="sxs-lookup"><span data-stu-id="687e1-154">Consider a different application that needs a 3D point structure, but must support mutability.</span></span> <span data-ttu-id="687e1-155">以下版本的三维点结构仅将 `readonly` 修饰符添加到不修改结构的成员。</span><span class="sxs-lookup"><span data-stu-id="687e1-155">The following version of the 3D point structure adds the `readonly` modifier only to those members that don't modify the structure.</span></span> <span data-ttu-id="687e1-156">当你的设计必须支持某些成员对结构的修改时，但仍然需要对某些成员强制执行只读操作的便利时，请遵循以下示例：</span><span class="sxs-lookup"><span data-stu-id="687e1-156">Follow this example when your design must support modifications to the struct by some members, but you still want the benefits of enforcing readonly on some members:</span></span>

```csharp
public struct Point3D
{
    public Point3D(double x, double y, double z)
    {
        _x = x;
        _y = y;
        _z = z;
    }

    private double _x;
    public double X
    {
        readonly get => _x;
        set => _x = value;
    }

    private double _y;
    public double Y
    {
        readonly get => _y;
        set => _y = value;
    }

    private double _z;
    public double Z
    {
        readonly get => _z;
        set => _z = value;
    }

    public readonly double Distance => Math.Sqrt(X * X + Y * Y + Z * Z);

    public readonly override string ToString() => $"{X}, {Y}, {Z}";
}
```

<span data-ttu-id="687e1-157">前面的示例介绍了可在其中应用 `readonly` 修饰符的许多位置：方法、属性和属性访问器。</span><span class="sxs-lookup"><span data-stu-id="687e1-157">The preceding sample shows many of the locations where you can apply the `readonly` modifier: methods, properties, and property accessors.</span></span> <span data-ttu-id="687e1-158">如果使用自动实现的属性，则编译器会将 `readonly` 修饰符添加到 `get` 访问器以获取读写属性。</span><span class="sxs-lookup"><span data-stu-id="687e1-158">If you use auto-implemented properties, the compiler adds the `readonly` modifier to the `get` accessor for read-write properties.</span></span> <span data-ttu-id="687e1-159">对于仅具有 `get` 访问器的属性，编译器会将 `readonly` 修饰符添加到自动实现的属性声明中。</span><span class="sxs-lookup"><span data-stu-id="687e1-159">The compiler adds the `readonly` modifier to the auto-implemented property declarations for properties with only a `get` accessor.</span></span>

<span data-ttu-id="687e1-160">向不改变状态的成员添加 `readonly` 修饰符有两个相关的好处。</span><span class="sxs-lookup"><span data-stu-id="687e1-160">Adding the `readonly` modifier to members that don't mutate state provides two related benefits.</span></span> <span data-ttu-id="687e1-161">首先，编译器会强制执行你的意图。</span><span class="sxs-lookup"><span data-stu-id="687e1-161">First, the compiler enforces your intent.</span></span> <span data-ttu-id="687e1-162">该成员无法改变结构的状态。</span><span class="sxs-lookup"><span data-stu-id="687e1-162">That member can't mutate the struct's state.</span></span> <span data-ttu-id="687e1-163">其次，访问 `readonly` 成员时，编译器不会创建 `in` 参数的防御性副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-163">Second, the compiler won't create defensive copies of `in` parameters when accessing a `readonly` member.</span></span> <span data-ttu-id="687e1-164">编译器可以安全地进行此优化，因为它可以保证 `readonly` 成员不会修改 `struct`。</span><span class="sxs-lookup"><span data-stu-id="687e1-164">The compiler can make this optimization safely because it guarantees that the `struct` is not modified by a `readonly` member.</span></span>

## <a name="use-ref-readonly-return-statements-for-large-structures-when-possible"></a><span data-ttu-id="687e1-165">尽可能对大型结构使用 `ref readonly return` 语句</span><span class="sxs-lookup"><span data-stu-id="687e1-165">Use `ref readonly return` statements for large structures when possible</span></span>

<span data-ttu-id="687e1-166">当返回的值不是返回方法的本地值时，可以按引用返回值。</span><span class="sxs-lookup"><span data-stu-id="687e1-166">You can return values by reference when the value being returned isn't local to the returning method.</span></span> <span data-ttu-id="687e1-167">按引用返回意味着仅复制引用，而不是结构。</span><span class="sxs-lookup"><span data-stu-id="687e1-167">Returning by reference means that only the reference is copied, not the structure.</span></span> <span data-ttu-id="687e1-168">在以下示例中，`Origin` 属性不能使用 `ref` 返回，因为返回的值是局部变量：</span><span class="sxs-lookup"><span data-stu-id="687e1-168">In the following example, the `Origin` property can't use a `ref` return because the value being returned is a local variable:</span></span>

```csharp
public Point3D Origin => new Point3D(0,0,0);
```

<span data-ttu-id="687e1-169">但是，可以按引用返回以下属性定义，因为返回的值是静态成员：</span><span class="sxs-lookup"><span data-stu-id="687e1-169">However, the following property definition can be returned by reference because the returned value is a static member:</span></span>

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    // Dangerous! returning a mutable reference to internal storage
    public ref Point3D Origin => ref origin;

    // other members removed for space
}
```

<span data-ttu-id="687e1-170">你不希望调用方修改原点，所以应该通过 `ref readonly` 返回值：</span><span class="sxs-lookup"><span data-stu-id="687e1-170">You don't want callers modifying the origin, so you should return the value by `ref readonly`:</span></span>

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    public static ref readonly Point3D Origin => ref origin;

    // other members removed for space
}
```

<span data-ttu-id="687e1-171">通过返回 `ref readonly` 可以保存复制较大的结构并保留内部数据成员的不变性。</span><span class="sxs-lookup"><span data-stu-id="687e1-171">Returning `ref readonly` enables you to save copying larger structures and preserve the immutability of your internal data members.</span></span>

<span data-ttu-id="687e1-172">在调用站点，调用方可以选择将 `Origin` 属性用作 `ref readonly` 或值：</span><span class="sxs-lookup"><span data-stu-id="687e1-172">At the call site, callers make the choice to use the `Origin` property as a `ref readonly` or as a value:</span></span>

[!code-csharp[AssignRefReadonly](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#AssignRefReadonly "Assigning a ref readonly")]

<span data-ttu-id="687e1-173">前面的代码中的第一个分配将创建 `Origin` 常数的副本，并分配该副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-173">The first assignment in the preceding code makes a copy of the `Origin` constant and assigns that copy.</span></span> <span data-ttu-id="687e1-174">第二个将分配引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-174">The second assigns a reference.</span></span> <span data-ttu-id="687e1-175">注意，`readonly` 修饰符必须包含在变量声明中。</span><span class="sxs-lookup"><span data-stu-id="687e1-175">Notice that the `readonly` modifier must be part of the declaration of the variable.</span></span> <span data-ttu-id="687e1-176">无法修改该修饰符引用对象的引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-176">The reference to which it refers can't be modified.</span></span> <span data-ttu-id="687e1-177">尝试执行该操作将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="687e1-177">Attempts to do so result in a compile-time error.</span></span>

<span data-ttu-id="687e1-178">`originReference` 的声明需要 `readonly` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="687e1-178">The `readonly` modifier is required on the declaration of `originReference`.</span></span>

<span data-ttu-id="687e1-179">编译器可强制使调用方不能修改引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-179">The compiler enforces that the caller can't modify the reference.</span></span> <span data-ttu-id="687e1-180">直接分配给该值的尝试会生成编译时错误。</span><span class="sxs-lookup"><span data-stu-id="687e1-180">Attempts to assign the value directly generate a compile-time error.</span></span> <span data-ttu-id="687e1-181">在其他情况下，编译器会分配防御性副本，除非它可安全地使用只读引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-181">In other cases, the compiler allocates a defensive copy unless it can safely use the readonly reference.</span></span> <span data-ttu-id="687e1-182">静态分析规则会确定是否可修改结构。</span><span class="sxs-lookup"><span data-stu-id="687e1-182">Static analysis rules determine if the struct could be modified.</span></span> <span data-ttu-id="687e1-183">当结构为 `readonly struct`，或者成员为结构的 `readonly` 成员时，编译器不会创建防御性副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-183">The compiler doesn't create a defensive copy when the struct is a `readonly struct` or the member is a `readonly` member of the struct.</span></span> <span data-ttu-id="687e1-184">无需使用防御性副本即可将结构作为 `in` 参数进行传递。</span><span class="sxs-lookup"><span data-stu-id="687e1-184">Defensive copies aren't needed to pass the struct as an `in` argument.</span></span>

## <a name="apply-the-in-modifier-to-readonly-struct-parameters-larger-than-systemintptrsize"></a><span data-ttu-id="687e1-185">将 `in` 修饰符应用于大于 `System.IntPtr.Size` 的 `readonly struct` 参数</span><span class="sxs-lookup"><span data-stu-id="687e1-185">Apply the `in` modifier to `readonly struct` parameters larger than `System.IntPtr.Size`</span></span>

<span data-ttu-id="687e1-186">`in` 关键字补充了现有的 `ref` 和 `out` 关键字，以按引用传递参数。</span><span class="sxs-lookup"><span data-stu-id="687e1-186">The `in` keyword complements the existing `ref` and `out` keywords to pass arguments by reference.</span></span> <span data-ttu-id="687e1-187">`in` 关键字指定按引用传递参数，但调用的方法不修改值。</span><span class="sxs-lookup"><span data-stu-id="687e1-187">The `in` keyword specifies passing the argument by reference, but the called method doesn't modify the value.</span></span>

<span data-ttu-id="687e1-188">这一新增功能可提供完整的词汇，以表达你的设计意图。</span><span class="sxs-lookup"><span data-stu-id="687e1-188">This addition provides a full vocabulary to express your design intent.</span></span>
<span data-ttu-id="687e1-189">如果未在方法签名中指定以下任一修饰符，值类型会在传递给调用的方法时进行复制。</span><span class="sxs-lookup"><span data-stu-id="687e1-189">Value types are copied when passed to a called method when you don't specify any of the following modifiers in the method signature.</span></span> <span data-ttu-id="687e1-190">每个修饰符指定变量按引用传递，避免复制操作。</span><span class="sxs-lookup"><span data-stu-id="687e1-190">Each of these modifiers specifies that a variable is passed by reference, avoiding the copy.</span></span> <span data-ttu-id="687e1-191">每个修饰符表达一种不同的意图：</span><span class="sxs-lookup"><span data-stu-id="687e1-191">Each modifier expresses a different intent:</span></span>

- <span data-ttu-id="687e1-192">`out`：此方法设置用作此形参的实参的值。</span><span class="sxs-lookup"><span data-stu-id="687e1-192">`out`: This method sets the value of the argument used as this parameter.</span></span>
- <span data-ttu-id="687e1-193">`ref`：此方法可设置用作此形参的实参的值。</span><span class="sxs-lookup"><span data-stu-id="687e1-193">`ref`: This method may set the value of the argument used as this parameter.</span></span>
- <span data-ttu-id="687e1-194">`in`：此方法不会修改用作此形参的实参的值。</span><span class="sxs-lookup"><span data-stu-id="687e1-194">`in`: This method doesn't modify the value of the argument used as this parameter.</span></span>

<span data-ttu-id="687e1-195">添加 `in` 修饰符，按引用传递参数，并声明设计意图是为了按引用传递参数，避免不必要的复制操作。</span><span class="sxs-lookup"><span data-stu-id="687e1-195">Add the `in` modifier to pass an argument by reference and declare your design intent to pass arguments by reference to avoid unnecessary copying.</span></span> <span data-ttu-id="687e1-196">你不打算修改用作该参数的对象。</span><span class="sxs-lookup"><span data-stu-id="687e1-196">You don't intend to modify the object used as that argument.</span></span>

<span data-ttu-id="687e1-197">这种做法通常可以提高大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 的只读值类型的性能。</span><span class="sxs-lookup"><span data-stu-id="687e1-197">This practice often improves performance for readonly value types that are larger than <xref:System.IntPtr.Size?displayProperty=nameWithType>.</span></span> <span data-ttu-id="687e1-198">对于简单类型（`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal` 和 `bool` 以及 `enum` 类型），任何潜在的性能提升都是极小的。</span><span class="sxs-lookup"><span data-stu-id="687e1-198">For simple types (`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal` and `bool`, and `enum` types), any potential performance gains are minimal.</span></span> <span data-ttu-id="687e1-199">实际上，对于小于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 的类型，使用按引用传递可能会降低性能。</span><span class="sxs-lookup"><span data-stu-id="687e1-199">In fact, performance may degrade by using pass-by-reference for types smaller than <xref:System.IntPtr.Size?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="687e1-200">下面的代码演示了一个方法示例，该方法用于计算三维空间中两点间的距离。</span><span class="sxs-lookup"><span data-stu-id="687e1-200">The following code shows an example of a method that calculates the distance between two points in 3D space.</span></span>

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

<span data-ttu-id="687e1-201">该参数具有两个结构，每个结构包含三个双精度值。</span><span class="sxs-lookup"><span data-stu-id="687e1-201">The arguments are two structures that each contain three doubles.</span></span> <span data-ttu-id="687e1-202">一个双精度值有 8 个字节，所以每个参数有 24 个字节。</span><span class="sxs-lookup"><span data-stu-id="687e1-202">A double is 8 bytes, so each argument is 24 bytes.</span></span> <span data-ttu-id="687e1-203">通过指定 `in` 修饰符，可向这些参数传递 4 字节或 8 字节引用，具体取决于计算机的体系结构。</span><span class="sxs-lookup"><span data-stu-id="687e1-203">By specifying the `in` modifier, you pass a 4 byte or 8-byte reference to those arguments, depending on the architecture of the machine.</span></span> <span data-ttu-id="687e1-204">大小的差异很小，但是当应用程序使用许多不同的值在一个紧凑的循环中调用此方法时，这些差异将累积。</span><span class="sxs-lookup"><span data-stu-id="687e1-204">The difference in size is small, but it adds up when your application calls this method in a tight loop using many different values.</span></span>

<span data-ttu-id="687e1-205">`in` 修饰符还可以通过其他方式补充 `out` 和 `ref`。</span><span class="sxs-lookup"><span data-stu-id="687e1-205">The `in` modifier complements `out` and `ref` in other ways as well.</span></span> <span data-ttu-id="687e1-206">无法创建差异仅为是否具有 `in`、`out` 或 `ref` 的方法的重载。</span><span class="sxs-lookup"><span data-stu-id="687e1-206">You can't create overloads of a method that differ only in the presence of `in`, `out`, or `ref`.</span></span> <span data-ttu-id="687e1-207">这些新规则可扩展始终为 `out` 和 `ref` 参数定义的相同行为。</span><span class="sxs-lookup"><span data-stu-id="687e1-207">These new rules extend the same behavior that had always been defined for `out` and `ref` parameters.</span></span> <span data-ttu-id="687e1-208">与 `out` 和 `ref` 修饰符类似，值类型未装箱，因为应用了 `in` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="687e1-208">Like the `out` and `ref` modifiers, value types aren't boxed because the `in` modifier is applied.</span></span>

<span data-ttu-id="687e1-209">`in` 修饰符可能适用于采用以下参数的任何成员：methods、delegates、lambdas、local functions、indexers 和 operators。</span><span class="sxs-lookup"><span data-stu-id="687e1-209">The `in` modifier may be applied to any member that takes parameters: methods, delegates, lambdas, local functions, indexers, operators.</span></span>

<span data-ttu-id="687e1-210">`in` 实参的另一个功能是可对 `in` 形参的实参使用文本值或常数。</span><span class="sxs-lookup"><span data-stu-id="687e1-210">Another feature of `in` parameters is that you may use literal values or constants for the argument to an `in` parameter.</span></span> <span data-ttu-id="687e1-211">此外，与 `ref` 或 `out` 参数不同，无需在调用站点应用 `in` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="687e1-211">Also, unlike a `ref` or `out` parameter, you don't need to apply the `in` modifier at the call site.</span></span> <span data-ttu-id="687e1-212">下面的代码演示调用 `CalculateDistance` 方法的两个示例。</span><span class="sxs-lookup"><span data-stu-id="687e1-212">The following code shows you two examples of calling the `CalculateDistance` method.</span></span> <span data-ttu-id="687e1-213">第一个示例使用按引用传递的两个本地变量。</span><span class="sxs-lookup"><span data-stu-id="687e1-213">The first uses two local variables passed by reference.</span></span> <span data-ttu-id="687e1-214">第二个示例包含方法调用过程中创建的临时变量。</span><span class="sxs-lookup"><span data-stu-id="687e1-214">The second includes a temporary variable created as part of the method call.</span></span>

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#UseInArgument "Specifying an In argument")]

<span data-ttu-id="687e1-215">有多种方法可让编译器确保强制执行 `in` 参数的只读性质。</span><span class="sxs-lookup"><span data-stu-id="687e1-215">There are several ways in which the compiler enforces the read-only nature of an `in` argument.</span></span>  <span data-ttu-id="687e1-216">首先，调用的方法不能直接分配给 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="687e1-216">First of all, the called method can't directly assign to an `in` parameter.</span></span> <span data-ttu-id="687e1-217">当值类型为 `struct` 时，不能直接分配给 `in` 参数的任何字段。</span><span class="sxs-lookup"><span data-stu-id="687e1-217">It can't directly assign to any field of an `in` parameter when that value is a `struct` type.</span></span> <span data-ttu-id="687e1-218">此外，不能向使用 `ref` 或 `out` 修饰符的任何方法传递 `in` 参数。</span><span class="sxs-lookup"><span data-stu-id="687e1-218">In addition, you can't pass an `in` parameter to any method using the `ref` or `out` modifier.</span></span>
<span data-ttu-id="687e1-219">如果字段为 `struct` 类型且该参数也为 `struct` 类型，则这些规则适用于 `in` 参数的任何字段。</span><span class="sxs-lookup"><span data-stu-id="687e1-219">These rules apply to any field of an `in` parameter, provided the field is a `struct` type and the parameter is also a `struct` type.</span></span> <span data-ttu-id="687e1-220">事实上，如果所有级别的成员访问类型都是 `structs`，则这些规则适用于多层成员访问。</span><span class="sxs-lookup"><span data-stu-id="687e1-220">In fact, these rules apply for multiple layers of member access provided the types at all levels of member access are `structs`.</span></span>
<span data-ttu-id="687e1-221">编译器强制将 `struct` 类型作为 `in` 参数传递，它们的 `struct` 成员用作其他方法的参数时，为只读变量。</span><span class="sxs-lookup"><span data-stu-id="687e1-221">The compiler enforces that `struct` types passed as  `in` arguments and their `struct` members are read-only variables when used as arguments to other methods.</span></span>

<span data-ttu-id="687e1-222">使用 `in` 参数可避免产生复制操作可能产生的性能成本。</span><span class="sxs-lookup"><span data-stu-id="687e1-222">The use of `in` parameters can avoid the potential performance costs of making copies.</span></span> <span data-ttu-id="687e1-223">这不会改变任何方法调用的语义。</span><span class="sxs-lookup"><span data-stu-id="687e1-223">It doesn't change the semantics of any method call.</span></span> <span data-ttu-id="687e1-224">所以不需要在调用站点指定 `in` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="687e1-224">Therefore, you don't need to specify the `in` modifier at the call site.</span></span> <span data-ttu-id="687e1-225">在调用站点省略 `in` 修饰符就会通知编译器你允许它出于以下原因复制参数：</span><span class="sxs-lookup"><span data-stu-id="687e1-225">Omitting the `in` modifier at the call site informs the compiler that it's allowed to make a copy of the argument for the following reasons:</span></span>

- <span data-ttu-id="687e1-226">存在从实参类型到形参类型的隐式转换，但不是标识转换。</span><span class="sxs-lookup"><span data-stu-id="687e1-226">There exists an implicit conversion but not an identity conversion from the argument type to the parameter type.</span></span>
- <span data-ttu-id="687e1-227">该参数是一个表达式，但是没有已知的存储变量。</span><span class="sxs-lookup"><span data-stu-id="687e1-227">The argument is an expression but doesn't have a known storage variable.</span></span>
- <span data-ttu-id="687e1-228">存在的重载因 `in` 是否存在而有所不同。</span><span class="sxs-lookup"><span data-stu-id="687e1-228">An overload exists that differs by the presence or absence of `in`.</span></span> <span data-ttu-id="687e1-229">在这种情况下，按值重载的匹配度会更高。</span><span class="sxs-lookup"><span data-stu-id="687e1-229">In that case, the by value overload is a better match.</span></span>

<span data-ttu-id="687e1-230">在更新现有代码以使用只读引用参数时，这些规则会很有用。</span><span class="sxs-lookup"><span data-stu-id="687e1-230">These rules are useful as you update existing code to use read-only reference arguments.</span></span> <span data-ttu-id="687e1-231">在调用的方法中，可以调用任何使用按值参数的实例方法。</span><span class="sxs-lookup"><span data-stu-id="687e1-231">Inside the called method, you can call any instance method that uses by value parameters.</span></span> <span data-ttu-id="687e1-232">在这些方法中，将创建 `in` 参数的副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-232">In those instances, a copy of the `in` parameter is created.</span></span> <span data-ttu-id="687e1-233">由于编译器可为任何 `in` 参数创建临时变量，因此还可指定任何 `in` 参数的默认值。</span><span class="sxs-lookup"><span data-stu-id="687e1-233">Because the compiler may create a temporary variable for any `in` parameter, you can also specify default values for any `in` parameter.</span></span> <span data-ttu-id="687e1-234">以下代码指定原点（点 0,0）为第二个点的默认值：</span><span class="sxs-lookup"><span data-stu-id="687e1-234">The following code specifies the origin (point 0,0) as the default value for the second point:</span></span>

[!code-csharp[InArgumentDefault](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgumentDefault "Specifying defaults for an in parameter")]

<span data-ttu-id="687e1-235">要强制编译器按引用传递只读参数，请在调用站点的参数上指定 `in` 修饰符，如下列代码所示：</span><span class="sxs-lookup"><span data-stu-id="687e1-235">To force the compiler to pass read-only arguments by reference, specify the `in` modifier on the arguments at the call site, as shown in the following code:</span></span>

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ExplicitInArgument "Specifying an In argument")]

<span data-ttu-id="687e1-236">这样可以更轻松地在大型代码库中采用一段时间的 `in` 参数，从而实现性能提升。</span><span class="sxs-lookup"><span data-stu-id="687e1-236">This behavior makes it easier to adopt `in` parameters over time in large codebases where performance gains are possible.</span></span> <span data-ttu-id="687e1-237">首先，将 `in` 修饰符添加到方法签名。</span><span class="sxs-lookup"><span data-stu-id="687e1-237">You add the `in` modifier to method signatures first.</span></span> <span data-ttu-id="687e1-238">然后，可以在调用站点添加 `in` 修饰符，并创建 `readonly struct` 类型，让编译器避免在更多位置创建 `in` 参数的防御性副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-238">Then, you can add the `in` modifier at call sites and create `readonly struct` types to enable the compiler to avoid creating defensive copies of `in` parameters in more locations.</span></span>

<span data-ttu-id="687e1-239">`in` 参数指定还可用于引用类型或数值。</span><span class="sxs-lookup"><span data-stu-id="687e1-239">The `in` parameter designation can also be used with reference types or numeric values.</span></span> <span data-ttu-id="687e1-240">但是，这两种情况下获得的好处都是最少的（如果有）。</span><span class="sxs-lookup"><span data-stu-id="687e1-240">However, the benefits in both cases are minimal, if any.</span></span>

## <a name="avoid-mutable-structs-as-an-in-argument"></a><span data-ttu-id="687e1-241">避免在 `in` 参数中使用可变结构</span><span class="sxs-lookup"><span data-stu-id="687e1-241">Avoid mutable structs as an `in` argument</span></span>

<span data-ttu-id="687e1-242">上述技术解释了如何通过返回引用和按引用传递值来避免创建副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-242">The techniques described above explain how to avoid copies by returning references and passing values by reference.</span></span> <span data-ttu-id="687e1-243">当参数类型声明为 `readonly struct` 类型时，这些技术最有效。</span><span class="sxs-lookup"><span data-stu-id="687e1-243">These techniques work best when the argument types are declared as `readonly struct` types.</span></span> <span data-ttu-id="687e1-244">否则，编译器必须在许多情况下创建“防御副本”以强制执行任何参数的只读状态  。</span><span class="sxs-lookup"><span data-stu-id="687e1-244">Otherwise, the compiler must create **defensive copies** in many situations to enforce the readonly-ness of any arguments.</span></span> <span data-ttu-id="687e1-245">请考虑下面这个计算三维点到原点距离的示例：</span><span class="sxs-lookup"><span data-stu-id="687e1-245">Consider the following example that calculates the distance of a 3D point from the origin:</span></span>

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

<span data-ttu-id="687e1-246">`Point3D` 结构不是只读结构  。</span><span class="sxs-lookup"><span data-stu-id="687e1-246">The `Point3D` structure is *not* a readonly struct.</span></span> <span data-ttu-id="687e1-247">此方法的主体中有六个不同的属性访问调用。</span><span class="sxs-lookup"><span data-stu-id="687e1-247">There are six different property access calls in the body of this method.</span></span> <span data-ttu-id="687e1-248">在首次检查时，你可能认为这些访问是安全的。</span><span class="sxs-lookup"><span data-stu-id="687e1-248">On first examination, you may have thought these accesses were safe.</span></span> <span data-ttu-id="687e1-249">毕竟，`get` 访问器不应该修改对象的状态。</span><span class="sxs-lookup"><span data-stu-id="687e1-249">After all, a `get` accessor shouldn't modify the state of the object.</span></span> <span data-ttu-id="687e1-250">但是没有强制执行的语言规则。</span><span class="sxs-lookup"><span data-stu-id="687e1-250">But there's no language rule that enforces that.</span></span> <span data-ttu-id="687e1-251">它只是通用约定。</span><span class="sxs-lookup"><span data-stu-id="687e1-251">It's only a common convention.</span></span> <span data-ttu-id="687e1-252">任何类型都可以实现修改内部状态的 `get` 访问器。</span><span class="sxs-lookup"><span data-stu-id="687e1-252">Any type could implement a `get` accessor that modified the internal state.</span></span> <span data-ttu-id="687e1-253">如果没有语言保证，编译器必须在调用任何未标记为 `readonly` 修饰符的成员之前创建参数的临时副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-253">Without some language guarantee, the compiler must create a temporary copy of the argument before calling any member not marked with the `readonly` modifier.</span></span> <span data-ttu-id="687e1-254">在堆栈上创建临时存储，将参数的值复制到临时存储中，并将每个成员访问的值作为 `this` 参数复制到堆栈中。</span><span class="sxs-lookup"><span data-stu-id="687e1-254">The temporary storage is created on the stack, the values of the argument are copied to the temporary storage, and the value is copied to the stack for each member access as the `this` argument.</span></span> <span data-ttu-id="687e1-255">在许多情况下，当参数类型不是 `readonly struct`，并且该方法调用成员未标记为 `readonly` 时，这些副本会降低性能，使得按值传递比按只读引用传递速度更快。</span><span class="sxs-lookup"><span data-stu-id="687e1-255">In many situations, these copies harm performance enough that pass-by-value is faster than pass-by-readonly-reference when the argument type isn't a `readonly struct` and the method calls members that aren't marked `readonly`.</span></span> <span data-ttu-id="687e1-256">如果将不修改结构状态的所有方法标记为 `readonly`，编译器就可以安全地确定不修改结构状态，并且不需要防御性复制。</span><span class="sxs-lookup"><span data-stu-id="687e1-256">If you mark all methods that don't modify the struct state as `readonly`, the compiler can safely determine that the struct state isn't modified, and a defensive copy is not needed.</span></span>

<span data-ttu-id="687e1-257">相反，如果距离计算使用不可变结构 `ReadonlyPoint3D`，则不需要临时对象：</span><span class="sxs-lookup"><span data-stu-id="687e1-257">Instead, if the distance calculation uses the immutable struct, `ReadonlyPoint3D`, temporary objects aren't needed:</span></span>

[!code-csharp[readonlyInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ReadOnlyInArgument "Specifying a readonly in argument")]

<span data-ttu-id="687e1-258">当你调用 `readonly struct` 的成员时，编译器会生成更有效的代码：`this` 引用始终是按引用成员方法传递的 `in` 参数，而不是接收器的副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-258">The compiler generates more efficient code when you call members of a `readonly struct`: The `this` reference, instead of a copy of the receiver, is always an `in` parameter passed by reference to the member method.</span></span> <span data-ttu-id="687e1-259">将 `readonly struct` 用作 `in` 参数时，此优化可以减少复制操作。</span><span class="sxs-lookup"><span data-stu-id="687e1-259">This optimization saves copying when you use a `readonly struct` as an `in` argument.</span></span>

<span data-ttu-id="687e1-260">不应将可为 null 的值类型作为 `in` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="687e1-260">You shouldn't pass a nullable value type as an `in` argument.</span></span> <span data-ttu-id="687e1-261"><xref:System.Nullable%601> 类型未声明为只读结构。</span><span class="sxs-lookup"><span data-stu-id="687e1-261">The <xref:System.Nullable%601> type isn't declared as a read-only struct.</span></span> <span data-ttu-id="687e1-262">这意味着编译器必须为使用参数声明中的 `in` 修饰符传递到方法的任何可以为 null 的值类型参数生成防御性副本。</span><span class="sxs-lookup"><span data-stu-id="687e1-262">That means the compiler must generate defensive copies for any nullable value type argument passed to a method using the `in` modifier on the parameter declaration.</span></span>

<span data-ttu-id="687e1-263">你可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/tree/master/csharp/safe-efficient-code/benchmark)中看到使用 [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) 演示性能差异的示例程序。</span><span class="sxs-lookup"><span data-stu-id="687e1-263">You can see an example program that demonstrates the performance differences using [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) in our [samples repository](https://github.com/dotnet/samples/tree/master/csharp/safe-efficient-code/benchmark) on GitHub.</span></span> <span data-ttu-id="687e1-264">它对按值和按引用传递可变结构与按值和按引用传递不可变结构进行了比较。</span><span class="sxs-lookup"><span data-stu-id="687e1-264">It compares passing a mutable struct by value and by reference with passing an immutable struct by value and by reference.</span></span> <span data-ttu-id="687e1-265">使用不可变结构并按引用传递是最快的。</span><span class="sxs-lookup"><span data-stu-id="687e1-265">The use of the immutable struct and pass by reference is fastest.</span></span>

## <a name="use-ref-struct-types-to-work-with-blocks-or-memory-on-a-single-stack-frame"></a><span data-ttu-id="687e1-266">使用 `ref struct` 类型处理单个堆栈帧上的块或内存</span><span class="sxs-lookup"><span data-stu-id="687e1-266">Use `ref struct` types to work with blocks or memory on a single stack frame</span></span>

<span data-ttu-id="687e1-267">相关语言功能是可声明必须约束为单个堆栈帧的值类型。</span><span class="sxs-lookup"><span data-stu-id="687e1-267">A related language feature is the ability to declare a value type that must be constrained to a single stack frame.</span></span> <span data-ttu-id="687e1-268">此限制可使编译器进行多次优化。</span><span class="sxs-lookup"><span data-stu-id="687e1-268">This restriction enables the compiler to make several optimizations.</span></span> <span data-ttu-id="687e1-269">此功能的主要动机是 <xref:System.Span%601> 和相关结构。</span><span class="sxs-lookup"><span data-stu-id="687e1-269">The primary motivation for this feature was <xref:System.Span%601> and related structures.</span></span> <span data-ttu-id="687e1-270">借助使用 <xref:System.Span%601> 类型的新的和更新后的 .NET API，可通过这些增强功能实现性能改进。</span><span class="sxs-lookup"><span data-stu-id="687e1-270">You'll achieve performance improvements from these enhancements by using new and updated .NET APIs that make use of the <xref:System.Span%601> type.</span></span>

<span data-ttu-id="687e1-271">在使用通过 [`stackalloc`](language-reference/operators/stackalloc.md) 创建的内存或使用互操作 API 中的内存时，可能具有类似要求。</span><span class="sxs-lookup"><span data-stu-id="687e1-271">You may have similar requirements working with memory created using [`stackalloc`](language-reference/operators/stackalloc.md) or when using memory from interop APIs.</span></span> <span data-ttu-id="687e1-272">可针对这些需求定义自己的 `ref struct` 类型。</span><span class="sxs-lookup"><span data-stu-id="687e1-272">You can define your own `ref struct` types for those needs.</span></span>

## <a name="readonly-ref-struct-type"></a><span data-ttu-id="687e1-273">`readonly ref struct` 类型</span><span class="sxs-lookup"><span data-stu-id="687e1-273">`readonly ref struct` type</span></span>

<span data-ttu-id="687e1-274">将结构声明为 `readonly ref` 兼具 `ref struct` 和 `readonly struct` 声明的优点和限制。</span><span class="sxs-lookup"><span data-stu-id="687e1-274">Declaring a struct as `readonly ref` combines the benefits and restrictions of `ref struct` and `readonly struct` declarations.</span></span> <span data-ttu-id="687e1-275">只读跨度所使用的内存仅限于单个堆栈帧，并且只读跨度使用的内存无法进行修改。</span><span class="sxs-lookup"><span data-stu-id="687e1-275">The memory used by the readonly span is restricted to a single stack frame, and the memory used by the readonly span can't be modified.</span></span>

## <a name="conclusions"></a><span data-ttu-id="687e1-276">结论</span><span class="sxs-lookup"><span data-stu-id="687e1-276">Conclusions</span></span>

<span data-ttu-id="687e1-277">使用值类型可最大限度地减少分配操作的数量：</span><span class="sxs-lookup"><span data-stu-id="687e1-277">Using value types minimizes the number of allocation operations:</span></span>

- <span data-ttu-id="687e1-278">值类型的存储是为局部变量和方法参数分配的堆栈。</span><span class="sxs-lookup"><span data-stu-id="687e1-278">Storage for value types is stack allocated for local variables and method arguments.</span></span>
- <span data-ttu-id="687e1-279">作为其他对象成员的值类型的存储被分配为该对象的一部分，而不是作为单独的分配。</span><span class="sxs-lookup"><span data-stu-id="687e1-279">Storage for value types that are members of other objects is allocated as part of that object, not as a separate allocation.</span></span>
- <span data-ttu-id="687e1-280">值类型返回值的存储为堆栈分配。</span><span class="sxs-lookup"><span data-stu-id="687e1-280">Storage for value type return values is stack allocated.</span></span>

<span data-ttu-id="687e1-281">将其与相同情况下的引用类型进行对比：</span><span class="sxs-lookup"><span data-stu-id="687e1-281">Contrast that with reference types in those same situations:</span></span>

- <span data-ttu-id="687e1-282">引用类型的存储是为本地变量和方法参数分配的堆。</span><span class="sxs-lookup"><span data-stu-id="687e1-282">Storage for reference types are heap allocated for local variables and method arguments.</span></span> <span data-ttu-id="687e1-283">引用存储在堆栈中。</span><span class="sxs-lookup"><span data-stu-id="687e1-283">The reference is stored on the stack.</span></span>
- <span data-ttu-id="687e1-284">作为其他对象成员的引用类型的存储在堆上分别进行分配。</span><span class="sxs-lookup"><span data-stu-id="687e1-284">Storage for reference types that are members of other objects are separately allocated on the heap.</span></span> <span data-ttu-id="687e1-285">包含的对象存储引用。</span><span class="sxs-lookup"><span data-stu-id="687e1-285">The containing object stores the reference.</span></span>
- <span data-ttu-id="687e1-286">引用类型返回值的存储是堆分配的。</span><span class="sxs-lookup"><span data-stu-id="687e1-286">Storage for reference type return values is heap allocated.</span></span> <span data-ttu-id="687e1-287">对该存储的引用存储在堆栈中。</span><span class="sxs-lookup"><span data-stu-id="687e1-287">The reference to that storage is stored on the stack.</span></span>

<span data-ttu-id="687e1-288">最小化分配附带权衡。</span><span class="sxs-lookup"><span data-stu-id="687e1-288">Minimizing allocations comes with tradeoffs.</span></span> <span data-ttu-id="687e1-289">当 `struct` 的大小大于引用大小时，可以复制更多内存。</span><span class="sxs-lookup"><span data-stu-id="687e1-289">You copy more memory when the size of the `struct` is larger than the size of a reference.</span></span> <span data-ttu-id="687e1-290">引用通常为 64 位或 32 位，并且取决于目标机器 CPU。</span><span class="sxs-lookup"><span data-stu-id="687e1-290">A reference is typically 64 bits or 32 bits, and depends on the target machine CPU.</span></span>

<span data-ttu-id="687e1-291">这些权衡通常对性能影响最小。</span><span class="sxs-lookup"><span data-stu-id="687e1-291">These tradeoffs generally have minimal performance impact.</span></span> <span data-ttu-id="687e1-292">但是，对于大型结构或大型集合，性能影响会增加。</span><span class="sxs-lookup"><span data-stu-id="687e1-292">However, for large structs or larger collections, the performance impact increases.</span></span> <span data-ttu-id="687e1-293">对于程序的紧密循环和热路径，影响可能很大。</span><span class="sxs-lookup"><span data-stu-id="687e1-293">The impact can be large in tight loops and hot paths for programs.</span></span>

<span data-ttu-id="687e1-294">C# 语言的这些增强功能专为性能关键型算法而设计，在这些算法中，使内存分配最小化是实现必需性能的主要因素。</span><span class="sxs-lookup"><span data-stu-id="687e1-294">These enhancements to the C# language are designed for performance critical algorithms where minimizing memory allocations is a major factor in achieving the necessary performance.</span></span> <span data-ttu-id="687e1-295">你可能会发现，编写代码时不经常使用这些功能。</span><span class="sxs-lookup"><span data-stu-id="687e1-295">You may find that you don't often use these features in the code you write.</span></span> <span data-ttu-id="687e1-296">但是，整个 .NET 中都已采用这些增强功能。</span><span class="sxs-lookup"><span data-stu-id="687e1-296">However, these enhancements have been adopted throughout .NET.</span></span> <span data-ttu-id="687e1-297">随着越来越多的 API 使用这些功能，你会发现应用程序的性能得到提升。</span><span class="sxs-lookup"><span data-stu-id="687e1-297">As more and more APIs make use of these features, you'll see the performance of your applications improve.</span></span>

## <a name="see-also"></a><span data-ttu-id="687e1-298">请参阅</span><span class="sxs-lookup"><span data-stu-id="687e1-298">See also</span></span>

- [<span data-ttu-id="687e1-299">ref 关键字</span><span class="sxs-lookup"><span data-stu-id="687e1-299">ref keyword</span></span>](language-reference/keywords/ref.md)
- [<span data-ttu-id="687e1-300">ref 返回结果和局部变量</span><span class="sxs-lookup"><span data-stu-id="687e1-300">Ref returns and ref locals</span></span>](programming-guide/classes-and-structs/ref-returns.md)
