---
title: 中断性变更：全球化 API 在 Windows 上使用 ICU 库
description: 了解 .NET 5 中的全球化中断性变更：ICU 库用于全球化功能而不是 NLS。
ms.date: 05/19/2020
ms.openlocfilehash: 4b8580fcb3ba3c9b95357a7922e3a3062ccd3728
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256746"
---
# <a name="globalization-apis-use-icu-libraries-on-windows"></a><span data-ttu-id="ac630-103">全球化 API 在 Windows 上使用 ICU 库</span><span class="sxs-lookup"><span data-stu-id="ac630-103">Globalization APIs use ICU libraries on Windows</span></span>

<span data-ttu-id="ac630-104">当在 Windows 10 2019 年 5 月更新或更高版本上运行时，.NET 5 及更高版本使用 [Unicode 国际组件 (ICU)](http://site.icu-project.org/home) 库来实现全球化功能。</span><span class="sxs-lookup"><span data-stu-id="ac630-104">.NET 5 and later versions use [International Components for Unicode (ICU)](http://site.icu-project.org/home) libraries for globalization functionality when running on Windows 10 May 2019 Update or later.</span></span>

## <a name="change-description"></a><span data-ttu-id="ac630-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="ac630-105">Change description</span></span>

<span data-ttu-id="ac630-106">在 .NET Core 1.0 - 3.1 和 .NET Framework 4 及更高版本中，.NET 库使用[本地语言支持 (NLS)](/windows/win32/intl/national-language-support) API 来实现 Windows 上的全球化功能。</span><span class="sxs-lookup"><span data-stu-id="ac630-106">In .NET Core 1.0 - 3.1 and .NET Framework 4 and later, .NET libraries use [National Language Support (NLS)](/windows/win32/intl/national-language-support) APIs for globalization functionality on Windows.</span></span> <span data-ttu-id="ac630-107">例如，NLS 函数用于比较字符串，获取区域性信息，并在适当的区域中执行字符串大小写。</span><span class="sxs-lookup"><span data-stu-id="ac630-107">For example, NLS functions were used to compare strings, get culture information, and perform string casing in the appropriate culture.</span></span>

<span data-ttu-id="ac630-108">从 .NET 5 开始，如果应用在 Windows 10 2019 年 5 月更新或更高版本上运行，.NET 库将默认使用 [ICU](http://site.icu-project.org/home) 全球化 API。</span><span class="sxs-lookup"><span data-stu-id="ac630-108">Starting in .NET 5, if an app is running on Windows 10 May 2019 Update or later, .NET libraries use [ICU](http://site.icu-project.org/home) globalization APIs, by default.</span></span>

> [!NOTE]
> <span data-ttu-id="ac630-109">Windows 10 2019 年 5 月更新及更高版本随 ICU 本机库一起提供。</span><span class="sxs-lookup"><span data-stu-id="ac630-109">Windows 10 May 2019 Update and later versions ship with the ICU native library.</span></span> <span data-ttu-id="ac630-110">如果 .NET 运行时无法加载 ICU，它将改用 NLS。</span><span class="sxs-lookup"><span data-stu-id="ac630-110">If the .NET runtime can't load ICU, it uses NLS instead.</span></span>

## <a name="behavioral-differences"></a><span data-ttu-id="ac630-111">行为差异</span><span class="sxs-lookup"><span data-stu-id="ac630-111">Behavioral differences</span></span>

<span data-ttu-id="ac630-112">即使你不知道正在使用全球化设施，你也可能会在应用中看到更改。</span><span class="sxs-lookup"><span data-stu-id="ac630-112">You might see changes in your app even if you don't realize you're using globalization facilities.</span></span> <span data-ttu-id="ac630-113">本部分列出了你可能会看到的一些行为更改，但还有其他一些行为更改。</span><span class="sxs-lookup"><span data-stu-id="ac630-113">This section lists a couple of the behavioral changes you might see, but there are others too.</span></span>

### <a name="stringindexof"></a><span data-ttu-id="ac630-114">String.IndexOf</span><span class="sxs-lookup"><span data-stu-id="ac630-114">String.IndexOf</span></span>

<span data-ttu-id="ac630-115">请考虑使用以下代码，它调用 <xref:System.String.IndexOf(System.String)?displayProperty=nameWithType> 来查找字符串中的换行符索引。</span><span class="sxs-lookup"><span data-stu-id="ac630-115">Consider the following code that calls <xref:System.String.IndexOf(System.String)?displayProperty=nameWithType> to find the index of the newline character in a string.</span></span>

```csharp
string s = "Hello\r\nworld!";
int idx = s.IndexOf("\n");
Console.WriteLine(idx);
```

- <span data-ttu-id="ac630-116">在 Windows 上的早期版本的 .NET 中，代码片段打印 `6`。</span><span class="sxs-lookup"><span data-stu-id="ac630-116">In previous versions of .NET on Windows, the snippet prints `6`.</span></span>
- <span data-ttu-id="ac630-117">在 Windows 19H1 和更高版本上的 .NET 5 及更高版本中，代码片段会打印 `-1`。</span><span class="sxs-lookup"><span data-stu-id="ac630-117">In .NET 5 and later versions on Windows 19H1 and later versions, the snippet prints `-1`.</span></span>

<span data-ttu-id="ac630-118">若要通过执行序号搜索而不是区分区域性的搜索来修复此代码，请调用 <xref:System.String.IndexOf(System.String,System.StringComparison)> 重载，并传入 <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> 作为参数。</span><span class="sxs-lookup"><span data-stu-id="ac630-118">To fix this code by conducting an ordinal search instead of a culture-sensitive search, call the <xref:System.String.IndexOf(System.String,System.StringComparison)> overload and pass in <xref:System.StringComparison.Ordinal?displayProperty=nameWithType> as an argument.</span></span>

<span data-ttu-id="ac630-119">可以运行代码分析规则 [CA1307：为了清晰起见，请指定 StringComparison](../../../../fundamentals/code-analysis/quality-rules/ca1307.md) 和 [CA1309：使用序号 StringComparison](../../../../fundamentals/code-analysis/quality-rules/ca1309.md) 在代码中查找这些调用站点。</span><span class="sxs-lookup"><span data-stu-id="ac630-119">You can run code analysis rules [CA1307: Specify StringComparison for clarity](../../../../fundamentals/code-analysis/quality-rules/ca1307.md) and [CA1309: Use ordinal StringComparison](../../../../fundamentals/code-analysis/quality-rules/ca1309.md) to find these call sites in your code.</span></span>

<span data-ttu-id="ac630-120">有关详细信息，请参阅[在 .NET 5 及更高版本中比较字符串时的行为更改](../../../../standard/base-types/string-comparison-net-5-plus.md)。</span><span class="sxs-lookup"><span data-stu-id="ac630-120">For more information, see [Behavior changes when comparing strings on .NET 5+](../../../../standard/base-types/string-comparison-net-5-plus.md).</span></span>

### <a name="currency-symbol"></a><span data-ttu-id="ac630-121">货币符号</span><span class="sxs-lookup"><span data-stu-id="ac630-121">Currency symbol</span></span>

<span data-ttu-id="ac630-122">请考虑使用以下代码，它使用货币格式说明符 `C` 设置字符串格式。</span><span class="sxs-lookup"><span data-stu-id="ac630-122">Consider the following code that formats a string using the currency format specifier `C`.</span></span> <span data-ttu-id="ac630-123">当前线程的区域性设置为仅包括语言（而非国家/地区）的区域性。</span><span class="sxs-lookup"><span data-stu-id="ac630-123">The current thread's culture is set to a culture that includes only the language and not the country.</span></span>

```csharp
System.Threading.Thread.CurrentThread.CurrentCulture = new System.Globalization.CultureInfo("de");
string text = string.Format("{0:C}", 100);
```

- <span data-ttu-id="ac630-124">在 Windows 上的早期版本的 .NET 中，文本值为 `"100,00 €"`。</span><span class="sxs-lookup"><span data-stu-id="ac630-124">In previous versions of .NET on Windows, the value of text is `"100,00 €"`.</span></span>
- <span data-ttu-id="ac630-125">在 Windows 19H1 和更高版本上的 .NET 5 及更高版本中，文本值为 `"100,00 ¤"`，它使用国际货币符号而不是欧元。</span><span class="sxs-lookup"><span data-stu-id="ac630-125">In .NET 5 and later versions on Windows 19H1 and later versions, the value of text is `"100,00 ¤"`, which uses the international currency symbol instead of the euro.</span></span> <span data-ttu-id="ac630-126">在 ICU 中，这种设计是指，货币是国家或地区的属性，而不是语言。</span><span class="sxs-lookup"><span data-stu-id="ac630-126">In ICU, the design is that a currency is a property of a country or region, not a language.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="ac630-127">更改原因</span><span class="sxs-lookup"><span data-stu-id="ac630-127">Reason for change</span></span>

<span data-ttu-id="ac630-128">引入此更改是为了统一所有支持的操作系统上的 .NET 的全球化行为。</span><span class="sxs-lookup"><span data-stu-id="ac630-128">This change was introduced to unify .NET's globalization behavior across all supported operating systems.</span></span> <span data-ttu-id="ac630-129">它还能让应用程序捆绑自己的全球化库，而不是依赖于操作系统的内置库。</span><span class="sxs-lookup"><span data-stu-id="ac630-129">It also provides the ability for applications to bundle their own globalization libraries rather than depend on the operating system's built-in libraries.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ac630-130">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ac630-130">Version introduced</span></span>

<span data-ttu-id="ac630-131">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="ac630-131">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ac630-132">建议操作</span><span class="sxs-lookup"><span data-stu-id="ac630-132">Recommended action</span></span>

<span data-ttu-id="ac630-133">开发人员一方不需要执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="ac630-133">No action is required on the part of the developer.</span></span> <span data-ttu-id="ac630-134">但是，如果你想要继续使用 NLS 全球化 API，则可以将[运行时开关](../../../run-time-config/globalization.md#nls)设置为还原到该行为。</span><span class="sxs-lookup"><span data-stu-id="ac630-134">However, if you wish to continue using NLS globalization APIs, you can set a [run-time switch](../../../run-time-config/globalization.md#nls) to revert to that behavior.</span></span> <span data-ttu-id="ac630-135">有关可用开关的详细信息，请参阅 [.NET 全球化和 ICU](../../../../standard/globalization-localization/globalization-icu.md) 一文。</span><span class="sxs-lookup"><span data-stu-id="ac630-135">For more information about the available switches, see the [.NET globalization and ICU](../../../../standard/globalization-localization/globalization-icu.md) article.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ac630-136">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ac630-136">Affected APIs</span></span>

- <xref:System.Span%601?displayProperty=fullName>
- <xref:System.String?displayProperty=fullName>
- <span data-ttu-id="ac630-137"><xref:System.Globalization?displayProperty=fullName> 命名空间中的大多数类型</span><span class="sxs-lookup"><span data-stu-id="ac630-137">Most types in the <xref:System.Globalization?displayProperty=fullName> namespace</span></span>
- <span data-ttu-id="ac630-138"><xref:System.Array.Sort%2A?displayProperty=fullName>（对字符串数组进行排序时）</span><span class="sxs-lookup"><span data-stu-id="ac630-138"><xref:System.Array.Sort%2A?displayProperty=fullName> (when sorting an array of strings)</span></span>
- <span data-ttu-id="ac630-139"><xref:System.Collections.Generic.List%601.Sort?displayProperty=fullName>（当列表元素为字符串时）</span><span class="sxs-lookup"><span data-stu-id="ac630-139"><xref:System.Collections.Generic.List%601.Sort?displayProperty=fullName> (when the list elements are strings)</span></span>
- <span data-ttu-id="ac630-140"><xref:System.Collections.Generic.SortedDictionary%602?displayProperty=fullName>（当键为字符串时）</span><span class="sxs-lookup"><span data-stu-id="ac630-140"><xref:System.Collections.Generic.SortedDictionary%602?displayProperty=fullName> (when the keys are strings)</span></span>
- <span data-ttu-id="ac630-141"><xref:System.Collections.Generic.SortedList%602?displayProperty=fullName>（当键为字符串时）</span><span class="sxs-lookup"><span data-stu-id="ac630-141"><xref:System.Collections.Generic.SortedList%602?displayProperty=fullName> (when the keys are strings)</span></span>
- <span data-ttu-id="ac630-142"><xref:System.Collections.Generic.SortedSet%601?displayProperty=fullName>（当集包含字符串时）</span><span class="sxs-lookup"><span data-stu-id="ac630-142"><xref:System.Collections.Generic.SortedSet%601?displayProperty=fullName> (when the set contains strings)</span></span>

<!--

### Affected APIs

- ``T:System.Span`1``
- `T:System.String`
- `N:System.Globalization`
- `Overload:System.Array.Sort`
- ``M:System.Collections.Generic.List`1.Sort``
- ``T:System.Collections.Generic.SortedDictionary`2``
- ``T:System.Collections.Generic.SortedList`2``
- ``T:System.Collections.Generic.SortedSet`1``

### Category

- Core .NET libraries
- Globalization

-->
