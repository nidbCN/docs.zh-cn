---
title: 方法 - C# 指南
description: 有关方法、方法参数和方法返回值的概述
ms.technology: csharp-fundamentals
ms.date: 03/16/2021
ms.assetid: 577a8527-1081-4b36-9b9e-0685b6553c6e
ms.openlocfilehash: 5d7f654ca268deff4a0c8e69b76e4d636d2f495e
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652927"
---
# <a name="methods-in-c"></a><span data-ttu-id="64fbf-103">(C#) 中的方法</span><span class="sxs-lookup"><span data-stu-id="64fbf-103">Methods in (C#)</span></span>

<span data-ttu-id="64fbf-104">方法是包含一系列语句的代码块。</span><span class="sxs-lookup"><span data-stu-id="64fbf-104">A method is a code block that contains a series of statements.</span></span> <span data-ttu-id="64fbf-105">程序通过调用该方法并指定任何所需的方法参数使语句得以执行。</span><span class="sxs-lookup"><span data-stu-id="64fbf-105">A program causes the statements to be executed by calling the method and specifying any required method arguments.</span></span> <span data-ttu-id="64fbf-106">在 C# 中，每个执行的指令均在方法的上下文中执行。</span><span class="sxs-lookup"><span data-stu-id="64fbf-106">In C#, every executed instruction is performed in the context of a method.</span></span> <span data-ttu-id="64fbf-107">`Main` 方法是每个 C# 应用程序的入口点，并在启动程序时由公共语言运行时 (CLR) 调用。</span><span class="sxs-lookup"><span data-stu-id="64fbf-107">The `Main` method is the entry point for every C# application and it is called by the common language runtime (CLR) when the program is started.</span></span>

> [!NOTE]
> <span data-ttu-id="64fbf-108">本主题讨论命名的方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-108">This topic discusses named methods.</span></span> <span data-ttu-id="64fbf-109">有关匿名函数的信息，请参阅[匿名函数](programming-guide/statements-expressions-operators/anonymous-functions.md)。</span><span class="sxs-lookup"><span data-stu-id="64fbf-109">For information about anonymous functions, see [Anonymous Functions](programming-guide/statements-expressions-operators/anonymous-functions.md).</span></span>

<a name="signatures"></a>

## <a name="method-signatures"></a><span data-ttu-id="64fbf-110">方法签名</span><span class="sxs-lookup"><span data-stu-id="64fbf-110">Method signatures</span></span>

<span data-ttu-id="64fbf-111">通过指定在 `class` 或 `struct` 中声明方法：</span><span class="sxs-lookup"><span data-stu-id="64fbf-111">Methods are declared in a `class` or `struct` by specifying:</span></span>

- <span data-ttu-id="64fbf-112">可选的访问级别，如 `public` 或 `private`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-112">An optional access level, such as `public` or `private`.</span></span> <span data-ttu-id="64fbf-113">默认值为 `private`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-113">The default is `private`.</span></span>
- <span data-ttu-id="64fbf-114">可选的修饰符，如 `abstract` 或 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-114">Optional modifiers such as `abstract` or `sealed`.</span></span>
- <span data-ttu-id="64fbf-115">返回值，或 `void`（如果该方法不具有）。</span><span class="sxs-lookup"><span data-stu-id="64fbf-115">The return value, or `void` if the method has none.</span></span>
- <span data-ttu-id="64fbf-116">方法名称。</span><span class="sxs-lookup"><span data-stu-id="64fbf-116">The method name.</span></span>
- <span data-ttu-id="64fbf-117">任何方法参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-117">Any method parameters.</span></span> <span data-ttu-id="64fbf-118">方法参数在括号内，并且用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="64fbf-118">Method parameters are enclosed in parentheses and are separated by commas.</span></span> <span data-ttu-id="64fbf-119">空括号指示方法不需要任何参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-119">Empty parentheses indicate that the method requires no parameters.</span></span>

<span data-ttu-id="64fbf-120">这些部分一同构成方法签名。</span><span class="sxs-lookup"><span data-stu-id="64fbf-120">These parts together form the method signature.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64fbf-121">出于方法重载的目的，方法的返回类型不是方法签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="64fbf-121">A return type of a method is not part of the signature of the method for the purposes of method overloading.</span></span> <span data-ttu-id="64fbf-122">但是在确定委托和它所指向的方法之间的兼容性时，它是方法签名的一部分。</span><span class="sxs-lookup"><span data-stu-id="64fbf-122">However, it is part of the signature of the method when determining the compatibility between a delegate and the method that it points to.</span></span>

<span data-ttu-id="64fbf-123">以下实例定义了一个包含五种方法的名为 `Motorcycle` 的类：</span><span class="sxs-lookup"><span data-stu-id="64fbf-123">The following example defines a class named `Motorcycle` that contains five methods:</span></span>

[!code-csharp[csSnippets.Methods#40](../../samples/snippets/csharp/concepts/methods/methods40.cs#40)]

<span data-ttu-id="64fbf-124">请注意，`Motorcycle` 类包括一个重载的方法 `Drive`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-124">Note that the `Motorcycle` class includes an overloaded method, `Drive`.</span></span> <span data-ttu-id="64fbf-125">两个方法具有相同的名称，但必须根据其参数类型来区分。</span><span class="sxs-lookup"><span data-stu-id="64fbf-125">Two methods have the same name, but must be differentiated by their parameter types.</span></span>

<a name="invocation"></a>

## <a name="method-invocation"></a><span data-ttu-id="64fbf-126">方法调用</span><span class="sxs-lookup"><span data-stu-id="64fbf-126">Method invocation</span></span>

<span data-ttu-id="64fbf-127">方法可以是实例的或静态的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-127">Methods can be either *instance* or *static*.</span></span> <span data-ttu-id="64fbf-128">调用实例方法需要将对象实例化，并对该对象调用方法；实例方法可对该实例及其数据进行操作。</span><span class="sxs-lookup"><span data-stu-id="64fbf-128">Invoking an instance method requires that you instantiate an object and call the method on that object; an instance method operates on that instance and its data.</span></span> <span data-ttu-id="64fbf-129">通过引用该方法所属类型的名称来调用静态方法；静态方法不对实例数据进行操作。</span><span class="sxs-lookup"><span data-stu-id="64fbf-129">You invoke a static method by referencing the name of the type to which the method belongs; static methods do not operate on instance data.</span></span> <span data-ttu-id="64fbf-130">尝试通过对象实例调用静态方法会引发编译器错误。</span><span class="sxs-lookup"><span data-stu-id="64fbf-130">Attempting to call a static method through an object instance generates a compiler error.</span></span>

<span data-ttu-id="64fbf-131">调用方法就像访问字段。</span><span class="sxs-lookup"><span data-stu-id="64fbf-131">Calling a method is like accessing a field.</span></span> <span data-ttu-id="64fbf-132">在对象名称（如果调用实例方法）或类型名称（如果调用 `static` 方法）后添加一个句点、方法名称和括号。</span><span class="sxs-lookup"><span data-stu-id="64fbf-132">After the object name (if you are calling an instance method) or the type name (if you are calling a `static` method), add a period, the name of the method, and parentheses.</span></span> <span data-ttu-id="64fbf-133">自变量列在括号里，并且用逗号分隔。</span><span class="sxs-lookup"><span data-stu-id="64fbf-133">Arguments are listed within the parentheses and are separated by commas.</span></span>

<span data-ttu-id="64fbf-134">该方法定义指定任何所需参数的名称和类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-134">The method definition specifies the names and types of any parameters that are required.</span></span> <span data-ttu-id="64fbf-135">调用方调用该方法时，它为每个参数提供了称为自变量的具体值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-135">When a caller invokes the method, it provides concrete values, called arguments, for each parameter.</span></span> <span data-ttu-id="64fbf-136">自变量必须与参数类型兼容，但调用代码中使用的自变量名（如果有）不需要与方法中定义的自变量名相同。</span><span class="sxs-lookup"><span data-stu-id="64fbf-136">The arguments must be compatible with the parameter type, but the argument name, if one is used in the calling code, does not have to be the same as the parameter named defined in the method.</span></span> <span data-ttu-id="64fbf-137">在下面示例中，`Square` 方法包含名为 i 的类型为 `int` 的单个参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-137">In the following example, the `Square` method includes a single parameter of type `int` named *i*.</span></span> <span data-ttu-id="64fbf-138">第一种方法调用将向 `Square` 方法传递名为 num 的 `int` 类型的变量；第二个方法调用将传递数值常量；第三个方法调用将传递表达式。</span><span class="sxs-lookup"><span data-stu-id="64fbf-138">The first method call passes the `Square` method a variable of type `int` named *num*; the second, a numeric constant; and the third, an expression.</span></span>

[!code-csharp[csSnippets.Methods#74](../../samples/snippets/csharp/concepts/methods/params74.cs#74)]

<span data-ttu-id="64fbf-139">方法调用最常见的形式是使用位置自变量；它会以与方法参数相同的顺序提供自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-139">The most common form of method invocation used positional arguments; it supplies arguments in the same order as method parameters.</span></span> <span data-ttu-id="64fbf-140">因此，可在以下示例中调用 `Motorcycle` 类的方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-140">The methods of the `Motorcycle` class can therefore be called as in the following example.</span></span> <span data-ttu-id="64fbf-141">例如，`Drive` 方法的调用包含两个与方法语法中的两个参数对应的自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-141">The call to the `Drive` method, for example, includes two arguments that correspond to the two parameters in the method's syntax.</span></span> <span data-ttu-id="64fbf-142">第一个成为 `miles` 参数的值，第二个成为 `speed` 参数的值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-142">The first becomes the value of the `miles` parameter, the second the value of the `speed` parameter.</span></span>

[!code-csharp[csSnippets.Methods#41](../../samples/snippets/csharp/concepts/methods/methods40.cs#41)]

<span data-ttu-id="64fbf-143">调用方法时，也可以使用命名的自变量，而不是位置自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-143">You can also use *named arguments* instead of positional arguments when invoking a method.</span></span> <span data-ttu-id="64fbf-144">使用命名的自变量时，指定参数名，然后后跟冒号（":"）和自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-144">When using named arguments, you specify the parameter name followed by a colon (":") and the argument.</span></span> <span data-ttu-id="64fbf-145">只要包含了所有必需的自变量，方法的自变量可以任何顺序出现。</span><span class="sxs-lookup"><span data-stu-id="64fbf-145">Arguments to the method can appear in any order, as long as all required arguments are present.</span></span> <span data-ttu-id="64fbf-146">下面的示例使用命名的自变量来调用 `TestMotorcycle.Drive` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-146">The following example uses named arguments to invoke the `TestMotorcycle.Drive` method.</span></span> <span data-ttu-id="64fbf-147">在此示例中，命名的自变量以相反于方法参数列表中的顺序进行传递。</span><span class="sxs-lookup"><span data-stu-id="64fbf-147">In this example, the named arguments are passed in the opposite order from the method's parameter list.</span></span>

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/named1.cs#45)]

<span data-ttu-id="64fbf-148">可以同时使用位置自变量和命名的自变量调用方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-148">You can invoke a method using both positional arguments and named arguments.</span></span> <span data-ttu-id="64fbf-149">但是，只有当命名参数位于正确位置时，才能在命名自变量后面放置位置参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-149">However, positional arguments can only follow named arguments when the named arguments are in the correct positions.</span></span> <span data-ttu-id="64fbf-150">下面的示例使用一个位置自变量和一个命名的自变量从上一个示例中调用 `TestMotorcycle.Drive` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-150">The following example invokes the `TestMotorcycle.Drive` method from the previous example using one positional argument and one named argument.</span></span>

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/named2.cs#46)]

<a name="inherited"></a>

## <a name="inherited-and-overridden-methods"></a><span data-ttu-id="64fbf-151">继承和重写方法</span><span class="sxs-lookup"><span data-stu-id="64fbf-151">Inherited and overridden methods</span></span>

<span data-ttu-id="64fbf-152">除了类型中显式定义的成员，类型还继承在其基类中定义的成员。</span><span class="sxs-lookup"><span data-stu-id="64fbf-152">In addition to the members that are explicitly defined in a type, a type inherits members defined in its base classes.</span></span> <span data-ttu-id="64fbf-153">由于托管类型系统中的所有类型都直接或间接继承自 <xref:System.Object> 类，因此所有类型都继承其成员，如 <xref:System.Object.Equals(System.Object)>、<xref:System.Object.GetType> 和 <xref:System.Object.ToString>。</span><span class="sxs-lookup"><span data-stu-id="64fbf-153">Since all types in the managed type system inherit directly or indirectly from the <xref:System.Object> class, all types inherit its members, such as <xref:System.Object.Equals(System.Object)>, <xref:System.Object.GetType>, and <xref:System.Object.ToString>.</span></span> <span data-ttu-id="64fbf-154">下面的示例定义 `Person` 类，实例化两个 `Person` 对象，并调用 `Person.Equals` 方法来确定两个对象是否相等。</span><span class="sxs-lookup"><span data-stu-id="64fbf-154">The following example defines a `Person` class, instantiates two `Person` objects, and calls the `Person.Equals` method to determine whether the two objects are equal.</span></span> <span data-ttu-id="64fbf-155">但是，`Equals` 方法不是在 `Person` 类中定义；而是继承自 <xref:System.Object>。</span><span class="sxs-lookup"><span data-stu-id="64fbf-155">The `Equals` method, however, is not defined in the `Person` class; it is inherited from <xref:System.Object>.</span></span>

[!code-csharp[csSnippets.Methods#104](../../samples/snippets/csharp/concepts/methods/inherited1.cs#104)]

<span data-ttu-id="64fbf-156">类型可以使用 `override` 关键字并提供重写方法的实现来重写继承的成员。</span><span class="sxs-lookup"><span data-stu-id="64fbf-156">Types can override inherited members by using the `override` keyword and providing an implementation for the overridden method.</span></span> <span data-ttu-id="64fbf-157">方法签名必须与重写的方法的签名一样。</span><span class="sxs-lookup"><span data-stu-id="64fbf-157">The method signature must be the same as that of the overridden method.</span></span> <span data-ttu-id="64fbf-158">下面的示例类似于上一个示例，只不过它重写 <xref:System.Object.Equals(System.Object)> 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-158">The following example is like the previous one, except that it overrides the <xref:System.Object.Equals(System.Object)> method.</span></span> <span data-ttu-id="64fbf-159">（它还重写 <xref:System.Object.GetHashCode> 方法，因为这两种方法用于提供一致的结果。）</span><span class="sxs-lookup"><span data-stu-id="64fbf-159">(It also overrides the <xref:System.Object.GetHashCode> method, since the two methods are intended to provide consistent results.)</span></span>

[!code-csharp[csSnippets.Methods#105](../../samples/snippets/csharp/concepts/methods/overridden1.cs#105)]

<a name="passing"></a>

## <a name="passing-parameters"></a><span data-ttu-id="64fbf-160">快速参考</span><span class="sxs-lookup"><span data-stu-id="64fbf-160">Passing parameters</span></span>

<span data-ttu-id="64fbf-161">C# 中的所有类型不是值类型就是引用类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-161">Types in C# are either *value types* or *reference types*.</span></span> <span data-ttu-id="64fbf-162">有关内置值类型的列表，请参阅[类型](./tour-of-csharp/types.md)。</span><span class="sxs-lookup"><span data-stu-id="64fbf-162">For a list of built-in value types, see [Types](./tour-of-csharp/types.md).</span></span> <span data-ttu-id="64fbf-163">默认情况下，值类型和引用类型均按值传递给方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-163">By default, both value types and reference types are passed to a method by value.</span></span>

<a name="byval"></a>

### <a name="passing-parameters-by-value"></a><span data-ttu-id="64fbf-164">按值传递参数</span><span class="sxs-lookup"><span data-stu-id="64fbf-164">Passing parameters by value</span></span>

<span data-ttu-id="64fbf-165">值类型按值传递给方法时，传递的是对象的副本而不是对象本身。</span><span class="sxs-lookup"><span data-stu-id="64fbf-165">When a value type is passed to a method by value, a copy of the object instead of the object itself is passed to the method.</span></span> <span data-ttu-id="64fbf-166">因此，当控件返回调用方时，对已调用方法中的对象的更改对原始对象无影响。</span><span class="sxs-lookup"><span data-stu-id="64fbf-166">Therefore, changes to the object in the called method have no effect on the original object when control returns to the caller.</span></span>

<span data-ttu-id="64fbf-167">下面的示例按值向方法传递值类型，且调用的方法尝试更改值类型的值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-167">The following example passes a value type to a method by value, and the called method attempts to change the value type's value.</span></span> <span data-ttu-id="64fbf-168">它定义属于值类型的 `int` 类型的变量，将其值初始化为 20，并将该类型传递给将变量值改为 30 的名为 `ModifyValue` 的方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-168">It defines a variable of type `int`, which is a value type, initializes its value to 20, and passes it to a method named `ModifyValue` that changes the variable's value to 30.</span></span> <span data-ttu-id="64fbf-169">但是，返回方法时，变量的值保持不变。</span><span class="sxs-lookup"><span data-stu-id="64fbf-169">When the method returns, however, the variable's value remains unchanged.</span></span>

[!code-csharp[csSnippets.Methods#10](../../samples/snippets/csharp/concepts/methods/byvalue10.cs#10)]

<span data-ttu-id="64fbf-170">引用类型的对象按值传递到方法中时，将按值传递对对象的引用。</span><span class="sxs-lookup"><span data-stu-id="64fbf-170">When an object of a reference type is passed to a method by value, a reference to the object is passed by value.</span></span> <span data-ttu-id="64fbf-171">也就是说，该方法接收的不是对象本身，而是指示该对象位置的自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-171">That is, the method receives not the object itself, but an argument that indicates the location of the object.</span></span> <span data-ttu-id="64fbf-172">控件返回到调用方法时，如果通过使用此引用更改对象的成员，此更改将反映在对象中。</span><span class="sxs-lookup"><span data-stu-id="64fbf-172">If you change a member of the object by using this reference, the change is reflected in the object when control returns to the calling method.</span></span> <span data-ttu-id="64fbf-173">但是，当控件返回到调用方时，替换传递到方法的对象对原始对象无影响。</span><span class="sxs-lookup"><span data-stu-id="64fbf-173">However, replacing the object passed to the method has no effect on the original object when control returns to the caller.</span></span>

<span data-ttu-id="64fbf-174">下面的示例定义名为 `SampleRefType` 的类（属于引用类型）。</span><span class="sxs-lookup"><span data-stu-id="64fbf-174">The following example defines a class (which is a reference type) named `SampleRefType`.</span></span> <span data-ttu-id="64fbf-175">它实例化 `SampleRefType` 对象，将 44 赋予其 `value` 字段，并将该对象传递给 `ModifyObject` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-175">It instantiates a `SampleRefType` object, assigns 44 to its `value` field, and passes the object to the `ModifyObject` method.</span></span> <span data-ttu-id="64fbf-176">该示例执行的内容实质上与先前示例相同，均按值将自变量传递到方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-176">This example does essentially the same thing as the previous example -- it passes an argument by value to a method.</span></span> <span data-ttu-id="64fbf-177">但因为使用了引用类型，结果会有所不同。</span><span class="sxs-lookup"><span data-stu-id="64fbf-177">But because a reference type is used, the result is different.</span></span> <span data-ttu-id="64fbf-178">`ModifyObject` 中所做的对 `obj.value` 字段的修改，也会将 `Main` 方法中的自变量 `rt` 的 `value` 字段更改为 33，如示例中的输出值所示。</span><span class="sxs-lookup"><span data-stu-id="64fbf-178">The modification that is made in `ModifyObject` to the `obj.value` field also changes the `value` field of the argument, `rt`, in the `Main` method to 33, as the output from the example shows.</span></span>

[!code-csharp[csSnippets.Methods#42](../../samples/snippets/csharp/concepts/methods/byvalue42.cs#42)]

<a name="byref"></a>

### <a name="passing-parameters-by-reference"></a><span data-ttu-id="64fbf-179">按引用传递参数</span><span class="sxs-lookup"><span data-stu-id="64fbf-179">Passing parameters by reference</span></span>

<span data-ttu-id="64fbf-180">如果想要更改方法中的自变量值并想要在控件返回到调用方法时反映出这一更改，请按引用传递参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-180">You pass a parameter by reference when you want to change the value of an argument in a method and want to reflect that change when control returns to the calling method.</span></span> <span data-ttu-id="64fbf-181">要按引用传递参数，请使用 [`ref`](language-reference/keywords/ref.md) 或 [`out`](language-reference/keywords/out-parameter-modifier.md) 关键字。</span><span class="sxs-lookup"><span data-stu-id="64fbf-181">To pass a parameter by reference, you use the [`ref`](language-reference/keywords/ref.md) or [`out`](language-reference/keywords/out-parameter-modifier.md) keyword.</span></span> <span data-ttu-id="64fbf-182">还可以使用 [`in`](language-reference/keywords/in-parameter-modifier.md) 关键字，按引用传递值以避免复制，但仍防止修改。</span><span class="sxs-lookup"><span data-stu-id="64fbf-182">You can also pass a value by reference to avoid copying but still prevent modifications using the [`in`](language-reference/keywords/in-parameter-modifier.md) keyword.</span></span>

<span data-ttu-id="64fbf-183">下面的示例与上一个示例完全一样，只是换成按引用将值传递给 `ModifyValue` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-183">The following example is identical to the previous one, except the value is passed by reference to the `ModifyValue` method.</span></span> <span data-ttu-id="64fbf-184">参数值在 `ModifyValue` 方法中修改时，值中的更改将在控件返回调用方时反映出来。</span><span class="sxs-lookup"><span data-stu-id="64fbf-184">When the value of the parameter is modified in the `ModifyValue` method, the change in value is reflected when control returns to the caller.</span></span>

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/byref106.cs#106)]

<span data-ttu-id="64fbf-185">引用参数所使用的常见模式涉及交换变量值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-185">A common pattern that uses by ref parameters involves swapping the values of variables.</span></span> <span data-ttu-id="64fbf-186">将两个变量按引用传递给一个方法，然后该方法将二者内容进行交换。</span><span class="sxs-lookup"><span data-stu-id="64fbf-186">You pass two variables to a method by reference, and the method swaps their contents.</span></span> <span data-ttu-id="64fbf-187">下面的示例交换整数值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-187">The following example swaps integer values.</span></span>

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/swap107.cs#107)]

<span data-ttu-id="64fbf-188">通过传递引用类型的参数，可以更改引用本身的值，而不是其单个元素或字段的值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-188">Passing a reference-type parameter allows you to change the value of the reference itself, rather than the value of its individual elements or fields.</span></span>

<a name="paramarray"></a>

### <a name="parameter-arrays"></a><span data-ttu-id="64fbf-189">参数数组</span><span class="sxs-lookup"><span data-stu-id="64fbf-189">Parameter arrays</span></span>

<span data-ttu-id="64fbf-190">有时，向方法指定精确数量的自变量这一要求是受限的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-190">Sometimes, the requirement that you specify the exact number of arguments to your method is restrictive.</span></span> <span data-ttu-id="64fbf-191">通过使用 `params` 关键字来指示一个参数是一个参数数组，可通过可变数量的自变量来调用方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-191">By using the `params` keyword to indicate that a parameter is a parameter array, you allow your method to be called with a variable number of arguments.</span></span> <span data-ttu-id="64fbf-192">使用 `params` 关键字标记的参数必须为数组类型，并且必须是该方法的参数列表中的最后一个参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-192">The parameter tagged with the `params` keyword must be an array type, and it must be the last parameter in the method's parameter list.</span></span>

<span data-ttu-id="64fbf-193">然后，调用方可通过以下四种方式中的任一种来调用方法：</span><span class="sxs-lookup"><span data-stu-id="64fbf-193">A caller can then invoke the method in either of four ways:</span></span>

- <span data-ttu-id="64fbf-194">传递相应类型的数组，该类型包含所需数量的元素。</span><span class="sxs-lookup"><span data-stu-id="64fbf-194">By passing an array of the appropriate type that contains the desired number of elements.</span></span>
- <span data-ttu-id="64fbf-195">向该方法传递相应类型的单独自变量的逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="64fbf-195">By passing a comma-separated list of individual arguments of the appropriate type to the method.</span></span>
- <span data-ttu-id="64fbf-196">传递 `null`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-196">By passing `null`.</span></span>
- <span data-ttu-id="64fbf-197">不向参数数组提供参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-197">By not providing an argument to the parameter array.</span></span>

<span data-ttu-id="64fbf-198">以下示例定义了一个名为 `GetVowels` 的方法，该方法返回参数数组中的所有元音。</span><span class="sxs-lookup"><span data-stu-id="64fbf-198">The following example defines a method named `GetVowels` that returns all the vowels from a parameter array.</span></span> <span data-ttu-id="64fbf-199">`Main` 方法演示了调用方法的全部四种方式。</span><span class="sxs-lookup"><span data-stu-id="64fbf-199">The `Main` method illustrates all four ways of invoking the method.</span></span> <span data-ttu-id="64fbf-200">调用方不需要为包含 `params` 修饰符的形参提供任何实参。</span><span class="sxs-lookup"><span data-stu-id="64fbf-200">Callers are not required to supply any arguments for parameters that include the `params` modifier.</span></span> <span data-ttu-id="64fbf-201">在这种情况下，参数是一个空数组。</span><span class="sxs-lookup"><span data-stu-id="64fbf-201">In that case, the parameter is an empty array.</span></span>

[!code-csharp[csSnippets.Methods#75](~/samples/snippets/csharp/concepts/methods/params75.cs#75)]

<a name="optional"></a>

## <a name="optional-parameters-and-arguments"></a><span data-ttu-id="64fbf-202">可选参数和自变量</span><span class="sxs-lookup"><span data-stu-id="64fbf-202">Optional parameters and arguments</span></span>

<span data-ttu-id="64fbf-203">方法定义可指定其参数是必需的还是可选的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-203">A method definition can specify that its parameters are required or that they are optional.</span></span> <span data-ttu-id="64fbf-204">默认情况下，参数是必需的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-204">By default, parameters are required.</span></span> <span data-ttu-id="64fbf-205">通过在方法定义中包含参数的默认值来指定可选参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-205">Optional parameters are specified by including the parameter's default value in the method definition.</span></span> <span data-ttu-id="64fbf-206">调用该方法时，如果未向可选参数提供自变量，则改为使用默认值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-206">When the method is called, if no argument is supplied for an optional parameter, the default value is used instead.</span></span>

<span data-ttu-id="64fbf-207">参数的默认值必须由以下几种表达式中的一种来赋予：</span><span class="sxs-lookup"><span data-stu-id="64fbf-207">The parameter's default value must be assigned by one of the following kinds of expressions:</span></span>

- <span data-ttu-id="64fbf-208">常量，例如文本字符串或数字。</span><span class="sxs-lookup"><span data-stu-id="64fbf-208">A constant, such as a literal string or number.</span></span>
- <span data-ttu-id="64fbf-209">`new ValType()` 形式的表达式，其中 `ValType` 是值类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-209">An expression of the form `new ValType()`, where `ValType` is a value type.</span></span> <span data-ttu-id="64fbf-210">请注意，这会调用该值类型的隐式无参数构造函数，该函数不是类型的实际成员。</span><span class="sxs-lookup"><span data-stu-id="64fbf-210">Note that this invokes the value type's implicit parameterless constructor, which is not an actual member of the type.</span></span>
- <span data-ttu-id="64fbf-211">`default(ValType)` 形式的表达式，其中 `ValType` 是值类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-211">An expression of the form `default(ValType)`, where `ValType` is a value type.</span></span>

<span data-ttu-id="64fbf-212">如果某个方法同时包含必需的和可选的参数，则在参数列表末尾定义可选参数，即在定义完所有必需参数之后定义。</span><span class="sxs-lookup"><span data-stu-id="64fbf-212">If a method includes both required and optional parameters, optional parameters are defined at the end of the parameter list, after all required parameters.</span></span>

<span data-ttu-id="64fbf-213">下面的示例定义方法 `ExampleMethod`，它具有一个必需参数和两个可选参数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-213">The following example defines a method, `ExampleMethod`, that has one required and two optional parameters.</span></span>

[!code-csharp[csSnippets.Methods#21](../../samples/snippets/csharp/concepts/methods/optional1.cs#21)]

<span data-ttu-id="64fbf-214">如果使用位置自变量调用包含多个可选自变量的方法，调用方必须逐一向所有需要自变量的可选参数提供自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-214">If a method with multiple optional arguments is invoked using positional arguments, the caller must supply an argument for all optional parameters from the first one to the last one for which an argument is supplied.</span></span> <span data-ttu-id="64fbf-215">例如，在使用 `ExampleMethod` 方法的情况下，如果调用方向 `description` 参数提供自变量，还必须向 `optionalInt` 参数提供一个自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-215">In the case of the  `ExampleMethod` method, for example, if the caller supplies an argument for the `description` parameter, it must also supply one for the `optionalInt` parameter.</span></span> <span data-ttu-id="64fbf-216">`opt.ExampleMethod(2, 2, "Addition of 2 and 2");` 是一个有效的方法调用；`opt.ExampleMethod(2, , "Addition of 2 and 0");` 生成编译器错误“缺少自变量”。</span><span class="sxs-lookup"><span data-stu-id="64fbf-216">`opt.ExampleMethod(2, 2, "Addition of 2 and 2");` is a valid method call; `opt.ExampleMethod(2, , "Addition of 2 and 0");` generates an "Argument missing" compiler error.</span></span>

<span data-ttu-id="64fbf-217">如果使用命名的自变量或位置自变量和命名的自变量的组合来调用某个方法，调用方可以省略方法调用中的最后一个位置自变量后的任何自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-217">If a method is called using named arguments or a combination of positional and named arguments, the caller can omit any arguments that follow the last positional argument in the method call.</span></span>

<span data-ttu-id="64fbf-218">下面的示例三次调用了 `ExampleMethod` 方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-218">The following example calls the `ExampleMethod` method three times.</span></span>  <span data-ttu-id="64fbf-219">前两个方法调用使用位置自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-219">The first two method calls use positional arguments.</span></span> <span data-ttu-id="64fbf-220">第一个方法同时省略了两个可选自变量，而第二个省略了最后一个自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-220">The first omits both optional arguments, while the second omits the last argument.</span></span> <span data-ttu-id="64fbf-221">第三个方法调用向必需的参数提供位置自变量，但使用命名的自变量向 `description` 参数提供值，同时省略 `optionalInt` 自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-221">The third method call supplies a positional argument for the required parameter but uses a named argument to supply a value to the `description` parameter while omitting the `optionalInt` argument.</span></span>

[!code-csharp[csSnippets.Methods#22](../../samples/snippets/csharp/concepts/methods/optional1.cs#22)]

<span data-ttu-id="64fbf-222">使用可选参数会影响重载决策，或影响 C# 编译器决定方法应调用哪个特定重载时所使用的方式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="64fbf-222">The use of optional parameters affects *overload resolution*, or the way in which the C# compiler determines which particular overload should be invoked by a method call, as follows:</span></span>

- <span data-ttu-id="64fbf-223">如果方法、索引器或构造函数的每个参数是可选的，或按名称或位置对应于调用语句中的单个自变量，且该自变量可转换为参数的类型，则方法、索引器或构造函数为执行的候选项。</span><span class="sxs-lookup"><span data-stu-id="64fbf-223">A method, indexer, or constructor is a candidate for execution if each of its parameters either is optional or corresponds, by name or by position, to a single argument in the calling statement, and that argument can be converted to the type of the parameter.</span></span>
- <span data-ttu-id="64fbf-224">如果找到多个候选项，则会将用于首选转换的重载决策规则应用于显式指定的自变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-224">If more than one candidate is found, overload resolution rules for preferred conversions are applied to the arguments that are explicitly specified.</span></span> <span data-ttu-id="64fbf-225">将忽略可选形参已省略的实参。</span><span class="sxs-lookup"><span data-stu-id="64fbf-225">Omitted arguments for optional parameters are ignored.</span></span>
- <span data-ttu-id="64fbf-226">如果两个候选项不相上下，则会将没有可选形参的候选项作为首选项，对于这些可选形参，已在调用中为其省略了实参。</span><span class="sxs-lookup"><span data-stu-id="64fbf-226">If two candidates are judged to be equally good, preference goes to a candidate that does not have optional parameters for which arguments were omitted in the call.</span></span> <span data-ttu-id="64fbf-227">这是重载决策中的常规引用的结果，该引用用于参数较少的候选项。</span><span class="sxs-lookup"><span data-stu-id="64fbf-227">This is a consequence of a general preference in overload resolution for candidates that have fewer parameters.</span></span>

<a name="return"></a>

## <a name="return-values"></a><span data-ttu-id="64fbf-228">返回值</span><span class="sxs-lookup"><span data-stu-id="64fbf-228">Return values</span></span>

<span data-ttu-id="64fbf-229">方法可以将值返回到调用方。</span><span class="sxs-lookup"><span data-stu-id="64fbf-229">Methods can return a value to the caller.</span></span> <span data-ttu-id="64fbf-230">如果列在方法名之前的返回类型不是 `void`，则该方法可通过使用 `return` 关键字返回值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-230">If the return type (the type listed before the method name) is not `void`, the method can return the value by using the `return` keyword.</span></span> <span data-ttu-id="64fbf-231">带 `return` 关键字且后跟与返回类型匹配的变量、常数或表达式的语句将向方法调用方返回该值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-231">A statement with the `return` keyword followed by a variable, constant, or expression that matches the return type will return that value to the method caller.</span></span> <span data-ttu-id="64fbf-232">具有非空的返回类型的方法都需要使用 `return` 关键字来返回值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-232">Methods with a non-void return type are required to use the `return` keyword to return a value.</span></span> <span data-ttu-id="64fbf-233">`return` 关键字还会停止执行该方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-233">The `return` keyword also stops the execution of the method.</span></span>

<span data-ttu-id="64fbf-234">如果返回类型为 `void`，没有值的 `return` 语句仍可用于停止执行该方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-234">If the return type is `void`, a `return` statement without a value is still useful to stop the execution of the method.</span></span> <span data-ttu-id="64fbf-235">没有 `return` 关键字，当方法到达代码块结尾时，将停止执行。</span><span class="sxs-lookup"><span data-stu-id="64fbf-235">Without the `return` keyword, the method will stop executing when it reaches the end of the code block.</span></span>

<span data-ttu-id="64fbf-236">例如，这两种方法都使用 `return` 关键字来返回整数：</span><span class="sxs-lookup"><span data-stu-id="64fbf-236">For example, these two methods use the `return` keyword to return integers:</span></span>

[!code-csharp[csSnippets.Methods#44](../../samples/snippets/csharp/concepts/methods/return44.cs#44)]

<span data-ttu-id="64fbf-237">若要使用从方法返回的值，调用方法可以在相同类型的值足够的地方使用该方法调用本身。</span><span class="sxs-lookup"><span data-stu-id="64fbf-237">To use a value returned from a method, the calling method can use the method call itself anywhere a value of the same type would be sufficient.</span></span> <span data-ttu-id="64fbf-238">也可以将返回值分配给变量。</span><span class="sxs-lookup"><span data-stu-id="64fbf-238">You can also assign the return value to a variable.</span></span> <span data-ttu-id="64fbf-239">例如，以下两个代码示例实现了相同的目标：</span><span class="sxs-lookup"><span data-stu-id="64fbf-239">For example, the following two code examples accomplish the same goal:</span></span>

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/return44.cs#45)]

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/return44.cs#46)]

<span data-ttu-id="64fbf-240">在这种情况下，使用本地变量 `result`存储值是可选的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-240">Using a local variable, in this case, `result`, to store a value is optional.</span></span> <span data-ttu-id="64fbf-241">此步骤可以帮助提高代码的可读性，或者如果需要存储该方法整个范围内自变量的原始值，则此步骤可能很有必要。</span><span class="sxs-lookup"><span data-stu-id="64fbf-241">It may help the readability of the code, or it may be necessary if you need to store the original value of the argument for the entire scope of the method.</span></span>

<span data-ttu-id="64fbf-242">有时，需要方法返回多个值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-242">Sometimes, you want your method to return more than a single value.</span></span> <span data-ttu-id="64fbf-243">从 C# 7.0 开始，可以使用元组类型和元组文本轻松实现此目的。</span><span class="sxs-lookup"><span data-stu-id="64fbf-243">Starting with C# 7.0, you can do this easily by using *tuple types* and *tuple literals*.</span></span> <span data-ttu-id="64fbf-244">元组类型定义元组元素的数据类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-244">The tuple type defines the data types of the tuple's elements.</span></span> <span data-ttu-id="64fbf-245">元组文本提供返回的元组的实际值。</span><span class="sxs-lookup"><span data-stu-id="64fbf-245">Tuple literals provide the actual values of the returned tuple.</span></span> <span data-ttu-id="64fbf-246">在下面的示例中，`(string, string, string, int)` 定义 `GetPersonalInfo` 方法返回的元组类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-246">In the following example, `(string, string, string, int)` defines the tuple type that is returned by the `GetPersonalInfo` method.</span></span> <span data-ttu-id="64fbf-247">表达式 `(per.FirstName, per.MiddleName, per.LastName, per.Age)` 是元组文本；方法返回 `PersonInfo` 对象的第一个、中间和最后一个名称及其使用期限。</span><span class="sxs-lookup"><span data-stu-id="64fbf-247">The expression `(per.FirstName, per.MiddleName, per.LastName, per.Age)` is the tuple literal; the method returns the first, middle, and last name, along with the age, of a `PersonInfo` object.</span></span>

```csharp
public (string, string, string, int) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

<span data-ttu-id="64fbf-248">然后调用方可通过类似以下的代码使用返回的元组：</span><span class="sxs-lookup"><span data-stu-id="64fbf-248">The caller can then consume the returned tuple with code like the following:</span></span>

```csharp
var person = GetPersonalInfo("111111111")
Console.WriteLine($"{person.Item1} {person.Item3}: age = {person.Item4}");
```

<span data-ttu-id="64fbf-249">还可向元组类型定义中的元组元素分配名称。</span><span class="sxs-lookup"><span data-stu-id="64fbf-249">Names can also be assigned to the tuple elements in the tuple type definition.</span></span> <span data-ttu-id="64fbf-250">下面的示例展示 `GetPersonalInfo` 方法的替代版本，该方法使用命名的元素：</span><span class="sxs-lookup"><span data-stu-id="64fbf-250">The following example shows an alternate version of the `GetPersonalInfo` method that uses named elements:</span></span>

```csharp
public (string FName, string MName, string LName, int Age) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

<span data-ttu-id="64fbf-251">然后可修改上一次对 `GetPersonInfo` 方法的调用，如下所示：</span><span class="sxs-lookup"><span data-stu-id="64fbf-251">The previous call to the `GetPersonInfo` method can then be modified as follows:</span></span>

```csharp
var person = GetPersonalInfo("111111111");
Console.WriteLine($"{person.FName} {person.LName}: age = {person.Age}");
```

<span data-ttu-id="64fbf-252">如果将数组作为自变量传递给一个方法，并修改各个元素的值，则该方法不一定会返回该数组，尽管选择这么操作的原因是为了实现更好的样式或功能性的值流。</span><span class="sxs-lookup"><span data-stu-id="64fbf-252">If a method is passed an array as an argument and modifies the value of individual elements, it is not necessary for the method to return the array, although you may choose to do so for good style or functional flow of values.</span></span>  <span data-ttu-id="64fbf-253">这是因为 C# 会按值传递所有引用类型，而数组引用的值是指向该数组的指针。</span><span class="sxs-lookup"><span data-stu-id="64fbf-253">This is because C# passes all reference types by value, and the value of an array reference is the pointer to the array.</span></span> <span data-ttu-id="64fbf-254">在下面的示例中，引用该数组的任何代码都能观察到在 `DoubleValues` 方法中对 `values` 数组内容的更改。</span><span class="sxs-lookup"><span data-stu-id="64fbf-254">In the following example, changes to the contents of the `values` array that are made in the `DoubleValues` method are observable by any code that has a reference to the array.</span></span>

[!code-csharp[csSnippets.Methods#101](../../samples/snippets/csharp/concepts/methods/returnarray1.cs#101)]

<a name="extension"></a>

## <a name="extension-methods"></a><span data-ttu-id="64fbf-255">扩展方法</span><span class="sxs-lookup"><span data-stu-id="64fbf-255">Extension methods</span></span>

<span data-ttu-id="64fbf-256">通常，可以通过两种方式向现有类型添加方法：</span><span class="sxs-lookup"><span data-stu-id="64fbf-256">Ordinarily, there are two ways to add a method to an existing type:</span></span>

- <span data-ttu-id="64fbf-257">修改该类型的源代码。</span><span class="sxs-lookup"><span data-stu-id="64fbf-257">Modify the source code for that type.</span></span> <span data-ttu-id="64fbf-258">当然，如果并不拥有该类型的源代码，则无法执行该操作。</span><span class="sxs-lookup"><span data-stu-id="64fbf-258">You cannot do this, of course, if you do not own the type's source code.</span></span> <span data-ttu-id="64fbf-259">并且，如果还添加任何专用数据字段来支持该方法，这会成为一项重大更改。</span><span class="sxs-lookup"><span data-stu-id="64fbf-259">And this becomes a breaking change if you also add any private data fields to support the method.</span></span>
- <span data-ttu-id="64fbf-260">在派生类中定义新方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-260">Define the new method in a derived class.</span></span> <span data-ttu-id="64fbf-261">无法使用其他类型（如结构和枚举）的继承来通过此方式添加方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-261">A method cannot be added in this way using inheritance for other types, such as structures and enumerations.</span></span> <span data-ttu-id="64fbf-262">也不能使用此方式向封闭类“添加”方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-262">Nor can it be used to "add" a method to a sealed class.</span></span>

<span data-ttu-id="64fbf-263">使用扩展方法，可向现有类型“添加”方法，而无需修改类型本身或在继承的类型中实现新方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-263">Extension methods let you "add" a method to an existing type without modifying the type itself or implementing the new method in an inherited type.</span></span> <span data-ttu-id="64fbf-264">扩展方法也无需驻留在与其扩展的类型相同的程序集中。</span><span class="sxs-lookup"><span data-stu-id="64fbf-264">The extension method also does not have to reside in the same assembly as the type it extends.</span></span> <span data-ttu-id="64fbf-265">要把扩展方法当作是定义的类型成员一样调用。</span><span class="sxs-lookup"><span data-stu-id="64fbf-265">You call an extension method as if it were a defined member of a type.</span></span>

<span data-ttu-id="64fbf-266">有关详细信息，请参阅[扩展方法](programming-guide/classes-and-structs/extension-methods.md)</span><span class="sxs-lookup"><span data-stu-id="64fbf-266">For more information, see [Extension Methods](programming-guide/classes-and-structs/extension-methods.md).</span></span>

<a name="async"></a>

## <a name="async-methods"></a><span data-ttu-id="64fbf-267">异步方法</span><span class="sxs-lookup"><span data-stu-id="64fbf-267">Async Methods</span></span>

<span data-ttu-id="64fbf-268">通过使用异步功能，你可以调用异步方法而无需使用显式回调，也不需要跨多个方法或 lambda 表达式来手动拆分代码。</span><span class="sxs-lookup"><span data-stu-id="64fbf-268">By using the async feature, you can invoke asynchronous methods without using explicit callbacks or manually splitting your code across multiple methods or lambda expressions.</span></span>

<span data-ttu-id="64fbf-269">如果用 [async](language-reference/keywords/async.md) 修饰符标记方法，则可以在该方法中使用 [await](language-reference/operators/await.md) 运算符。</span><span class="sxs-lookup"><span data-stu-id="64fbf-269">If you mark a method with the [async](language-reference/keywords/async.md) modifier, you can use the [await](language-reference/operators/await.md) operator in the method.</span></span> <span data-ttu-id="64fbf-270">当控件到达异步方法中的 `await` 表达式时，如果等待的任务未完成，控件将返回到调用方，并在等待任务完成前，包含 `await` 关键字的方法中的进度将一直处于挂起状态。</span><span class="sxs-lookup"><span data-stu-id="64fbf-270">When control reaches an `await` expression in the async method, control returns to the caller if the awaited task is not completed, and progress in the method with the `await` keyword is suspended until the awaited task completes.</span></span> <span data-ttu-id="64fbf-271">任务完成后，可以在方法中恢复执行。</span><span class="sxs-lookup"><span data-stu-id="64fbf-271">When the task is complete, execution can resume in the method.</span></span>

> [!NOTE]
> <span data-ttu-id="64fbf-272">异步方法在遇到第一个尚未完成的 awaited 对象或到达异步方法的末尾时（以先发生者为准），将返回到调用方。</span><span class="sxs-lookup"><span data-stu-id="64fbf-272">An async method returns to the caller when either it encounters the first awaited object that's not yet complete or it gets to the end of the async method, whichever occurs first.</span></span>

<span data-ttu-id="64fbf-273">异步方法通常具有 <xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.Task>、<xref:System.Collections.Generic.IAsyncEnumerable%601> 或 `void` 返回类型。</span><span class="sxs-lookup"><span data-stu-id="64fbf-273">An async method typically has a return type of <xref:System.Threading.Tasks.Task%601>, <xref:System.Threading.Tasks.Task>, <xref:System.Collections.Generic.IAsyncEnumerable%601>or `void`.</span></span> <span data-ttu-id="64fbf-274">`void` 返回类型主要用于定义需要 `void` 返回类型的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="64fbf-274">The `void` return type is used primarily to define event handlers, where a `void` return type is required.</span></span> <span data-ttu-id="64fbf-275">无法等待返回 `void` 的异步方法，并且返回 void 方法的调用方无法捕获该方法引发的异常。</span><span class="sxs-lookup"><span data-stu-id="64fbf-275">An async method that returns `void` can't be awaited, and the caller of a void-returning method can't catch exceptions that the method throws.</span></span> <span data-ttu-id="64fbf-276">从 C# 7.0 开始，异步方法可以有[任何类似任务的返回类型](./whats-new/csharp-7.md#generalized-async-return-types)。</span><span class="sxs-lookup"><span data-stu-id="64fbf-276">Starting with C# 7.0, an async method can have [any task-like return type](./whats-new/csharp-7.md#generalized-async-return-types).</span></span>

<span data-ttu-id="64fbf-277">在下面的示例中，`DelayAsync` 是一个异步方法，包含返回整数的 return 语句。</span><span class="sxs-lookup"><span data-stu-id="64fbf-277">In the following example, `DelayAsync` is an async method that has a return statement that returns an integer.</span></span> <span data-ttu-id="64fbf-278">由于它是异步方法，其方法声明必须具有返回类型 `Task<int>`。</span><span class="sxs-lookup"><span data-stu-id="64fbf-278">Because it is an async method, its method declaration must have a return type of `Task<int>`.</span></span> <span data-ttu-id="64fbf-279">因为返回类型是 `Task<int>`，`DoSomethingAsync` 中 `await` 表达式的计算将如以下 `int result = await delayTask` 语句所示得出整数。</span><span class="sxs-lookup"><span data-stu-id="64fbf-279">Because the return type is `Task<int>`, the evaluation of the `await` expression in `DoSomethingAsync` produces an integer, as the following `int result = await delayTask` statement demonstrates.</span></span>

:::code language="csharp" source="programming-guide/classes-and-structs/snippets/classes-and-structs/methods/Program.cs":::

<span data-ttu-id="64fbf-280">异步方法不能声明任何 [in](language-reference/keywords/in-parameter-modifier.md)、[ref](language-reference/keywords/ref.md) 或 [out](language-reference/keywords/out-parameter-modifier.md) 参数，但是可以调用具有这类参数的方法。</span><span class="sxs-lookup"><span data-stu-id="64fbf-280">An async method can't declare any [in](language-reference/keywords/in-parameter-modifier.md), [ref](language-reference/keywords/ref.md), or [out](language-reference/keywords/out-parameter-modifier.md) parameters, but it can call methods that have such parameters.</span></span>

 <span data-ttu-id="64fbf-281">有关异步方法的详细信息，请参阅[使用 Async 和 Await 的异步编程](async.md)和[异步返回类型](programming-guide/concepts/async/async-return-types.md)。</span><span class="sxs-lookup"><span data-stu-id="64fbf-281">For more information about async methods, see [Asynchronous programming with async and await](async.md) and [Async return types](programming-guide/concepts/async/async-return-types.md).</span></span>

<a name="expr"></a>

## <a name="expression-bodied-members"></a><span data-ttu-id="64fbf-282">Expression-Bodied 成员</span><span class="sxs-lookup"><span data-stu-id="64fbf-282">Expression-bodied members</span></span>

<span data-ttu-id="64fbf-283">具有立即仅返回表达式结果，或单个语句作为方法主题的方法定义很常见。</span><span class="sxs-lookup"><span data-stu-id="64fbf-283">It is common to have method definitions that simply return immediately with the result of an expression, or that have a single statement as the body of the method.</span></span>  <span data-ttu-id="64fbf-284">以下是使用 `=>`定义此类方法的语法快捷方式：</span><span class="sxs-lookup"><span data-stu-id="64fbf-284">There is a syntax shortcut for defining such methods using `=>`:</span></span>

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

<span data-ttu-id="64fbf-285">如果该方法返回 `void` 或是异步方法，则该方法的主体必须是语句表达式（与 lambda 相同）。</span><span class="sxs-lookup"><span data-stu-id="64fbf-285">If the method returns `void` or is an async method, the body of the method must be a statement expression (same as with lambdas).</span></span>  <span data-ttu-id="64fbf-286">对于属性和索引器，两者必须是只读，并且不使用 `get` 访问器关键字。</span><span class="sxs-lookup"><span data-stu-id="64fbf-286">For properties and indexers, they must be read-only, and you do not use the `get` accessor keyword.</span></span>

<a name="iterators"></a>

## <a name="iterators"></a><span data-ttu-id="64fbf-287">迭代器</span><span class="sxs-lookup"><span data-stu-id="64fbf-287">Iterators</span></span>

<span data-ttu-id="64fbf-288">迭代器对集合执行自定义迭代，如列表或数组。</span><span class="sxs-lookup"><span data-stu-id="64fbf-288">An iterator performs a custom iteration over a collection, such as a list or an array.</span></span> <span data-ttu-id="64fbf-289">迭代器使用 [yield return](language-reference/keywords/yield.md) 语句返回元素，每次返回一个。</span><span class="sxs-lookup"><span data-stu-id="64fbf-289">An iterator uses the [yield return](language-reference/keywords/yield.md) statement to return each element one at a time.</span></span> <span data-ttu-id="64fbf-290">到达 `yield return` 语句后，会记住当前位置，以便调用方可以请求序列中的下一个元素。</span><span class="sxs-lookup"><span data-stu-id="64fbf-290">When a `yield return` statement is reached, the current location is remembered so that the caller can request the next element in the sequence.</span></span>

<span data-ttu-id="64fbf-291">迭代器的返回类型可以是 <xref:System.Collections.IEnumerable>、 <xref:System.Collections.Generic.IEnumerable%601>、 <xref:System.Collections.IEnumerator>或 <xref:System.Collections.Generic.IEnumerator%601>。</span><span class="sxs-lookup"><span data-stu-id="64fbf-291">The return type of an iterator can be <xref:System.Collections.IEnumerable>, <xref:System.Collections.Generic.IEnumerable%601>, <xref:System.Collections.IEnumerator>, or <xref:System.Collections.Generic.IEnumerator%601>.</span></span>

<span data-ttu-id="64fbf-292">有关更多信息，请参见 [迭代器](programming-guide/concepts/iterators.md)。</span><span class="sxs-lookup"><span data-stu-id="64fbf-292">For more information, see [Iterators](programming-guide/concepts/iterators.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="64fbf-293">另请参阅</span><span class="sxs-lookup"><span data-stu-id="64fbf-293">See also</span></span>

- [<span data-ttu-id="64fbf-294">访问修饰符</span><span class="sxs-lookup"><span data-stu-id="64fbf-294">Access Modifiers</span></span>](language-reference/keywords/access-modifiers.md)
- [<span data-ttu-id="64fbf-295">静态类和静态类成员</span><span class="sxs-lookup"><span data-stu-id="64fbf-295">Static Classes and Static Class Members</span></span>](programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [<span data-ttu-id="64fbf-296">继承</span><span class="sxs-lookup"><span data-stu-id="64fbf-296">Inheritance</span></span>](programming-guide/classes-and-structs/inheritance.md)
- [<span data-ttu-id="64fbf-297">抽象类、密封类及类成员</span><span class="sxs-lookup"><span data-stu-id="64fbf-297">Abstract and Sealed Classes and Class Members</span></span>](programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [<span data-ttu-id="64fbf-298">params</span><span class="sxs-lookup"><span data-stu-id="64fbf-298">params</span></span>](language-reference/keywords/params.md)
- [<span data-ttu-id="64fbf-299">out</span><span class="sxs-lookup"><span data-stu-id="64fbf-299">out</span></span>](language-reference/keywords/out-parameter-modifier.md)
- [<span data-ttu-id="64fbf-300">ref</span><span class="sxs-lookup"><span data-stu-id="64fbf-300">ref</span></span>](language-reference/keywords/ref.md)
- [<span data-ttu-id="64fbf-301">in</span><span class="sxs-lookup"><span data-stu-id="64fbf-301">in</span></span>](language-reference/keywords/in-parameter-modifier.md)
- [<span data-ttu-id="64fbf-302">传递参数</span><span class="sxs-lookup"><span data-stu-id="64fbf-302">Passing Parameters</span></span>](programming-guide/classes-and-structs/passing-parameters.md)
