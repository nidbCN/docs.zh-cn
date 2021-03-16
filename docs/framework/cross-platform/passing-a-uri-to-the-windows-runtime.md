---
description: 详细了解：向 Windows 运行时传递 URI
title: 向 Windows 运行时传递 URI
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Runtime, .NET Framework support for
- Windows Runtime, passing a URI to
ms.assetid: 3eb5ce6f-f304-4f87-8e81-0f25092f5ad4
ms.openlocfilehash: 918f8ea71f4b23aeaf2a1295f2edd043a73a95ed
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "102402229"
---
# <a name="passing-a-uri-to-the-windows-runtime"></a><span data-ttu-id="60e85-103">向 Windows 运行时传递 URI</span><span class="sxs-lookup"><span data-stu-id="60e85-103">Passing a URI to the Windows Runtime</span></span>

<span data-ttu-id="60e85-104">Windows 运行时方法只接受绝对 URI。</span><span class="sxs-lookup"><span data-stu-id="60e85-104">Windows Runtime methods accept only absolute URIs.</span></span> <span data-ttu-id="60e85-105">如果向 Windows 运行时方法传递相对的 URI，则会引发 <xref:System.ArgumentException> 异常。</span><span class="sxs-lookup"><span data-stu-id="60e85-105">If you pass a relative URI to a Windows Runtime method, an <xref:System.ArgumentException> exception is thrown.</span></span> <span data-ttu-id="60e85-106">原因是当你在 .NET Framework 代码中使用 Windows 运行时，<xref:Windows.Foundation.Uri?displayProperty=nameWithType> 类会显示为 Intellisense 中的 <xref:System.Uri?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="60e85-106">Here's why: When you use the Windows Runtime in .NET Framework code, the <xref:Windows.Foundation.Uri?displayProperty=nameWithType> class appears as <xref:System.Uri?displayProperty=nameWithType> in Intellisense.</span></span> <span data-ttu-id="60e85-107"><xref:System.Uri?displayProperty=nameWithType> 类允许相对 URI，但 <xref:Windows.Foundation.Uri?displayProperty=nameWithType> 类不允许。</span><span class="sxs-lookup"><span data-stu-id="60e85-107">The <xref:System.Uri?displayProperty=nameWithType> class allows relative URIs, but the <xref:Windows.Foundation.Uri?displayProperty=nameWithType> class does not.</span></span> <span data-ttu-id="60e85-108">这也适用于 Windows 运行时组件中公开的方法。</span><span class="sxs-lookup"><span data-stu-id="60e85-108">This is also true for methods you expose in Windows Runtime Components.</span></span> <span data-ttu-id="60e85-109">如果组件公开接收 URI 的方法，则代码中的签名包含 <xref:System.Uri?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="60e85-109">If your component exposes a method that takes a URI, the signature in your code includes <xref:System.Uri?displayProperty=nameWithType>.</span></span> <span data-ttu-id="60e85-110">但对于组件的用户来说，签名包含 <xref:Windows.Foundation.Uri?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="60e85-110">However, to users of your component, the signature includes <xref:Windows.Foundation.Uri?displayProperty=nameWithType>.</span></span> <span data-ttu-id="60e85-111">传递给组件的 URI 必须是绝对 URI。</span><span class="sxs-lookup"><span data-stu-id="60e85-111">A URI that is passed to your component must be an absolute URI.</span></span>  
  
<span data-ttu-id="60e85-112">本主题演示了如何检测绝对 URI 以及如何在引用应用包中的资源时创建一个。</span><span class="sxs-lookup"><span data-stu-id="60e85-112">This topic shows how to detect an absolute URI and how to create one when referring to a resource in the app package.</span></span>  
  
## <a name="detecting-and-using-an-absolute-uri"></a><span data-ttu-id="60e85-113">检测和使用绝对 URI</span><span class="sxs-lookup"><span data-stu-id="60e85-113">Detecting and using an absolute URI</span></span>  

<span data-ttu-id="60e85-114">使用 <xref:System.Uri.IsAbsoluteUri%2A?displayProperty=nameWithType> 属性确保在将 URI 传递给 Windows 运行时之前，该 URI 是绝对的。</span><span class="sxs-lookup"><span data-stu-id="60e85-114">Use the <xref:System.Uri.IsAbsoluteUri%2A?displayProperty=nameWithType> property to ensure that a URI is absolute before passing it to the Windows Runtime.</span></span> <span data-ttu-id="60e85-115">使用此属性比捕获和处理 <xref:System.ArgumentException> 异常更为有效。</span><span class="sxs-lookup"><span data-stu-id="60e85-115">Using this property is more efficient than catching and handling the <xref:System.ArgumentException> exception.</span></span>  
  
## <a name="using-an-absolute-uri-for-a-resource-in-the-app-package"></a><span data-ttu-id="60e85-116">将绝对 URI 用于应用包中的资源</span><span class="sxs-lookup"><span data-stu-id="60e85-116">Using an absolute URI for a resource in the app package</span></span>  

<span data-ttu-id="60e85-117">如果想要为应用包包含的资源指定 URI，可以使用 `ms-appx` 或 `ms-appx-web` 方案来创建绝对 URI。</span><span class="sxs-lookup"><span data-stu-id="60e85-117">If you want to specify a URI for a resource that your app package contains, you can use the `ms-appx` or `ms-appx-web` scheme to create an absolute URI.</span></span>  
  
<span data-ttu-id="60e85-118">下面的示例演示如何使用 XAML 和代码将 <xref:Windows.UI.Xaml.Controls.WebView> 控件的 <xref:Windows.UI.Xaml.Controls.WebView.Source%2A> 属性和 <xref:Windows.UI.Xaml.Controls.Image> 控件的 <xref:Windows.UI.Xaml.Controls.Image.Source%2A> 属性设置为包含在“Pages”文件夹中的资源。</span><span class="sxs-lookup"><span data-stu-id="60e85-118">The following example shows how to set the <xref:Windows.UI.Xaml.Controls.WebView.Source%2A> property for a <xref:Windows.UI.Xaml.Controls.WebView> control and the <xref:Windows.UI.Xaml.Controls.Image.Source%2A> property for an <xref:Windows.UI.Xaml.Controls.Image> control to resources that are contained in a folder named Pages, using both XAML and code.</span></span>  
  
[!code-xaml[System.URIToWindowsURI#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml#1)]  
[!code-csharp[System.URIToWindowsURI#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.uritowindowsuri/cs/mainpage.xaml.cs#2)]
[!code-vb[System.URIToWindowsURI#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.uritowindowsuri/vb/mainpage.xaml.vb#2)]  
  
<span data-ttu-id="60e85-119">有关这些方案的详细信息，请参阅 Windows 开发人员中心中的 [URI 方案](/windows/uwp/app-resources/uri-schemes)。</span><span class="sxs-lookup"><span data-stu-id="60e85-119">For more information about these schemes, see [URI schemes](/windows/uwp/app-resources/uri-schemes) in the Windows Dev Center.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="60e85-120">请参阅</span><span class="sxs-lookup"><span data-stu-id="60e85-120">See also</span></span>

- [<span data-ttu-id="60e85-121">.NET Framework 对 Windows 应用商店应用和 Windows 运行时的支持情况</span><span class="sxs-lookup"><span data-stu-id="60e85-121">.NET Framework Support for Windows Store Apps and Windows Runtime</span></span>](support-for-windows-store-apps-and-windows-runtime.md)
