---
title: 中断性变更：WinForms 方法现在会引发 ArgumentException
description: 了解 .NET 5 中的中断性变更：某些 Windows 窗体方法现在将针对无效参数引发 ArgumentException。
ms.date: 07/18/2020
ms.openlocfilehash: 9823e9162a562081cdd64346a502ca136b51fa75
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256131"
---
# <a name="winforms-methods-now-throw-argumentexception"></a><span data-ttu-id="05046-103">WinForms 方法现在会引发 ArgumentException</span><span class="sxs-lookup"><span data-stu-id="05046-103">WinForms methods now throw ArgumentException</span></span>

<span data-ttu-id="05046-104">某些 Windows 窗体方法现在将针对无效参数引发 <xref:System.ArgumentException>，之前不会这样。</span><span class="sxs-lookup"><span data-stu-id="05046-104">Some Windows Forms methods now throw an <xref:System.ArgumentException> for invalid arguments, where previously they did not.</span></span>

## <a name="change-description"></a><span data-ttu-id="05046-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="05046-105">Change description</span></span>

<span data-ttu-id="05046-106">以前，如果将异常类型或错误类型的参数传递给某些 Windows 窗体方法，会导致不确定的状态。</span><span class="sxs-lookup"><span data-stu-id="05046-106">Previously, passing arguments of an unexpected or incorrect type to certain Windows Forms methods would result in an indeterminate state.</span></span> <span data-ttu-id="05046-107">从 .NET 5 开始，在传递无效参数后，这些方法现会引发 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="05046-107">Starting in .NET 5, these methods now throw an <xref:System.ArgumentException> when passed invalid arguments.</span></span>

<span data-ttu-id="05046-108">引发 <xref:System.ArgumentException> 符合 .NET 运行时的行为。</span><span class="sxs-lookup"><span data-stu-id="05046-108">Throwing an <xref:System.ArgumentException> conforms to the behavior of the .NET runtime.</span></span> <span data-ttu-id="05046-109">它还通过清楚地指示具体的无效参数来改进调试体验。</span><span class="sxs-lookup"><span data-stu-id="05046-109">It also improves the debugging experience by clearly communicating which argument is invalid.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="05046-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="05046-110">Version introduced</span></span>

<span data-ttu-id="05046-111">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="05046-111">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="05046-112">建议操作</span><span class="sxs-lookup"><span data-stu-id="05046-112">Recommended action</span></span>

- <span data-ttu-id="05046-113">更新代码以防止传递无效参数。</span><span class="sxs-lookup"><span data-stu-id="05046-113">Update the code to prevent passing invalid arguments.</span></span>
- <span data-ttu-id="05046-114">如有必要，请在调用方法时处理 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="05046-114">If necessary, handle an <xref:System.ArgumentException> when calling the method.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="05046-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="05046-115">Affected APIs</span></span>

<span data-ttu-id="05046-116">下表列出了受影响的方法和参数：</span><span class="sxs-lookup"><span data-stu-id="05046-116">The following table lists the affected methods and parameters:</span></span>

| <span data-ttu-id="05046-117">方法</span><span class="sxs-lookup"><span data-stu-id="05046-117">Method</span></span> | <span data-ttu-id="05046-118">参数名称</span><span class="sxs-lookup"><span data-stu-id="05046-118">Parameter name</span></span> | <span data-ttu-id="05046-119">条件</span><span class="sxs-lookup"><span data-stu-id="05046-119">Condition</span></span> | <span data-ttu-id="05046-120">新增的版本</span><span class="sxs-lookup"><span data-stu-id="05046-120">Version added</span></span> |
|-|-|-|-|
| <xref:System.Windows.Forms.TabControl.GetToolTipText(System.Object)?displayProperty=fullName> | `item` | <span data-ttu-id="05046-121">参数不属于 <xref:System.Windows.Forms.TabPage> 类型。</span><span class="sxs-lookup"><span data-stu-id="05046-121">Argument is not of type <xref:System.Windows.Forms.TabPage>.</span></span> | <span data-ttu-id="05046-122">预览版 1</span><span class="sxs-lookup"><span data-stu-id="05046-122">Preview 1</span></span> |
| <xref:System.Windows.Forms.DataFormats.GetFormat(System.String)?displayProperty=fullName> | `format` | <span data-ttu-id="05046-123">参数为 `null`、<xref:System.String.Empty?displayProperty=nameWithType> 或空格。</span><span class="sxs-lookup"><span data-stu-id="05046-123">Argument is `null`, <xref:System.String.Empty?displayProperty=nameWithType>, or white space.</span></span> | <span data-ttu-id="05046-124">预览版 5</span><span class="sxs-lookup"><span data-stu-id="05046-124">Preview 5</span></span> |
| <xref:System.Windows.Forms.InputLanguageChangedEventArgs.%23ctor(System.Globalization.CultureInfo,System.Byte)> | `culture` | <span data-ttu-id="05046-125">无法检索指定区域性的 `InputLanguage`。</span><span class="sxs-lookup"><span data-stu-id="05046-125">Unable to retrieve an `InputLanguage` for the specified culture.</span></span> | <span data-ttu-id="05046-126">预览版 7</span><span class="sxs-lookup"><span data-stu-id="05046-126">Preview 7</span></span> |

<!--

### Affected APIs

- `M:System.Windows.Forms.TabControl.GetToolTipText(System.Object)`
- `M:System.Windows.Forms.DataFormats.GetFormat(System.String)`
- `M:System.Windows.Forms.InputLanguageChangedEventArgs.%23ctor(System.Globalization.CultureInfo,System.Byte)`

### Category

Windows Forms

-->
