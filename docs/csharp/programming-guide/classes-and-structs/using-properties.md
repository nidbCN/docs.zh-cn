---
title: 使用属性 - C# 编程指南
description: 这些示例演示如何使用 C# 中的属性。 了解 get 和 set 访问器如何实现读取和写入访问，并了解属性的用法。
ms.date: 07/20/2015
helpviewer_keywords:
- set accessor [C#]
- get accessor [C#]
- properties [C#], about properties
ms.assetid: f7f67b05-0983-4cdb-96af-1855d24c967c
ms.openlocfilehash: 16ff0f02db9640ad8cfe41fce9ce954cb75b4e08
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190328"
---
# <a name="using-properties-c-programming-guide"></a><span data-ttu-id="56f38-104">使用属性（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="56f38-104">Using Properties (C# Programming Guide)</span></span>

<span data-ttu-id="56f38-105">属性结合了字段和方法的多个方面。</span><span class="sxs-lookup"><span data-stu-id="56f38-105">Properties combine aspects of both fields and methods.</span></span> <span data-ttu-id="56f38-106">对于对象的用户来说，属性似乎是一个字段，访问属性需要相同的语法。</span><span class="sxs-lookup"><span data-stu-id="56f38-106">To the user of an object, a property appears to be a field, accessing the property requires the same syntax.</span></span> <span data-ttu-id="56f38-107">对于类的实现者来说，属性是一两个代码块，表示 [get](../../language-reference/keywords/get.md) 访问器和/或 [set](../../language-reference/keywords/set.md) 访问器。</span><span class="sxs-lookup"><span data-stu-id="56f38-107">To the implementer of a class, a property is one or two code blocks, representing a [get](../../language-reference/keywords/get.md) accessor and/or a [set](../../language-reference/keywords/set.md) accessor.</span></span> <span data-ttu-id="56f38-108">读取属性时，执行 `get` 访问器的代码块；向属性赋予新值时，执行 `set` 访问器的代码块。</span><span class="sxs-lookup"><span data-stu-id="56f38-108">The code block for the `get` accessor is executed when the property is read; the code block for the `set` accessor is executed when the property is assigned a new value.</span></span> <span data-ttu-id="56f38-109">将不带 `set` 访问器的属性视为只读。</span><span class="sxs-lookup"><span data-stu-id="56f38-109">A property without a `set` accessor is considered read-only.</span></span> <span data-ttu-id="56f38-110">将不带 `get` 访问器的属性视为只写。</span><span class="sxs-lookup"><span data-stu-id="56f38-110">A property without a `get` accessor is considered write-only.</span></span> <span data-ttu-id="56f38-111">将具有以上两个访问器的属性视为读写。</span><span class="sxs-lookup"><span data-stu-id="56f38-111">A property that has both accessors is read-write.</span></span> <span data-ttu-id="56f38-112">在 C# 9 及更高版本中，可以使用 `init` 访问器代替 `set` 访问器将属性设为只读。</span><span class="sxs-lookup"><span data-stu-id="56f38-112">In C# 9 and later, you can use an `init` accessor instead of a `set` accessor to make the property read-only.</span></span>

<span data-ttu-id="56f38-113">与字段不同，属性不会被归类为变量。</span><span class="sxs-lookup"><span data-stu-id="56f38-113">Unlike fields, properties are not classified as variables.</span></span> <span data-ttu-id="56f38-114">因此，不能将属性作为 [ref](../../language-reference/keywords/ref.md) 或 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数传递。</span><span class="sxs-lookup"><span data-stu-id="56f38-114">Therefore, you cannot pass a property as a [ref](../../language-reference/keywords/ref.md) or [out](../../language-reference/keywords/out-parameter-modifier.md) parameter.</span></span>

<span data-ttu-id="56f38-115">属性具有许多用途：它们可以先验证数据，再允许进行更改；可以在类上透明地公开数据，其中数据实际是从某个其他源（如数据库）检索到的；可以在数据发生更改时采取措施，例如引发事件或更改其他字段的值。</span><span class="sxs-lookup"><span data-stu-id="56f38-115">Properties have many uses: they can validate data before allowing a change; they can transparently expose data on a class where that data is actually retrieved from some other source, such as a database; they can take an action when data is changed, such as raising an event, or changing the value of other fields.</span></span>

<span data-ttu-id="56f38-116">通过依次指定字段的访问级别、属性类型、属性名、声明 `get` 访问器和/或 `set` 访问器的代码块，在类块中声明属性。</span><span class="sxs-lookup"><span data-stu-id="56f38-116">Properties are declared in the class block by specifying the access level of the field, followed by the type of the property, followed by the name of the property, and followed by a code block that declares a `get`-accessor and/or a `set` accessor.</span></span> <span data-ttu-id="56f38-117">例如：</span><span class="sxs-lookup"><span data-stu-id="56f38-117">For example:</span></span>

[!code-csharp[csProgGuideProperties#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#7)]

<span data-ttu-id="56f38-118">此示例中将 `Month` 声明为属性，以便 `set` 访问器可确保 `Month` 值设置在 1 至 12 之间。</span><span class="sxs-lookup"><span data-stu-id="56f38-118">In this example, `Month` is declared as a property so that the `set` accessor can make sure that the `Month` value is set between 1 and 12.</span></span> <span data-ttu-id="56f38-119">`Month` 属性使用私有字段跟踪实际值。</span><span class="sxs-lookup"><span data-stu-id="56f38-119">The `Month` property uses a private field to track the actual value.</span></span> <span data-ttu-id="56f38-120">属性的数据的实际位置通常被称为属性的“后备存储”。</span><span class="sxs-lookup"><span data-stu-id="56f38-120">The real location of a property's data is often referred to as the property's "backing store."</span></span> <span data-ttu-id="56f38-121">属性使用私有字段作为后备存储，这很常见。</span><span class="sxs-lookup"><span data-stu-id="56f38-121">It is common for properties to use private fields as a backing store.</span></span> <span data-ttu-id="56f38-122">将字段被标记为私有，确保只能通过调用属性来对其进行更改。</span><span class="sxs-lookup"><span data-stu-id="56f38-122">The field is marked private in order to make sure that it can only be changed by calling the property.</span></span> <span data-ttu-id="56f38-123">有关公共和专用访问限制的详细信息，请参阅[访问修饰符](./access-modifiers.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-123">For more information about public and private access restrictions, see [Access Modifiers](./access-modifiers.md).</span></span>

<span data-ttu-id="56f38-124">自动实现的属性为简单属性声明提供简化的语法。</span><span class="sxs-lookup"><span data-stu-id="56f38-124">Auto-implemented properties provide simplified syntax for simple property declarations.</span></span> <span data-ttu-id="56f38-125">有关详细信息，请参阅[自动实现的属性](auto-implemented-properties.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-125">For more information, see [Auto-Implemented Properties](auto-implemented-properties.md).</span></span>

## <a name="the-get-accessor"></a><span data-ttu-id="56f38-126">get 访问器</span><span class="sxs-lookup"><span data-stu-id="56f38-126">The get accessor</span></span>

<span data-ttu-id="56f38-127">`get` 访问器的正文类似于方法。</span><span class="sxs-lookup"><span data-stu-id="56f38-127">The body of the `get` accessor resembles that of a method.</span></span> <span data-ttu-id="56f38-128">它必须返回属性类型的值。</span><span class="sxs-lookup"><span data-stu-id="56f38-128">It must return a value of the property type.</span></span> <span data-ttu-id="56f38-129">执行 `get` 访问器等效于读取字段的值。</span><span class="sxs-lookup"><span data-stu-id="56f38-129">The execution of the `get` accessor is equivalent to reading the value of the field.</span></span> <span data-ttu-id="56f38-130">例如，在从 `get` 访问器返回私有变量且已启用优化时，对 `get` 访问器方法的调用由编译器内联，因此不存在方法调用开销。</span><span class="sxs-lookup"><span data-stu-id="56f38-130">For example, when you are returning the private variable from the `get` accessor and optimizations are enabled, the call to the `get` accessor method is inlined by the compiler so there is no method-call overhead.</span></span> <span data-ttu-id="56f38-131">但是，无法内联虚拟 `get` 访问器方法，因为编译器在编译时不知道在运行时实际可调用哪些方法。</span><span class="sxs-lookup"><span data-stu-id="56f38-131">However, a virtual `get` accessor method cannot be inlined because the compiler does not know at compile-time which method may actually be called at run time.</span></span> <span data-ttu-id="56f38-132">以下是一个 `get` 访问器，它返回私有字段 `_name` 的值：</span><span class="sxs-lookup"><span data-stu-id="56f38-132">The following is a `get` accessor that returns the value of a private field `_name`:</span></span>

[!code-csharp[csProgGuideProperties#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#8)]

<span data-ttu-id="56f38-133">引用属性时，除了作为赋值目标外，还调用 `get` 访问器读取属性值。</span><span class="sxs-lookup"><span data-stu-id="56f38-133">When you reference the property, except as the target of an assignment, the `get` accessor is invoked to read the value of the property.</span></span> <span data-ttu-id="56f38-134">例如：</span><span class="sxs-lookup"><span data-stu-id="56f38-134">For example:</span></span>

[!code-csharp[csProgGuideProperties#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#9)]

<span data-ttu-id="56f38-135">`get` 访问器必须以 [return](../../language-reference/keywords/return.md) 或 [throw](../../language-reference/keywords/throw.md) 语句结尾，且控件不能超出访问器正文。</span><span class="sxs-lookup"><span data-stu-id="56f38-135">The `get` accessor must end in a [return](../../language-reference/keywords/return.md) or [throw](../../language-reference/keywords/throw.md) statement, and control cannot flow off the accessor body.</span></span>

<span data-ttu-id="56f38-136">使用 `get` 访问器更改对象的状态是一种糟糕的编程风格。</span><span class="sxs-lookup"><span data-stu-id="56f38-136">It is a bad programming style to change the state of the object by using the `get` accessor.</span></span> <span data-ttu-id="56f38-137">例如，以下访问器将产生在每次访问 `_number` 字段时更改对象状态的副作用。</span><span class="sxs-lookup"><span data-stu-id="56f38-137">For example, the following accessor produces the side effect of changing the state of the object every time that the `_number` field is accessed.</span></span>

[!code-csharp[csProgGuideProperties#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#10)]

<span data-ttu-id="56f38-138">`get` 访问器可以用于返回字段值或计算并返回字段值。</span><span class="sxs-lookup"><span data-stu-id="56f38-138">The `get` accessor can be used to return the field value or to compute it and return it.</span></span> <span data-ttu-id="56f38-139">例如：</span><span class="sxs-lookup"><span data-stu-id="56f38-139">For example:</span></span>

[!code-csharp[csProgGuideProperties#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#11)]

<span data-ttu-id="56f38-140">在上一个代码段中，如果不给 `Name` 属性赋值，它将返回值 `NA`。</span><span class="sxs-lookup"><span data-stu-id="56f38-140">In the previous code segment, if you do not assign a value to the `Name` property, it will return the value `NA`.</span></span>

## <a name="the-set-accessor"></a><span data-ttu-id="56f38-141">set 访问器</span><span class="sxs-lookup"><span data-stu-id="56f38-141">The set accessor</span></span>

<span data-ttu-id="56f38-142">`set` 访问器类似于返回类型为 [void](../../language-reference/builtin-types/void.md) 的方法。</span><span class="sxs-lookup"><span data-stu-id="56f38-142">The `set` accessor resembles a method whose return type is [void](../../language-reference/builtin-types/void.md).</span></span> <span data-ttu-id="56f38-143">它使用名为 `value` 的隐式参数，该参数的类型为属性的类型。</span><span class="sxs-lookup"><span data-stu-id="56f38-143">It uses an implicit parameter called `value`, whose type is the type of the property.</span></span> <span data-ttu-id="56f38-144">在下面的示例中，将 `set` 访问器添加到 `Name` 属性：</span><span class="sxs-lookup"><span data-stu-id="56f38-144">In the following example, a `set` accessor is added to the `Name` property:</span></span>

[!code-csharp[csProgGuideProperties#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#12)]

<span data-ttu-id="56f38-145">向属性赋值时，通过使用提供新值的自变量调用 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="56f38-145">When you assign a value to the property, the `set` accessor is invoked by using an argument that provides the new value.</span></span> <span data-ttu-id="56f38-146">例如：</span><span class="sxs-lookup"><span data-stu-id="56f38-146">For example:</span></span>

[!code-csharp[csProgGuideProperties#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#13)]

<span data-ttu-id="56f38-147">为 `set` 访问器中的本地变量声明使用隐式参数名 `value` 是错误的。</span><span class="sxs-lookup"><span data-stu-id="56f38-147">It is an error to use the implicit parameter name, `value`, for a local variable declaration in a `set` accessor.</span></span>

## <a name="the-init-accessor"></a><span data-ttu-id="56f38-148">init 访问器</span><span class="sxs-lookup"><span data-stu-id="56f38-148">The init accessor</span></span>

<span data-ttu-id="56f38-149">用于创建 `init` 访问器的代码与用于创建 `set` 访问器的代码相同，只不过前者使用的关键字是 `init` 而不是 `set`。</span><span class="sxs-lookup"><span data-stu-id="56f38-149">The code to create an `init` accessor is the same as the code to create a `set` accessor except that you use the `init` keyword instead of `set`.</span></span> <span data-ttu-id="56f38-150">不同之处在于，`init` 访问器只能在构造函数中使用，或通过[对象初始值设定项](object-and-collection-initializers.md)使用。</span><span class="sxs-lookup"><span data-stu-id="56f38-150">The difference is that the `init` accessor can only be used in the constructor or by using an [object-initializer](object-and-collection-initializers.md).</span></span>

## <a name="remarks"></a><span data-ttu-id="56f38-151">备注</span><span class="sxs-lookup"><span data-stu-id="56f38-151">Remarks</span></span>

<span data-ttu-id="56f38-152">可以将属性标记为 `public`、`private`、`protected`、`internal`、`protected internal` 或 `private protected`。</span><span class="sxs-lookup"><span data-stu-id="56f38-152">Properties can be marked as `public`, `private`, `protected`, `internal`, `protected internal`, or `private protected`.</span></span> <span data-ttu-id="56f38-153">这些访问修饰符定义该类的用户访问该属性的方式。</span><span class="sxs-lookup"><span data-stu-id="56f38-153">These access modifiers define how users of the class can access the property.</span></span> <span data-ttu-id="56f38-154">相同属性的 `get` 和 `set` 访问器可以具有不同的访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="56f38-154">The `get` and `set` accessors for the same property may have different access modifiers.</span></span> <span data-ttu-id="56f38-155">例如，`get` 可能为 `public`允许从类型外部进行只读访问；而 `set` 可能为 `private` 或 `protected`。</span><span class="sxs-lookup"><span data-stu-id="56f38-155">For example, the `get` may be `public` to allow read-only access from outside the type, and the `set` may be `private` or `protected`.</span></span> <span data-ttu-id="56f38-156">有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-156">For more information, see [Access Modifiers](./access-modifiers.md).</span></span>

<span data-ttu-id="56f38-157">可以通过使用 `static` 关键字将属性声明为静态属性。</span><span class="sxs-lookup"><span data-stu-id="56f38-157">A property may be declared as a static property by using the `static` keyword.</span></span> <span data-ttu-id="56f38-158">这使属性可供调用方在任何时候使用，即使不存在类的任何实例。</span><span class="sxs-lookup"><span data-stu-id="56f38-158">This makes the property available to callers at any time, even if no instance of the class exists.</span></span> <span data-ttu-id="56f38-159">有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-159">For more information, see [Static Classes and Static Class Members](./static-classes-and-static-class-members.md).</span></span>

<span data-ttu-id="56f38-160">可以通过使用 [virtual](../../language-reference/keywords/virtual.md) 关键字将属性标记为虚拟属性。</span><span class="sxs-lookup"><span data-stu-id="56f38-160">A property may be marked as a virtual property by using the [virtual](../../language-reference/keywords/virtual.md) keyword.</span></span> <span data-ttu-id="56f38-161">这可使派生类使用 [override](../../language-reference/keywords/override.md) 关键字重写属性行为。</span><span class="sxs-lookup"><span data-stu-id="56f38-161">This enables derived classes to override the property behavior by using the [override](../../language-reference/keywords/override.md) keyword.</span></span> <span data-ttu-id="56f38-162">有关这些选项的详细信息，请参阅[继承](inheritance.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-162">For more information about these options, see [Inheritance](inheritance.md).</span></span>

<span data-ttu-id="56f38-163">重写虚拟属性的属性也可以是 [sealed](../../language-reference/keywords/sealed.md)，指定对于派生类，它不再是虚拟的。</span><span class="sxs-lookup"><span data-stu-id="56f38-163">A property overriding a virtual property can also be [sealed](../../language-reference/keywords/sealed.md), specifying that for derived classes it is no longer virtual.</span></span> <span data-ttu-id="56f38-164">最后，可以将属性声明为 [abstract](../../language-reference/keywords/abstract.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-164">Lastly, a property can be declared [abstract](../../language-reference/keywords/abstract.md).</span></span> <span data-ttu-id="56f38-165">这意味着类中没有实现，派生类必须写入自己的实现。</span><span class="sxs-lookup"><span data-stu-id="56f38-165">This means that there is no implementation in the class, and derived classes must write their own implementation.</span></span> <span data-ttu-id="56f38-166">有关这些选项的详细信息，请参阅[抽象类、密封类及类成员](abstract-and-sealed-classes-and-class-members.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-166">For more information about these options, see [Abstract and Sealed Classes and Class Members](abstract-and-sealed-classes-and-class-members.md).</span></span>
  
> [!NOTE]
> <span data-ttu-id="56f38-167">在 [static](../../language-reference/keywords/static.md) 属性的访问器上使用 [virtual](../../language-reference/keywords/virtual.md)、[abstract](../../language-reference/keywords/abstract.md) 或 [override](../../language-reference/keywords/override.md) 修饰符是错误的。</span><span class="sxs-lookup"><span data-stu-id="56f38-167">It is an error to use a [virtual](../../language-reference/keywords/virtual.md), [abstract](../../language-reference/keywords/abstract.md), or [override](../../language-reference/keywords/override.md) modifier on an accessor of a [static](../../language-reference/keywords/static.md) property.</span></span>

## <a name="examples"></a><span data-ttu-id="56f38-168">示例</span><span class="sxs-lookup"><span data-stu-id="56f38-168">Examples</span></span>

<span data-ttu-id="56f38-169">此示例演示实例、静态和只读属性。</span><span class="sxs-lookup"><span data-stu-id="56f38-169">This example demonstrates instance, static, and read-only properties.</span></span> <span data-ttu-id="56f38-170">它接收通过键盘键入的员工姓名，按 1 递增 `NumberOfEmployees`，并显示员工姓名和编号。</span><span class="sxs-lookup"><span data-stu-id="56f38-170">It accepts the name of the employee from the keyboard, increments `NumberOfEmployees` by 1, and displays the Employee name and number.</span></span>

[!code-csharp[csProgGuideProperties#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#2)]

## <a name="hidden-property-example"></a><span data-ttu-id="56f38-171">Hidden 属性示例</span><span class="sxs-lookup"><span data-stu-id="56f38-171">Hidden property example</span></span>

<span data-ttu-id="56f38-172">此示例演示如何访问由派生类中同名的另一属性隐藏的基类中的属性：</span><span class="sxs-lookup"><span data-stu-id="56f38-172">This example demonstrates how to access a property in a base class that is hidden by another property that has the same name in a derived class:</span></span>

[!code-csharp[csProgGuideProperties#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#3)]

<span data-ttu-id="56f38-173">以下内容是前一示例中的要点：</span><span class="sxs-lookup"><span data-stu-id="56f38-173">The following are important points in the previous example:</span></span>

- <span data-ttu-id="56f38-174">派生类中的属性 `Name` 隐藏基类中的属性 `Name`。</span><span class="sxs-lookup"><span data-stu-id="56f38-174">The property `Name` in the derived class hides the property `Name` in the base class.</span></span> <span data-ttu-id="56f38-175">在这种情况下，`new` 修饰符用于派生类的属性声明中：</span><span class="sxs-lookup"><span data-stu-id="56f38-175">In such a case, the `new` modifier is used in the declaration of the property in the derived class:</span></span>

     [!code-csharp[csProgGuideProperties#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#4)]  

- <span data-ttu-id="56f38-176">`(Employee)` 转换用于访问基类中的隐藏属性：</span><span class="sxs-lookup"><span data-stu-id="56f38-176">The cast `(Employee)` is used to access the hidden property in the base class:</span></span>

     [!code-csharp[csProgGuideProperties#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#5)]

     <span data-ttu-id="56f38-177">有关隐藏成员的详细信息，请参阅 [new 修饰符](../../language-reference/keywords/new-modifier.md)。</span><span class="sxs-lookup"><span data-stu-id="56f38-177">For more information about hiding members, see the [new Modifier](../../language-reference/keywords/new-modifier.md).</span></span>

## <a name="override-property-example"></a><span data-ttu-id="56f38-178">Override 属性示例</span><span class="sxs-lookup"><span data-stu-id="56f38-178">Override property example</span></span>

<span data-ttu-id="56f38-179">在此示例中，两个类（`Cube` 和 `Square`）实现抽象类 `Shape`，并重写其抽象 `Area` 属性。</span><span class="sxs-lookup"><span data-stu-id="56f38-179">In this example, two classes, `Cube` and `Square`, implement an abstract class, `Shape`, and override its abstract `Area` property.</span></span> <span data-ttu-id="56f38-180">请注意属性上的 [override](../../language-reference/keywords/override.md) 修饰符的使用。</span><span class="sxs-lookup"><span data-stu-id="56f38-180">Note the use of the [override](../../language-reference/keywords/override.md) modifier on the properties.</span></span> <span data-ttu-id="56f38-181">程序接受将边长作为输入，计算正方形和立方体的面积。</span><span class="sxs-lookup"><span data-stu-id="56f38-181">The program accepts the side as an input and calculates the areas for the square and cube.</span></span> <span data-ttu-id="56f38-182">它还接受将面积作为输入，计算正方形和立方体的相应边长。</span><span class="sxs-lookup"><span data-stu-id="56f38-182">It also accepts the area as an input and calculates the corresponding side for the square and cube.</span></span>

[!code-csharp[csProgGuideProperties#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#6)]

## <a name="see-also"></a><span data-ttu-id="56f38-183">请参阅</span><span class="sxs-lookup"><span data-stu-id="56f38-183">See also</span></span>

- [<span data-ttu-id="56f38-184">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="56f38-184">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="56f38-185">属性</span><span class="sxs-lookup"><span data-stu-id="56f38-185">Properties</span></span>](properties.md)
- [<span data-ttu-id="56f38-186">接口属性</span><span class="sxs-lookup"><span data-stu-id="56f38-186">Interface Properties</span></span>](interface-properties.md)
- [<span data-ttu-id="56f38-187">自动实现的属性</span><span class="sxs-lookup"><span data-stu-id="56f38-187">Auto-Implemented Properties</span></span>](auto-implemented-properties.md)
