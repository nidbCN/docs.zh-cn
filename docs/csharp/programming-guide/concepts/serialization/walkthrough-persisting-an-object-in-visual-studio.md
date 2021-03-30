---
title: 演练：使用 C# 保留对象
description: 本示例使用 C# 创建一个基本 Loan 对象，并将其数据保存到文件中，然后使用该文件中的数据创建一个新的对象。
ms.date: 04/26/2018
ms.openlocfilehash: 0fc733982b7800653a3c8c283bd0af8384d72168
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876443"
---
# <a name="walkthrough-persisting-an-object-using-c"></a><span data-ttu-id="aec03-103">演练：使用 C\# 保留对象</span><span class="sxs-lookup"><span data-stu-id="aec03-103">Walkthrough: persisting an object using C\#</span></span>

<span data-ttu-id="aec03-104">可使用序列化在实例之间保持对象的数据，以便可存储值并在下次实例化对象时检索这些值。</span><span class="sxs-lookup"><span data-stu-id="aec03-104">You can use serialization to persist an object's data between instances, which enables you to store values and retrieve them the next time that the object is instantiated.</span></span>

<span data-ttu-id="aec03-105">本演练将创建一个基础的 `Loan` 对象，并将其值保留在文件中。</span><span class="sxs-lookup"><span data-stu-id="aec03-105">In this walkthrough, you will create a basic `Loan` object and persist its data to a file.</span></span> <span data-ttu-id="aec03-106">当重新创建该对象时，将检索文件中的数据。</span><span class="sxs-lookup"><span data-stu-id="aec03-106">You will then retrieve the data from the file when you re-create the object.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aec03-107">如果此文件尚不存在，本示例将新建文件。</span><span class="sxs-lookup"><span data-stu-id="aec03-107">This example creates a new file if the file does not already exist.</span></span> <span data-ttu-id="aec03-108">如果应用程序必须创建文件，则该应用程序必须对文件夹具有 `Create` 权限。</span><span class="sxs-lookup"><span data-stu-id="aec03-108">If an application must create a file, that application must have `Create` permission for the folder.</span></span> <span data-ttu-id="aec03-109">可使用访问控制列表设置权限。</span><span class="sxs-lookup"><span data-stu-id="aec03-109">Permissions are set by using access control lists.</span></span> <span data-ttu-id="aec03-110">如果文件已存在，则该应用程序只需要 `Write` 权限（这是较弱的权限）。</span><span class="sxs-lookup"><span data-stu-id="aec03-110">If the file already exists, the application needs only `Write` permission, a lesser permission.</span></span> <span data-ttu-id="aec03-111">如有可能，较安全的做法是在部署过程中创建文件并仅向单个文件授予 `Read` 权限（而不是授予文件夹的“创建”权限）。</span><span class="sxs-lookup"><span data-stu-id="aec03-111">Where possible, it's more secure to create the file during deployment and only grant `Read` permissions to a single file (instead of Create permissions for a folder).</span></span> <span data-ttu-id="aec03-112">此外，较安全的做法是将数据写入用户文件夹，而不是根文件夹或 Program Files 文件夹。</span><span class="sxs-lookup"><span data-stu-id="aec03-112">Also, it's more secure to write data to user folders than to the root folder or the Program Files folder.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aec03-113">此示例将数据存储为二进制格式的文件。</span><span class="sxs-lookup"><span data-stu-id="aec03-113">This example stores data in a binary format file.</span></span> <span data-ttu-id="aec03-114">不应将这些格式用于敏感数据，如密码或信用卡信息。</span><span class="sxs-lookup"><span data-stu-id="aec03-114">These formats should not be used for sensitive data, such as passwords or credit-card information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aec03-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="aec03-115">Prerequisites</span></span>

- <span data-ttu-id="aec03-116">若要生成并运行，请安装 [.NET Core SDK](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="aec03-116">To build and run, install the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span>

- <span data-ttu-id="aec03-117">安装常用的代码编辑器（如果尚未安装）。</span><span class="sxs-lookup"><span data-stu-id="aec03-117">Install your favorite code editor, if you haven't already.</span></span>

> [!TIP]
> <span data-ttu-id="aec03-118">需要安装代码编辑器？</span><span class="sxs-lookup"><span data-stu-id="aec03-118">Need to install a code editor?</span></span> <span data-ttu-id="aec03-119">试用 [Visual Studio](https://visualstudio.com/downloads)！</span><span class="sxs-lookup"><span data-stu-id="aec03-119">Try [Visual Studio](https://visualstudio.com/downloads)!</span></span>

- <span data-ttu-id="aec03-120">该示例需要 C# 7.3。</span><span class="sxs-lookup"><span data-stu-id="aec03-120">The example requires C# 7.3.</span></span> <span data-ttu-id="aec03-121">请参阅[选择 C# 语言版本](../../../language-reference/configure-language-version.md)</span><span class="sxs-lookup"><span data-stu-id="aec03-121">See [Select the C# language version](../../../language-reference/configure-language-version.md)</span></span>

<span data-ttu-id="aec03-122">可在 [.NET 示例 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/serialization)在线检查示例代码。</span><span class="sxs-lookup"><span data-stu-id="aec03-122">You can examine the sample code online [at the .NET samples GitHub repository](https://github.com/dotnet/samples/tree/main/csharp/serialization).</span></span>

## <a name="creating-the-loan-object"></a><span data-ttu-id="aec03-123">创建 Loan 对象</span><span class="sxs-lookup"><span data-stu-id="aec03-123">Creating the loan object</span></span>

<span data-ttu-id="aec03-124">第一步是创建 `Loan` 类和使用该类的控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="aec03-124">The first step is to create a `Loan` class and a console application that uses the class:</span></span>

1. <span data-ttu-id="aec03-125">创建一个新的应用程序。</span><span class="sxs-lookup"><span data-stu-id="aec03-125">Create a new application.</span></span> <span data-ttu-id="aec03-126">键入 `dotnet new console -o serialization`，在名为 `serialization` 的子目录下创建新的控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="aec03-126">Type `dotnet new console -o serialization` to create a new console application in a subdirectory named `serialization`.</span></span>
1. <span data-ttu-id="aec03-127">在编辑器中打开应用程序，然后添加名为 `Loan.cs` 的新类。</span><span class="sxs-lookup"><span data-stu-id="aec03-127">Open the application in your editor, and add a new class named `Loan.cs`.</span></span>
1. <span data-ttu-id="aec03-128">将以下代码添加到 `Loan` 类：</span><span class="sxs-lookup"><span data-stu-id="aec03-128">Add the following code to your `Loan` class:</span></span>

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#1)]

<span data-ttu-id="aec03-129">还需要创建一个使用 `Loan` 类的应用程序。</span><span class="sxs-lookup"><span data-stu-id="aec03-129">You will also have to create an application that uses the `Loan` class.</span></span>

## <a name="serialize-the-loan-object"></a><span data-ttu-id="aec03-130">串行化 Loan 对象</span><span class="sxs-lookup"><span data-stu-id="aec03-130">Serialize the loan object</span></span>

1. <span data-ttu-id="aec03-131">打开 `Program.cs`。</span><span class="sxs-lookup"><span data-stu-id="aec03-131">Open `Program.cs`.</span></span> <span data-ttu-id="aec03-132">添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="aec03-132">Add the following code:</span></span>

[!code-csharp[Create a loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#1)]

<span data-ttu-id="aec03-133">添加 `PropertyChanged` 事件的事件处理程序和几行以修改 `Loan` 对象并显示此更改。</span><span class="sxs-lookup"><span data-stu-id="aec03-133">Add an event handler for the `PropertyChanged` event, and a few lines to modify the `Loan` object and display the changes.</span></span> <span data-ttu-id="aec03-134">你可以在下列代码中查看添加项：</span><span class="sxs-lookup"><span data-stu-id="aec03-134">You can see the additions in the following code:</span></span>

[!code-csharp[Listening for the PropertyChanged event](../../../../../samples/snippets/csharp/serialization/Program.cs#2)]

<span data-ttu-id="aec03-135">现在，可运行该代码，并查看当前输出：</span><span class="sxs-lookup"><span data-stu-id="aec03-135">At this point, you can run the code, and see the current output:</span></span>

```console
New customer value: Henry Clay
7.5
7.1
```

<span data-ttu-id="aec03-136">重复运行此应用程序始终编写相同值。</span><span class="sxs-lookup"><span data-stu-id="aec03-136">Running this application repeatedly always writes the same values.</span></span> <span data-ttu-id="aec03-137">每次运行该程序都会创建新的 Loan 对象。</span><span class="sxs-lookup"><span data-stu-id="aec03-137">A new Loan object is created every time you run the program.</span></span> <span data-ttu-id="aec03-138">在现实生活中，利率会定期更改，但不必在每次运行应用程序时都更改利率。</span><span class="sxs-lookup"><span data-stu-id="aec03-138">In the real world, interest rates change periodically, but not necessarily every time that the application is run.</span></span> <span data-ttu-id="aec03-139">序列化代码表示在应用程序的实例之间保存最新的利率。</span><span class="sxs-lookup"><span data-stu-id="aec03-139">Serialization code means you preserve the most recent interest rate between instances of the application.</span></span> <span data-ttu-id="aec03-140">下一步是通过向 Loan 类添加序列化来执行此操作。</span><span class="sxs-lookup"><span data-stu-id="aec03-140">In the next step, you will do just that by adding serialization to the Loan class.</span></span>

## <a name="using-serialization-to-persist-the-object"></a><span data-ttu-id="aec03-141">使用序列化保持对象</span><span class="sxs-lookup"><span data-stu-id="aec03-141">Using Serialization to Persist the Object</span></span>

<span data-ttu-id="aec03-142">为了保持 Loan 类的值，必须首先使用 `Serializable` 属性标记该类。</span><span class="sxs-lookup"><span data-stu-id="aec03-142">In order to persist the values for the Loan class, you must first mark the class with the `Serializable` attribute.</span></span> <span data-ttu-id="aec03-143">将下面的代码添加到 Loan 类声明的上方：</span><span class="sxs-lookup"><span data-stu-id="aec03-143">Add the following code above the Loan class definition:</span></span>

[!code-csharp[Loan class definition](../../../../../samples/snippets/csharp/serialization/Loan.cs#2)]

<span data-ttu-id="aec03-144"><xref:System.SerializableAttribute> 通知编译器可将类中的所有内容保留到文件中。</span><span class="sxs-lookup"><span data-stu-id="aec03-144">The <xref:System.SerializableAttribute> tells the compiler that everything in the class can be persisted to a file.</span></span> <span data-ttu-id="aec03-145">因为 `PropertyChanged` 事件不表示应该存储的对象图的部分，所以它不应序列化。</span><span class="sxs-lookup"><span data-stu-id="aec03-145">Because the `PropertyChanged` event does not represent part of the object graph that should be stored, it should not be serialized.</span></span> <span data-ttu-id="aec03-146">执行此操作可能将所有附加到该事件的对象序列化。</span><span class="sxs-lookup"><span data-stu-id="aec03-146">Doing so would serialize all objects that are attached to that event.</span></span> <span data-ttu-id="aec03-147">可将 <xref:System.NonSerializedAttribute> 添加到 `PropertyChanged` 事件处理程序的字段声明。</span><span class="sxs-lookup"><span data-stu-id="aec03-147">You can add the <xref:System.NonSerializedAttribute> to the field declaration for the `PropertyChanged` event handler.</span></span>

[!code-csharp[Disable serialization for the event handler](../../../../../samples/snippets/csharp/serialization/Loan.cs#3)]

<span data-ttu-id="aec03-148">从 C# 7.3 开始，可使用 `field` 目标值将特性附加到自动实现的属性的支持字段。</span><span class="sxs-lookup"><span data-stu-id="aec03-148">Beginning with C# 7.3, you can attach attributes to the backing field of an auto-implemented property using the `field` target value.</span></span> <span data-ttu-id="aec03-149">以下代码添加 `TimeLastLoaded` 属性并将其标记为不可序列化：</span><span class="sxs-lookup"><span data-stu-id="aec03-149">The following code adds a `TimeLastLoaded` property and marks it as not serializable:</span></span>

[!code-csharp[Disable serialization for an auto-implemented property](../../../../../samples/snippets/csharp/serialization/Loan.cs#4)]

<span data-ttu-id="aec03-150">下一步是向 LoanApp 应用程序添加序列化代码。</span><span class="sxs-lookup"><span data-stu-id="aec03-150">The next step is to add the serialization code to the LoanApp application.</span></span> <span data-ttu-id="aec03-151">为了将该类序列化并将其写入到文件，可使用 <xref:System.IO> 和 <xref:System.Runtime.Serialization.Formatters.Binary> 命名空间。</span><span class="sxs-lookup"><span data-stu-id="aec03-151">In order to serialize the class and write it to a file, you use the <xref:System.IO> and <xref:System.Runtime.Serialization.Formatters.Binary> namespaces.</span></span> <span data-ttu-id="aec03-152">为了避免键入完全限定的名称，可以添加对必要命名空间的引用，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="aec03-152">To avoid typing the fully qualified names, you can add references to the necessary namespaces as shown in the following code:</span></span>

[!code-csharp[Adding namespaces for serialization](../../../../../samples/snippets/csharp/serialization/Program.cs#3)]

<span data-ttu-id="aec03-153">下一步是添加代码，在创建对象时对文件中的对象进行反序列化。</span><span class="sxs-lookup"><span data-stu-id="aec03-153">The next step is to add code to deserialize the object from the file when the object is created.</span></span> <span data-ttu-id="aec03-154">向序列化数据的文件名的类中添加一个常量，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="aec03-154">Add a constant to the class for the serialized data's file name as shown in the following code:</span></span>

[!code-csharp[Define the name of the saved file](../../../../../samples/snippets/csharp/serialization/Program.cs#4)]

<span data-ttu-id="aec03-155">接下来，在创建 `TestLoan` 对象的行后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="aec03-155">Next, add the following code after the line that creates the `TestLoan` object:</span></span>

[!code-csharp[Read from a file if it exists](../../../../../samples/snippets/csharp/serialization/Program.cs#5)]

<span data-ttu-id="aec03-156">首先必须检查该文件是否存在。</span><span class="sxs-lookup"><span data-stu-id="aec03-156">You first must check that the file exists.</span></span> <span data-ttu-id="aec03-157">如果存在，则创建 <xref:System.IO.Stream> 类来读取二进制文件和 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 类，以转换该文件。</span><span class="sxs-lookup"><span data-stu-id="aec03-157">If it exists, create a <xref:System.IO.Stream> class to read the binary file and a <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> class to translate the file.</span></span> <span data-ttu-id="aec03-158">还需将流类型转换为 Loan 对象类型。</span><span class="sxs-lookup"><span data-stu-id="aec03-158">You also need to convert from the stream type to the Loan object type.</span></span>

<span data-ttu-id="aec03-159">然后必须添加代码以将该类序列化到文件中。</span><span class="sxs-lookup"><span data-stu-id="aec03-159">Next you must add code to serialize the class to a file.</span></span> <span data-ttu-id="aec03-160">以 `Main` 方式在现有代码后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="aec03-160">Add the following code after the existing code in the `Main` method:</span></span>

[!code-csharp[Save the existing Loan object](../../../../../samples/snippets/csharp/serialization/Program.cs#6)]

<span data-ttu-id="aec03-161">此时可再次生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="aec03-161">At this point, you can again build and run the application.</span></span> <span data-ttu-id="aec03-162">首次运行时，请注意起始利率为 7.5，然后更改为 7.1.</span><span class="sxs-lookup"><span data-stu-id="aec03-162">The first time it runs, notice that the interest rates starts at 7.5, and then changes to 7.1.</span></span> <span data-ttu-id="aec03-163">关闭该应用程序，然后重新运行。</span><span class="sxs-lookup"><span data-stu-id="aec03-163">Close the application and then run it again.</span></span> <span data-ttu-id="aec03-164">现在，应用程序打印已读取所保存文件的消息，即使在代码更改它之前，利率也是 7.1。</span><span class="sxs-lookup"><span data-stu-id="aec03-164">Now, the application prints the message that it has read the saved file, and the interest rate is 7.1 even before the code that changes it.</span></span>

## <a name="see-also"></a><span data-ttu-id="aec03-165">请参阅</span><span class="sxs-lookup"><span data-stu-id="aec03-165">See also</span></span>

- [<span data-ttu-id="aec03-166">序列化 (C#)</span><span class="sxs-lookup"><span data-stu-id="aec03-166">Serialization (C#)</span></span>](index.md)
- [<span data-ttu-id="aec03-167">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="aec03-167">C# Programming Guide</span></span>](../../index.md)
