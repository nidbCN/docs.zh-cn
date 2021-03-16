---
title: Mgmtclassgen.exe（管理强类型类生成器）
description: 了解 Mgmtclassgen.exe（管理强类型类生成器）。 使用此工具可以为 WMI 类快速生成早期绑定的托管类。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CIM types
- Management Strongly Typed Class Generator
- WMI class
- Mgmtclassgen.exe
- early-bound managed classes
ms.assetid: 02ce6699-49b5-4a0b-b0d5-1003c491232e
ms.openlocfilehash: 8008644a4c443a2fcd4430c6710f4845f1e4bb30
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259364"
---
# <a name="mgmtclassgenexe-management-strongly-typed-class-generator"></a><span data-ttu-id="fe524-104">Mgmtclassgen.exe（管理强类型类生成器）</span><span class="sxs-lookup"><span data-stu-id="fe524-104">Mgmtclassgen.exe (Management Strongly Typed Class Generator)</span></span>

<span data-ttu-id="fe524-105">利用管理强类型类生成器工具，你可以为指定的 Windows Management Instrumentation (WMI) 类快速生成早期绑定的托管类。</span><span class="sxs-lookup"><span data-stu-id="fe524-105">The Management Strongly Typed Class Generator tool enables you to quickly generate an early-bound managed class for a specified Windows Management Instrumentation (WMI) class.</span></span> <span data-ttu-id="fe524-106">生成的类简化了访问 WMI 类的实例所必须编写的代码。</span><span class="sxs-lookup"><span data-stu-id="fe524-106">The generated class simplifies the code you must write to access an instance of the WMI class.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="fe524-107">语法</span><span class="sxs-lookup"><span data-stu-id="fe524-107">Syntax</span></span>  
  
```console  
mgmtclassgen
WMIClass [options]
```  
  
|<span data-ttu-id="fe524-108">参数</span><span class="sxs-lookup"><span data-stu-id="fe524-108">Argument</span></span>|<span data-ttu-id="fe524-109">说明</span><span class="sxs-lookup"><span data-stu-id="fe524-109">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="fe524-110">WMIClass</span><span class="sxs-lookup"><span data-stu-id="fe524-110">*WMIClass*</span></span>|<span data-ttu-id="fe524-111">为其生成早期绑定的托管类的 Windows Management Instrumentation 类。</span><span class="sxs-lookup"><span data-stu-id="fe524-111">The Windows Management Instrumentation class for which to generate an early-bound managed class.</span></span>|  
  
|<span data-ttu-id="fe524-112">选项</span><span class="sxs-lookup"><span data-stu-id="fe524-112">Option</span></span>|<span data-ttu-id="fe524-113">描述</span><span class="sxs-lookup"><span data-stu-id="fe524-113">Description</span></span>|  
|------------|-----------------|  
|<span data-ttu-id="fe524-114">/l  language</span><span class="sxs-lookup"><span data-stu-id="fe524-114">**/l**  *language*</span></span>|<span data-ttu-id="fe524-115">指定用于生成早期绑定的托管类的语言。</span><span class="sxs-lookup"><span data-stu-id="fe524-115">Specifies the language in which to generate the early-bound managed class.</span></span> <span data-ttu-id="fe524-116">可以指定 CS（C#；默认）、VB (Visual Basic)、MC (C++) 或 JS (JScript) 作为语言参数。</span><span class="sxs-lookup"><span data-stu-id="fe524-116">You can specify **CS** (C#; default), **VB** (Visual Basic),  **MC** (C++), or **JS** (JScript) as the language argument.</span></span>|  
|<span data-ttu-id="fe524-117">/m  machine</span><span class="sxs-lookup"><span data-stu-id="fe524-117">**/m**  *machine*</span></span>|<span data-ttu-id="fe524-118">指定要连接到的计算机，WMI 类驻留在该计算机中。</span><span class="sxs-lookup"><span data-stu-id="fe524-118">Specifies the computer to connect to, where the WMI class resides.</span></span> <span data-ttu-id="fe524-119">默认为本地计算机。</span><span class="sxs-lookup"><span data-stu-id="fe524-119">The default is the local computer.</span></span>|  
|<span data-ttu-id="fe524-120">/n  path</span><span class="sxs-lookup"><span data-stu-id="fe524-120">**/n**  *path*</span></span>|<span data-ttu-id="fe524-121">指定包含 WMI 类的 WMI 命名空间的路径。</span><span class="sxs-lookup"><span data-stu-id="fe524-121">Specifies the path to the WMI namespace that contains the WMI class.</span></span> <span data-ttu-id="fe524-122">如果没有指定该选项，则该工具为默认 Root\cimv2 命名空间中的 WMIClass 生成代码。</span><span class="sxs-lookup"><span data-stu-id="fe524-122">If you do not specify this option, the tool generates code for *WMIClass* in the default **Root\cimv2** namespace.</span></span>|  
|<span data-ttu-id="fe524-123">/o  classnamespace</span><span class="sxs-lookup"><span data-stu-id="fe524-123">**/o**  *classnamespace*</span></span>|<span data-ttu-id="fe524-124">指定在其中生成托管代码类的 .NET 命名空间。</span><span class="sxs-lookup"><span data-stu-id="fe524-124">Specifies the .NET namespace in which to generate the managed code class.</span></span> <span data-ttu-id="fe524-125">如果没有指定该选项，则该工具使用 WMI 命名空间和架构前缀生成命名空间。</span><span class="sxs-lookup"><span data-stu-id="fe524-125">If you do not specify this option, the tool generates the namespace using the WMI namespace and the schema prefix.</span></span> <span data-ttu-id="fe524-126">架构前缀是下划线字符前面的类名的一部分。</span><span class="sxs-lookup"><span data-stu-id="fe524-126">The schema prefix is the part of the class name preceding the underscore character.</span></span> <span data-ttu-id="fe524-127">例如，对于 Root\cimv2 命名空间中的 Win32_OperatingSystem 类，该工具会在 ROOT.CIMV2.Win32 中生成类。</span><span class="sxs-lookup"><span data-stu-id="fe524-127">For example, for the **Win32_OperatingSystem** class in the **Root\cimv2** namespace, the tool would generate the class in **ROOT.CIMV2.Win32**.</span></span>|  
|<span data-ttu-id="fe524-128">/p  filepath</span><span class="sxs-lookup"><span data-stu-id="fe524-128">**/p**  *filepath*</span></span>|<span data-ttu-id="fe524-129">指定用于保存已生成代码的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="fe524-129">Specifies the path to the file in which to save the generated code.</span></span> <span data-ttu-id="fe524-130">如果没有指定该选项，则该工具将在当前目录中创建文件。</span><span class="sxs-lookup"><span data-stu-id="fe524-130">If you do not specify this option, the tool creates the file in the current directory.</span></span> <span data-ttu-id="fe524-131">它使用 WMIClass 参数为类和在其中生成类的文件命名。</span><span class="sxs-lookup"><span data-stu-id="fe524-131">It names the class and file in which it generates the class using the *WMIClass* argument.</span></span> <span data-ttu-id="fe524-132">类名和文件名与 WMIClass 的名称相同。</span><span class="sxs-lookup"><span data-stu-id="fe524-132">The name of the class and the file are the same as the name of the *WMIClass.*</span></span> <span data-ttu-id="fe524-133">如果 WMIClass 包含下划线字符，则该工具使用下划线字符后面的类名那部分。</span><span class="sxs-lookup"><span data-stu-id="fe524-133">If *WMIClass* contains an underscore character, the tool uses the part of the class name following the underscore character.</span></span> <span data-ttu-id="fe524-134">例如，如果 WMIClass 名称采用 Win32_LogicalDisk 格式，则生成的类和文件名为“logicaldisk”。</span><span class="sxs-lookup"><span data-stu-id="fe524-134">For example, if the *WMIClass* name is in the format **Win32_LogicalDisk**, the generated class and file is named "logicaldisk".</span></span> <span data-ttu-id="fe524-135">如果文件已存在，则该工具会覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="fe524-135">If a file already exists, the tool overwrites the existing file.</span></span>|  
|<span data-ttu-id="fe524-136">/pw  password</span><span class="sxs-lookup"><span data-stu-id="fe524-136">**/pw**  *password*</span></span>|<span data-ttu-id="fe524-137">指定登录到由 /m 选项指定的计算机时使用的密码。</span><span class="sxs-lookup"><span data-stu-id="fe524-137">Specifies the password to use when logging on to a computer specified by the **/m** option.</span></span>|  
|<span data-ttu-id="fe524-138">/u  user name</span><span class="sxs-lookup"><span data-stu-id="fe524-138">**/u**  *user name*</span></span>|<span data-ttu-id="fe524-139">指定登录到由 /m 选项指定的计算机时使用的用户名。</span><span class="sxs-lookup"><span data-stu-id="fe524-139">Specifies the user name to use when logging on to a computer specified by the **/m** option.</span></span>|  
|<span data-ttu-id="fe524-140">**/?**</span><span class="sxs-lookup"><span data-stu-id="fe524-140">**/?**</span></span>|<span data-ttu-id="fe524-141">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="fe524-141">Displays command syntax and options for the tool.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="fe524-142">备注</span><span class="sxs-lookup"><span data-stu-id="fe524-142">Remarks</span></span>  

 <span data-ttu-id="fe524-143">Mgmtclassgen.exe 使用 <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="fe524-143">Mgmtclassgen.exe uses the <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="fe524-144">因此，你可使用任何自定义代码提供程序用 C#、Visual Basic 和 JScript 以外的托管语言生成代码。</span><span class="sxs-lookup"><span data-stu-id="fe524-144">Therefore, you can use any custom code provider to generate code in managed languages other than C#, Visual Basic, and JScript.</span></span>  
  
 <span data-ttu-id="fe524-145">请注意，生成的类会绑定到为其生成类的架构。</span><span class="sxs-lookup"><span data-stu-id="fe524-145">Note that generated classes are bound to the schema for which they are generated.</span></span> <span data-ttu-id="fe524-146">在基础架构发生更改的情况下，如果你希望类反映架构的更改，则必须重新生成类。</span><span class="sxs-lookup"><span data-stu-id="fe524-146">If the underlying schema changes, you must regenerate the class if you want it to reflect changes to the schema.</span></span>  
  
 <span data-ttu-id="fe524-147">下表显示 WMI 通用信息模型 (CIM) 类型映射到生成的类中的数据类型的方式：</span><span class="sxs-lookup"><span data-stu-id="fe524-147">The following table shows how WMI Common Information Model (CIM) types map to data types in a generated class:</span></span>  
  
|<span data-ttu-id="fe524-148">CIM 类型</span><span class="sxs-lookup"><span data-stu-id="fe524-148">CIM type</span></span>|<span data-ttu-id="fe524-149">生成的类中的数据类型</span><span class="sxs-lookup"><span data-stu-id="fe524-149">Data type in the generated class</span></span>|  
|--------------|--------------------------------------|  
|<span data-ttu-id="fe524-150">CIM_SINT8</span><span class="sxs-lookup"><span data-stu-id="fe524-150">CIM_SINT8</span></span>|<span data-ttu-id="fe524-151">**SByte**</span><span class="sxs-lookup"><span data-stu-id="fe524-151">**SByte**</span></span>|  
|<span data-ttu-id="fe524-152">CIM_UINT8</span><span class="sxs-lookup"><span data-stu-id="fe524-152">CIM_UINT8</span></span>|<span data-ttu-id="fe524-153">**Byte**</span><span class="sxs-lookup"><span data-stu-id="fe524-153">**Byte**</span></span>|  
|<span data-ttu-id="fe524-154">CIM_SINT16</span><span class="sxs-lookup"><span data-stu-id="fe524-154">CIM_SINT16</span></span>|<span data-ttu-id="fe524-155">**Int16**</span><span class="sxs-lookup"><span data-stu-id="fe524-155">**Int16**</span></span>|  
|<span data-ttu-id="fe524-156">CIM_UINT16</span><span class="sxs-lookup"><span data-stu-id="fe524-156">CIM_UINT16</span></span>|<span data-ttu-id="fe524-157">**UInt16**</span><span class="sxs-lookup"><span data-stu-id="fe524-157">**UInt16**</span></span>|  
|<span data-ttu-id="fe524-158">CIM_SINT32</span><span class="sxs-lookup"><span data-stu-id="fe524-158">CIM_SINT32</span></span>|<span data-ttu-id="fe524-159">**Int32**</span><span class="sxs-lookup"><span data-stu-id="fe524-159">**Int32**</span></span>|  
|<span data-ttu-id="fe524-160">SIM_UINT32</span><span class="sxs-lookup"><span data-stu-id="fe524-160">SIM_UINT32</span></span>|<span data-ttu-id="fe524-161">**UInt32**</span><span class="sxs-lookup"><span data-stu-id="fe524-161">**UInt32**</span></span>|  
|<span data-ttu-id="fe524-162">CIM_SINT64</span><span class="sxs-lookup"><span data-stu-id="fe524-162">CIM_SINT64</span></span>|<span data-ttu-id="fe524-163">**Int64**</span><span class="sxs-lookup"><span data-stu-id="fe524-163">**Int64**</span></span>|  
|<span data-ttu-id="fe524-164">CIM_UINT64</span><span class="sxs-lookup"><span data-stu-id="fe524-164">CIM_UINT64</span></span>|<span data-ttu-id="fe524-165">**UInt64**</span><span class="sxs-lookup"><span data-stu-id="fe524-165">**UInt64**</span></span>|  
|<span data-ttu-id="fe524-166">CIM_REAL32</span><span class="sxs-lookup"><span data-stu-id="fe524-166">CIM_REAL32</span></span>|<span data-ttu-id="fe524-167">**单精度**</span><span class="sxs-lookup"><span data-stu-id="fe524-167">**Single**</span></span>|  
|<span data-ttu-id="fe524-168">CIM_REAL64</span><span class="sxs-lookup"><span data-stu-id="fe524-168">CIM_REAL64</span></span>|<span data-ttu-id="fe524-169">**双精度**</span><span class="sxs-lookup"><span data-stu-id="fe524-169">**Double**</span></span>|  
|<span data-ttu-id="fe524-170">CIM_BOOLEAN</span><span class="sxs-lookup"><span data-stu-id="fe524-170">CIM_BOOLEAN</span></span>|<span data-ttu-id="fe524-171">**布尔值**</span><span class="sxs-lookup"><span data-stu-id="fe524-171">**Boolean**</span></span>|  
|<span data-ttu-id="fe524-172">CIM_String</span><span class="sxs-lookup"><span data-stu-id="fe524-172">CIM_String</span></span>|<span data-ttu-id="fe524-173">**字符串**</span><span class="sxs-lookup"><span data-stu-id="fe524-173">**String**</span></span>|  
|<span data-ttu-id="fe524-174">CIM_DATETIME</span><span class="sxs-lookup"><span data-stu-id="fe524-174">CIM_DATETIME</span></span>|<span data-ttu-id="fe524-175">DateTime 或 TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fe524-175">**DateTime** or **TimeSpan**</span></span>|  
|<span data-ttu-id="fe524-176">CIM_REFERENCE</span><span class="sxs-lookup"><span data-stu-id="fe524-176">CIM_REFERENCE</span></span>|<span data-ttu-id="fe524-177">ManagementPath</span><span class="sxs-lookup"><span data-stu-id="fe524-177">**ManagementPath**</span></span>|  
|<span data-ttu-id="fe524-178">CIM_CHAR16</span><span class="sxs-lookup"><span data-stu-id="fe524-178">CIM_CHAR16</span></span>|<span data-ttu-id="fe524-179">**Char**</span><span class="sxs-lookup"><span data-stu-id="fe524-179">**Char**</span></span>|  
|<span data-ttu-id="fe524-180">CIM_OBJECT</span><span class="sxs-lookup"><span data-stu-id="fe524-180">CIM_OBJECT</span></span>|<span data-ttu-id="fe524-181">ManagementBaseObject</span><span class="sxs-lookup"><span data-stu-id="fe524-181">**ManagementBaseObject**</span></span>|  
|<span data-ttu-id="fe524-182">CIM_IUNKNOWN</span><span class="sxs-lookup"><span data-stu-id="fe524-182">CIM_IUNKNOWN</span></span>|<span data-ttu-id="fe524-183">**对象**</span><span class="sxs-lookup"><span data-stu-id="fe524-183">**Object**</span></span>|  
|<span data-ttu-id="fe524-184">CIM_ARRAY</span><span class="sxs-lookup"><span data-stu-id="fe524-184">CIM_ARRAY</span></span>|<span data-ttu-id="fe524-185">上述对象的数组</span><span class="sxs-lookup"><span data-stu-id="fe524-185">Array of the above mentioned objects</span></span>|  
  
 <span data-ttu-id="fe524-186">记下生成 WMI 类时的以下行为：</span><span class="sxs-lookup"><span data-stu-id="fe524-186">Note the following behaviors when you generate a WMI class:</span></span>  
  
- <span data-ttu-id="fe524-187">标准公共属性或方法可能与现有属性或方法具有相同名称。</span><span class="sxs-lookup"><span data-stu-id="fe524-187">It is possible for a standard public property or method to have the same name as an existing property or method.</span></span> <span data-ttu-id="fe524-188">如果出现这种情况，该工具会更改生成的类中的属性或方法的名称，以避免命名冲突。</span><span class="sxs-lookup"><span data-stu-id="fe524-188">If this occurs, the tool changes the name of the property or method in the generated class to avoid naming conflicts.</span></span>  
  
- <span data-ttu-id="fe524-189">生成的类中的属性或方法的名称可能是目标编程语言中的关键字。</span><span class="sxs-lookup"><span data-stu-id="fe524-189">It is possible for the name of a property or method in a generated class to be a keyword in the target programming language.</span></span> <span data-ttu-id="fe524-190">如果出现这种情况，该工具会更改生成的类中的属性或方法的名称，以避免命名冲突。</span><span class="sxs-lookup"><span data-stu-id="fe524-190">If this occurs, the tool changes the name of the property or method in the generated class to avoid naming conflicts.</span></span>  
  
- <span data-ttu-id="fe524-191">在 WMI 中，限定符是包含用于描述类、实例、属性或方法的信息的修饰符。</span><span class="sxs-lookup"><span data-stu-id="fe524-191">In WMI, qualifiers are modifiers that contain information to describe a class, instance, property, or method.</span></span> <span data-ttu-id="fe524-192">WMI 使用标准限定符（如 Read、Write 和 Key）描述生成的类中的属性。</span><span class="sxs-lookup"><span data-stu-id="fe524-192">WMI uses standard qualifiers such as **Read**, **Write**, and **Key** to describe a property in a generated class.</span></span> <span data-ttu-id="fe524-193">例如，生成的类中用 Read 限定符修饰的属性只能使用属性 get 访问器定义。</span><span class="sxs-lookup"><span data-stu-id="fe524-193">For example, a property that is modified with a **Read** qualifier is defined only with a property **get** accessor in the generated class.</span></span> <span data-ttu-id="fe524-194">因为使用 Read 限定符标记的属性将是只读的，所以未定义 set 访问器。</span><span class="sxs-lookup"><span data-stu-id="fe524-194">Because a property marked with the **Read** qualifier is intended to be read-only, a **set** accessor is not defined.</span></span>  
  
- <span data-ttu-id="fe524-195">可使用 Values 和 ValueMaps 限定符来修饰数值型属性，以指示仅可将该属性设置为指定的允许值。</span><span class="sxs-lookup"><span data-stu-id="fe524-195">A numeric property can be modified by the **Values** and **ValueMaps** qualifiers to indicate that the property can be set only to specified permissible values.</span></span> <span data-ttu-id="fe524-196">使用这些 Values 和 ValueMaps 生成枚举，并且属性将映射到该枚举。</span><span class="sxs-lookup"><span data-stu-id="fe524-196">An enumeration is generated with these **Values** and **ValueMaps** and the property is mapped to the enumeration.</span></span>  
  
- <span data-ttu-id="fe524-197">WMI 使用术语单一实例 (singleton) 来描述只能有一个实例的类。</span><span class="sxs-lookup"><span data-stu-id="fe524-197">The WMI uses the term singleton to describe a class that can have only one instance.</span></span> <span data-ttu-id="fe524-198">因此，单一实例类无参数构造函数会将该类初始化为该类的唯一实例。</span><span class="sxs-lookup"><span data-stu-id="fe524-198">Therefore, the parameterless constructor for a singleton class will initialize the class to the only instance of the class.</span></span>  
  
- <span data-ttu-id="fe524-199">WMI 类可以有对象类型的属性。</span><span class="sxs-lookup"><span data-stu-id="fe524-199">A WMI class can have properties that are objects.</span></span> <span data-ttu-id="fe524-200">为此类型的 WMI 类生成强类型类时，应考虑为嵌入式对象属性的类型生成强类型类。</span><span class="sxs-lookup"><span data-stu-id="fe524-200">When you generate a strongly typed class for this type of WMI class, you should consider generating strongly typed classes for the types of the embedded object properties.</span></span> <span data-ttu-id="fe524-201">这使你能以强类型方式访问嵌入对象。</span><span class="sxs-lookup"><span data-stu-id="fe524-201">This will allow you to access the embedded objects in a strongly typed manner.</span></span> <span data-ttu-id="fe524-202">请注意，生成的代码可能无法检测嵌入对象的类型。</span><span class="sxs-lookup"><span data-stu-id="fe524-202">Note that the generated code might not be able to detect the type of the embedded object.</span></span> <span data-ttu-id="fe524-203">在这种情况下，生成的代码中将创建一条注释，以告知你存在此问题。</span><span class="sxs-lookup"><span data-stu-id="fe524-203">In this case, a comment will be created in the generated code to notify you of this issue.</span></span> <span data-ttu-id="fe524-204">然后，你可修改生成的代码以将该属性的类型转换为其他生成类的类型。</span><span class="sxs-lookup"><span data-stu-id="fe524-204">You can then modify the generated code to type the property to the other generated class.</span></span>  
  
- <span data-ttu-id="fe524-205">在 WMI 中，CIM_DATETIME 数据类型的数据值可表示某一特定的日期和时间或时间间隔。</span><span class="sxs-lookup"><span data-stu-id="fe524-205">In WMI, the data value of the CIM_DATETIME data type can represent either a specific date and time or a time interval.</span></span> <span data-ttu-id="fe524-206">如果数据值表示日期和时间，则生成的类中的数据类型为 DateTime。</span><span class="sxs-lookup"><span data-stu-id="fe524-206">If the data value represents a date and time, the data type in the generated class is **DateTime**.</span></span> <span data-ttu-id="fe524-207">如果数据值表示时间间隔，则生成的类中的数据类型为 TimeSpan。</span><span class="sxs-lookup"><span data-stu-id="fe524-207">If the data value represents a time interval, the data type in the generated class is **TimeSpan**.</span></span>  
  
 <span data-ttu-id="fe524-208">你可以选择使用 Visual Studio .NET 中的服务器资源管理器管理扩展来生成强类型的类。</span><span class="sxs-lookup"><span data-stu-id="fe524-208">You can alternately generate a strongly typed class using the Server Explorer Management Extension in Visual Studio .NET.</span></span>  
  
 <span data-ttu-id="fe524-209">有关 WMI 的详细信息，请参阅平台 SDK 文档中的 Windows Management Instrumentation 主题。</span><span class="sxs-lookup"><span data-stu-id="fe524-209">For more information about WMI, see the **Windows Management Instrumentation** topic in the Platform SDK documentation.</span></span>  
  
## <a name="examples"></a><span data-ttu-id="fe524-210">示例</span><span class="sxs-lookup"><span data-stu-id="fe524-210">Examples</span></span>  

 <span data-ttu-id="fe524-211">以下命令在 C# 代码中为 Root\cimv2 命名空间中的 Win32_LogicalDisk WMI 类生成托管类。</span><span class="sxs-lookup"><span data-stu-id="fe524-211">The following command generates a managed class in C# code for the **Win32_LogicalDisk** WMI class in the **Root\cimv2** namespace.</span></span> <span data-ttu-id="fe524-212">该工具将 ROOT.CIMV2.Win32 命名空间中的托管类写入位于 c:\disk.cs 的源文件中。</span><span class="sxs-lookup"><span data-stu-id="fe524-212">The tool writes the managed class to the source file at c:\disk.cs in the **ROOT.CIMV2.Win32** namespace.</span></span>  
  
```console  
mgmtclassgen Win32_LogicalDisk /n root\cimv2 /l CS /p c:\disk.cs  
```  
  
 <span data-ttu-id="fe524-213">以下代码示例说明如何以编程方式使用生成的类。</span><span class="sxs-lookup"><span data-stu-id="fe524-213">The following code example shows how to use a generated class programmatically.</span></span> <span data-ttu-id="fe524-214">首先，枚举类的一个实例并打印路径。</span><span class="sxs-lookup"><span data-stu-id="fe524-214">First, an instance of the class is enumerated and the path is printed.</span></span> <span data-ttu-id="fe524-215">接下来，使用 WMI 的实例创建要初始化的生成类的实例。</span><span class="sxs-lookup"><span data-stu-id="fe524-215">Next, an instance of the generated class to be initialized is created with an instance of WMI.</span></span> <span data-ttu-id="fe524-216">`Process` 是为 Win32_Process 生成的类，`LogicalDisk` 是为 Root\cimv2 命名空间中的 Win32_LogicalDisk 生成的类。</span><span class="sxs-lookup"><span data-stu-id="fe524-216">`Process` is the class generated for **Win32_Process** and `LogicalDisk` is the class generated for **Win32_LogicalDisk** in the **Root\cimv2** namespace.</span></span>  
  
```vb  
Imports System  
Imports System.Management  
Imports ROOT.CIMV2.Win32  
  
Public Class App
   Public Shared Sub Main()
      ' Enumerate instances of the Win32_process.  
      ' Print the Name property of the instance.  
      Dim ps As Process
      For Each ps In  Process.GetInstances()  
         Console.WriteLine(ps.Name)  
      Next ps  
  
      ' Initialize the instance of LogicalDisk with  
      ' the WMI instance pointing to logical drive d:.  
      Dim dskD As New LogicalDisk(New _  
         ManagementPath("win32_LogicalDisk.DeviceId=""d:"""))  
      Console.WriteLine(dskD.Caption)  
   End Sub  
End Class  
```  
  
```csharp  
using System;  
using System.Management;  
using ROOT.CIMV2.Win32;  
  
public class App  
{  
   public static void Main()  
   {  
      // Enumerate instances of the Win32_process.  
      // Print the Name property of the instance.  
      foreach(Process ps in Process.GetInstances())  
      {  
         Console.WriteLine(ps.Name);  
      }  
  
      // Initialize the instance of LogicalDisk with  
      // the WMI instance pointing to logical drive d:.  
      LogicalDisk dskD = new LogicalDisk(new ManagementPath(  
        "win32_LogicalDisk.DeviceId=\"d:\""));  
      Console.WriteLine(dskD.Caption);  
   }  
}  
```  
  
## <a name="see-also"></a><span data-ttu-id="fe524-217">请参阅</span><span class="sxs-lookup"><span data-stu-id="fe524-217">See also</span></span>

- <xref:System.Management>
- <xref:System.Management.ManagementClass.GetStronglyTypedClassCode%2A?displayProperty=nameWithType>
- <xref:System.CodeDom.Compiler.CodeDomProvider?displayProperty=nameWithType>
- [<span data-ttu-id="fe524-218">工具</span><span class="sxs-lookup"><span data-stu-id="fe524-218">Tools</span></span>](index.md)
- [<span data-ttu-id="fe524-219">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="fe524-219">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
