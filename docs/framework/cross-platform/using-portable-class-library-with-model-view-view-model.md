---
description: 详细了解：如何将可移植类库与模型-视图-视图模型配合使用
title: 将可移植类库与模型-视图-视图模型配合使用
ms.date: 07/18/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Portable Class Library [.NET Framework], and MVVM
- MVVM, and Portable Class Library
ms.assetid: 41a0b9f8-15a2-431a-bc35-e310b2953b03
ms.openlocfilehash: 4eea099aaa515c2b7d9874fd6c6d5c4132c5e7a2
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "102402224"
---
# <a name="using-portable-class-library-with-model-view-view-model"></a><span data-ttu-id="fa694-103">将可移植类库与模型-视图-视图模型配合使用</span><span class="sxs-lookup"><span data-stu-id="fa694-103">Using Portable Class Library with Model-View-View Model</span></span>

<span data-ttu-id="fa694-104">你可以使用 .NET Framework [可移植类库](portable-class-library.md)来实现模型-视图-视图模型 (MVVM) 模式，并在多个平台之间共享程序集。</span><span class="sxs-lookup"><span data-stu-id="fa694-104">You can use the .NET Framework [Portable Class Library](portable-class-library.md) to implement the Model-View-View Model (MVVM) pattern and share assemblies across multiple platforms.</span></span>

[!INCLUDE[standard](../../../includes/pcl-to-standard.md)]

 <span data-ttu-id="fa694-105">MVVM 是将用户界面与基础业务逻辑相隔离的应用程序模式。</span><span class="sxs-lookup"><span data-stu-id="fa694-105">MVVM is an application pattern that isolates the user interface from the underlying business logic.</span></span> <span data-ttu-id="fa694-106">你可以在 Visual Studio 2012 中的可移植类库项目内实现模型和视图模型类，然后创建针对不同平台自定义的视图。</span><span class="sxs-lookup"><span data-stu-id="fa694-106">You can implement the model and view model classes in a Portable Class Library project in Visual Studio 2012, and then create views that are customized for different platforms.</span></span> <span data-ttu-id="fa694-107">通过此方法，只需编写数据模型和业务逻辑一次，即可将该代码用于 .NET Framework、Silverlight、Windows Phone 和 Windows 8.x 应用商店应用，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="fa694-107">This approach enables you to write the data model and business logic only once, and use that code from .NET Framework, Silverlight, Windows Phone, and Windows 8.x Store apps, as shown in the following illustration.</span></span>

 ![展示将可移植类库与 MVVM 配合使用，并在多个平台之间共享程序集。](./media/using-portable-class-library-with-model-view-view-model/mvvm-share-assemblies-across-platforms.png)

 <span data-ttu-id="fa694-109">本主题不提供有关 MVVM 模式的一般信息。</span><span class="sxs-lookup"><span data-stu-id="fa694-109">This topic does not provide general information about the MVVM pattern.</span></span> <span data-ttu-id="fa694-110">只介绍如何使用可移植类库来实现 MVVM。</span><span class="sxs-lookup"><span data-stu-id="fa694-110">It only provides information about how to use Portable Class Library to implement MVVM.</span></span> <span data-ttu-id="fa694-111">有关 MVVM 的详细信息，请参阅[使用适用于 WPF 的 Prism 库 5.0 的 MVVM 快速入门](/previous-versions/msp-n-p/gg430857(v=pandp.40))。</span><span class="sxs-lookup"><span data-stu-id="fa694-111">For more information about MVVM, see the [MVVM Quickstart Using the Prism Library 5.0 for WPF](/previous-versions/msp-n-p/gg430857(v=pandp.40)).</span></span>

## <a name="classes-that-support-mvvm"></a><span data-ttu-id="fa694-112">支持 MVVM 的类</span><span class="sxs-lookup"><span data-stu-id="fa694-112">Classes That Support MVVM</span></span>

 <span data-ttu-id="fa694-113">当你将 .NET Framework 4.5、适用于 Windows 8.x 应用商店应用的 .NET、Silverlight 或 Windows Phone 7.5 作为可移植类库项目的目标时，可使用以下类实现 MVVM 模式：</span><span class="sxs-lookup"><span data-stu-id="fa694-113">When you target the .NET Framework 4.5, .NET for Windows 8.x Store apps, Silverlight, or Windows Phone 7.5 for your Portable Class Library project, the following classes are available for implementing the MVVM pattern:</span></span>

- <span data-ttu-id="fa694-114"><xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-114"><xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-115"><xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-115"><xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-116"><xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-116"><xref:System.Collections.Specialized.INotifyCollectionChanged?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-117"><xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-117"><xref:System.Collections.Specialized.NotifyCollectionChangedAction?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-118"><xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-118"><xref:System.Collections.Specialized.NotifyCollectionChangedEventArgs?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-119"><xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-119"><xref:System.Collections.Specialized.NotifyCollectionChangedEventHandler?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-120"><xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-120"><xref:System.ComponentModel.DataErrorsChangedEventArgs?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-121"><xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-121"><xref:System.ComponentModel.INotifyDataErrorInfo?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-122"><xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-122"><xref:System.ComponentModel.INotifyPropertyChanged?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-123"><xref:System.Windows.Input.ICommand?displayProperty=nameWithType> 类</span><span class="sxs-lookup"><span data-stu-id="fa694-123"><xref:System.Windows.Input.ICommand?displayProperty=nameWithType> class</span></span>

- <span data-ttu-id="fa694-124"><xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType> 命名空间中的所有类</span><span class="sxs-lookup"><span data-stu-id="fa694-124">All classes in the <xref:System.ComponentModel.DataAnnotations?displayProperty=nameWithType> namespace</span></span>

## <a name="implementing-mvvm"></a><span data-ttu-id="fa694-125">实现 MVVM</span><span class="sxs-lookup"><span data-stu-id="fa694-125">Implementing MVVM</span></span>

 <span data-ttu-id="fa694-126">要实现 MVVM，通常需要在可移植类库项目中同时创建模型和视图模型，因为可移植类库项目不能引用不可移植的项目。</span><span class="sxs-lookup"><span data-stu-id="fa694-126">To implement MVVM, you typically create both the model and the view model in a Portable Class Library project, because a Portable Class Library project cannot reference a non-portable project.</span></span> <span data-ttu-id="fa694-127">模型和视图模型可以在同一个项目中，也可以在不同的项目中。</span><span class="sxs-lookup"><span data-stu-id="fa694-127">The model and view model can be in the same project or in separate projects.</span></span> <span data-ttu-id="fa694-128">如果使用不同的项目，请将视图模型项目中的引用添加到模型项目中。</span><span class="sxs-lookup"><span data-stu-id="fa694-128">If you use separate projects, add a reference from the view model project to the model project.</span></span>

 <span data-ttu-id="fa694-129">编译模型和视图模型项目后，在包含视图的应用中引用这些程序集。</span><span class="sxs-lookup"><span data-stu-id="fa694-129">After you compile the model and view model projects, you reference those assemblies in the app that contains the view.</span></span> <span data-ttu-id="fa694-130">如果视图仅与视图模型交互，你就只需要引用包含视图模型的程序集。</span><span class="sxs-lookup"><span data-stu-id="fa694-130">If the view interacts only with the view model, you only have to reference the assembly that contains the view model.</span></span>

### <a name="model"></a><span data-ttu-id="fa694-131">建模</span><span class="sxs-lookup"><span data-stu-id="fa694-131">Model</span></span>

 <span data-ttu-id="fa694-132">下面的示例展示了可在可移植类库项目中驻留的一个简化的模型类。</span><span class="sxs-lookup"><span data-stu-id="fa694-132">The following example shows a simplified model class that could reside in a Portable Class Library project.</span></span>

 [!code-csharp[PortableClassLibraryMVVM#1](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customer.cs#1)]
 [!code-vb[PortableClassLibraryMVVM#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customer.vb#1)]

 <span data-ttu-id="fa694-133">下面的示例展示了在可移植类库项目中填充、检索和更新数据的一种简单的方法。</span><span class="sxs-lookup"><span data-stu-id="fa694-133">The following example shows a simple way to populate, retrieve, and update the data in a Portable Class Library project.</span></span> <span data-ttu-id="fa694-134">在实际应用中，你将从 Windows Communication Foundation (WCF) 服务等源中检索数据。</span><span class="sxs-lookup"><span data-stu-id="fa694-134">In a real app, you would retrieve the data from a source such as a Windows Communication Foundation (WCF) service.</span></span>

 [!code-csharp[PortableClassLibraryMVVM#2](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/customerrepository.cs#2)]
 [!code-vb[PortableClassLibraryMVVM#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerrepository.vb#2)]

### <a name="view-model"></a><span data-ttu-id="fa694-135">视图模型</span><span class="sxs-lookup"><span data-stu-id="fa694-135">View Model</span></span>

 <span data-ttu-id="fa694-136">在实现 MVVM 模式时，经常会添加一个视图模型的基类。</span><span class="sxs-lookup"><span data-stu-id="fa694-136">A base class for view models is frequently added when implementing the MVVM pattern.</span></span> <span data-ttu-id="fa694-137">下面的示例展示了一个基类。</span><span class="sxs-lookup"><span data-stu-id="fa694-137">The following example shows a base class.</span></span>

 [!code-csharp[PortableClassLibraryMVVM#3](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/viewmodelbase.cs#3)]
 [!code-vb[PortableClassLibraryMVVM#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/viewmodelbase.vb#3)]

 <span data-ttu-id="fa694-138"><xref:System.Windows.Input.ICommand> 接口的实现经常与 MVVM 模式配合使用。</span><span class="sxs-lookup"><span data-stu-id="fa694-138">An implementation of the <xref:System.Windows.Input.ICommand> interface is frequently used with the MVVM pattern.</span></span> <span data-ttu-id="fa694-139">下面的示例演示 <xref:System.Windows.Input.ICommand> 接口的实现。</span><span class="sxs-lookup"><span data-stu-id="fa694-139">The following example shows an implementation of the <xref:System.Windows.Input.ICommand> interface.</span></span>

 [!code-csharp[PortableClassLibraryMVVM#4](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/relaycommand.cs#4)]
 [!code-vb[PortableClassLibraryMVVM#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/relaycommand.vb#4)]

 <span data-ttu-id="fa694-140">下面的示例展示了一个简化的视图模型。</span><span class="sxs-lookup"><span data-stu-id="fa694-140">The following example shows a simplified view model.</span></span>

 [!code-csharp[PortableClassLibraryMVVM#5](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainpageviewmodel.cs#5)]
 [!code-vb[PortableClassLibraryMVVM#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/portableclasslibrarymvvm/vb/customerviewmodel.vb#5)]  
  
### <a name="view"></a><span data-ttu-id="fa694-141">查看</span><span class="sxs-lookup"><span data-stu-id="fa694-141">View</span></span>  

 <span data-ttu-id="fa694-142">从 .NET Framework 4.5 应用、Windows 8.x 应用商店应用、基于 Silverlight 的应用或 Windows Phone 7.5 应用中，可以引用包含模型和视图模型项目的程序集。</span><span class="sxs-lookup"><span data-stu-id="fa694-142">From a .NET Framework 4.5 app, Windows 8.x Store app, Silverlight-based app, or Windows Phone 7.5 app, you can reference the assembly that contains the model and view model projects.</span></span>  <span data-ttu-id="fa694-143">然后创建一个与视图模型交互的视图。</span><span class="sxs-lookup"><span data-stu-id="fa694-143">You then create a view that interacts with the view model.</span></span> <span data-ttu-id="fa694-144">下面的示例展示了一个简化的 Windows Presentation Foundation (WPF) 应用，此应用从视图模型检索和更新数据。</span><span class="sxs-lookup"><span data-stu-id="fa694-144">The following example shows a simplified Windows Presentation Foundation (WPF) app that retrieves and updates data from the view model.</span></span> <span data-ttu-id="fa694-145">你可以在 Silverlight、Windows Phone 或 Windows 8.x 应用商店应用中创建类似的视图。</span><span class="sxs-lookup"><span data-stu-id="fa694-145">You could create similar views in Silverlight, Windows Phone, or Windows 8.x Store apps.</span></span>  
  
 [!code-xaml[PortableClassLibraryMVVM#6](../../../samples/snippets/csharp/VS_Snippets_CLR/portableclasslibrarymvvm/cs/mainwindow.xaml#6)]  
  
## <a name="see-also"></a><span data-ttu-id="fa694-146">另请参阅</span><span class="sxs-lookup"><span data-stu-id="fa694-146">See also</span></span>

- [<span data-ttu-id="fa694-147">可移植类库</span><span class="sxs-lookup"><span data-stu-id="fa694-147">Portable Class Library</span></span>](portable-class-library.md)
