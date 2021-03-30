---
title: 将库移植到 .NET
description: 了解如何将 .NET Framework 中的库项目移植到 .NET。
author: cartermp
ms.date: 03/04/2021
ms.openlocfilehash: 19ee6ab15597b3c99b39b47ad55ac72a3f9d681a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873674"
---
# <a name="port-net-framework-libraries-to-net"></a><span data-ttu-id="ab3ad-103">将 .NET Framework 库移植到 .NET 中</span><span class="sxs-lookup"><span data-stu-id="ab3ad-103">Port .NET Framework libraries to .NET</span></span>

<span data-ttu-id="ab3ad-104">了解如何将 .NET Framework 库代码移植到 .NET，它在其中跨平台运行并扩展使用它的应用的范围。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-104">Learn how to port .NET Framework library code to .NET, where it runs cross-platform and expands the reach of the apps that use it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab3ad-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="ab3ad-105">Prerequisites</span></span>

<span data-ttu-id="ab3ad-106">本文假定你：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-106">This article assumes that you:</span></span>

- <span data-ttu-id="ab3ad-107">正在使用 Visual Studio 2019 或更高版本？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-107">Are using Visual Studio 2019 or later?</span></span>
  - <span data-ttu-id="ab3ad-108">.NET 5 需要 Visual Studio 2019 (v16.9) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-108">.NET 5 requires Visual Studio 2019 (v16.9) or later.</span></span>
  - <span data-ttu-id="ab3ad-109">.NET Core 3.1 需要 Visual Studio 2019 (v16.4) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-109">.NET Core 3.1 requires Visual Studio 2019 (v16.4) or later.</span></span>
- <span data-ttu-id="ab3ad-110">了解[推荐的移植过程](index.md)。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-110">Understand the [recommended porting process](index.md).</span></span>
- <span data-ttu-id="ab3ad-111">已解决与[第三方依赖项](third-party-deps.md)有关的任何问题。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-111">Have resolved any issues with [third-party dependencies](third-party-deps.md).</span></span>

<span data-ttu-id="ab3ad-112">熟悉以下文章的内容：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-112">Familiarize yourself with the content of the following articles:</span></span>

- <span data-ttu-id="ab3ad-113">[.NET Standard。](../../standard/net-standard.md)</span><span class="sxs-lookup"><span data-stu-id="ab3ad-113">[.NET Standard.](../../standard/net-standard.md)</span></span>\
<span data-ttu-id="ab3ad-114">本文介绍了适用于所有 .NET 实现代码的 .NET API 正式规范。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-114">This article describes the formal specification of .NET APIs that are intended to be available on all .NET implementations.</span></span>

- <span data-ttu-id="ab3ad-115">[使用 .NET CLI 开发库。](../tutorials/libraries.md)</span><span class="sxs-lookup"><span data-stu-id="ab3ad-115">[Develop libraries with the .NET CLI.](../tutorials/libraries.md)</span></span>\
<span data-ttu-id="ab3ad-116">本文介绍如何使用 .NET CLI 编写库。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-116">This article explains how to write libraries using the .NET CLI.</span></span>

- <span data-ttu-id="ab3ad-117">[.NET 项目 SDK。](../project-sdk/overview.md)</span><span class="sxs-lookup"><span data-stu-id="ab3ad-117">[.NET project SDKs.](../project-sdk/overview.md)</span></span>\
<span data-ttu-id="ab3ad-118">本文介绍 SDK 样式的项目文件格式。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-118">This article describes the SDK-style project file format.</span></span>

- <span data-ttu-id="ab3ad-119">[分析依赖项以将代码从 .NET Framework 移植到 .NET 中。](third-party-deps.md)</span><span class="sxs-lookup"><span data-stu-id="ab3ad-119">[Analyze your dependencies to port code from .NET Framework to .NET.](third-party-deps.md)</span></span>\
<span data-ttu-id="ab3ad-120">本文介绍了第三方依赖项的可移植性及 NuGet 包依赖项无法在 .NET 上运行时要执行的操作。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-120">This article discusses the portability of third-party dependencies, and what to do when a NuGet package dependency doesn't run on .NET.</span></span>

## <a name="retarget-to-net-framework-472"></a><span data-ttu-id="ab3ad-121">重定向到 .NET Framework 4.7.2</span><span class="sxs-lookup"><span data-stu-id="ab3ad-121">Retarget to .NET Framework 4.7.2</span></span>

<span data-ttu-id="ab3ad-122">如果代码不面向 .NET Framework 4.7.2，建议重定向到 .NET Framework 4.7.2。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-122">If your code isn't targeting .NET Framework 4.7.2, we recommended that you retarget to .NET Framework 4.7.2.</span></span> <span data-ttu-id="ab3ad-123">在 .NET Standard 不支持现有 API 情况下，这可确保最新备用 API 的可用性。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-123">This ensures the availability of the latest API alternatives for cases where .NET Standard doesn't support existing APIs.</span></span>

<span data-ttu-id="ab3ad-124">对于每个想要移植的项目，请在 Visual Studio 中执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-124">For each of the projects you wish to port, do the following in Visual Studio:</span></span>

01. <span data-ttu-id="ab3ad-125">右键单击该项目，然后选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-125">Right-click on the project and select **Properties**.</span></span>
01. <span data-ttu-id="ab3ad-126">在“目标框架”下拉列表中，选择“.NET Framework 4.7.2”。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-126">In the **Target Framework** dropdown, select **.NET Framework 4.7.2**.</span></span>
01. <span data-ttu-id="ab3ad-127">重新编译该项目。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-127">Recompile the project.</span></span>

<span data-ttu-id="ab3ad-128">因为项目现在面向 .NET Framework 4.7.2，因此可使用该版本的 .NET Framework 作为移植代码的基准。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-128">Because your projects now target .NET Framework 4.7.2, use that version of the .NET Framework as your base for porting code.</span></span>

## <a name="determine-portability"></a><span data-ttu-id="ab3ad-129">确定可移植性</span><span class="sxs-lookup"><span data-stu-id="ab3ad-129">Determine portability</span></span>

<span data-ttu-id="ab3ad-130">下一步是运行 API 可移植性分析器 (ApiPort) 生成可供分析的可移植性报表。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-130">The next step is to run the API Portability Analyzer (ApiPort) to generate a portability report for analysis.</span></span>

<span data-ttu-id="ab3ad-131">确保了解 [API 可移植性分析器 (ApiPort)](../../standard/analyzers/portability-analyzer.md) 及如何生成用于面向 .NET 的可移植性报表。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-131">Make sure you understand the [API Portability Analyzer (ApiPort)](../../standard/analyzers/portability-analyzer.md) and how to generate portability reports for targeting .NET.</span></span> <span data-ttu-id="ab3ad-132">执行此操作的方式可能取决于需求和个人偏好。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-132">How you do this likely varies based on your needs and personal tastes.</span></span> <span data-ttu-id="ab3ad-133">以下部分详细介绍了几种不同的方法。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-133">The following sections detail a few different approaches.</span></span> <span data-ttu-id="ab3ad-134">用户可能会发现自己根据生成代码的方式混合使用了这些方法中的步骤。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-134">You may find yourself mixing steps of these approaches depending on how your code is structured.</span></span>

### <a name="deal-primarily-with-the-compiler"></a><span data-ttu-id="ab3ad-135">主要处理编译器</span><span class="sxs-lookup"><span data-stu-id="ab3ad-135">Deal primarily with the compiler</span></span>

<span data-ttu-id="ab3ad-136">此方法可能较适合小项目或不会用很多 .NET Framework API 的项目。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-136">This approach works well for small projects or projects that don't use many .NET Framework APIs.</span></span> <span data-ttu-id="ab3ad-137">此方法很简单：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-137">The approach is simple:</span></span>

01. <span data-ttu-id="ab3ad-138">可选择在项目上运行 ApiPort。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-138">Optionally, run ApiPort on your project.</span></span> <span data-ttu-id="ab3ad-139">若运行 ApiPort，则从报告获取有关需要解决的问题的信息。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-139">If you run ApiPort, gain knowledge from the report on issues you'll need to address.</span></span>
01. <span data-ttu-id="ab3ad-140">将所有代码复制到新的 .NET 项目。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-140">Copy all of your code over into a new .NET project.</span></span>
01. <span data-ttu-id="ab3ad-141">查看可移植性报表（如果已生成）时，解决编译器错误，直至项目完全得到编译。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-141">While referring to the portability report (if generated), solve compiler errors until the project fully compiles.</span></span>

<span data-ttu-id="ab3ad-142">尽管它是非结构化的，但这种以代码为中心的方法通常可以快速解决问题。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-142">Although it's unstructured, this code-focused approach often resolves issues quickly.</span></span> <span data-ttu-id="ab3ad-143">只包含数据模型的项目可能是此方法的理想选择。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-143">A project that contains only data models might be an ideal candidate for this approach.</span></span>

### <a name="stay-on-the-net-framework-until-portability-issues-are-resolved"></a><span data-ttu-id="ab3ad-144">可移植性问题得到解决前停留在 .NET Framework 上</span><span class="sxs-lookup"><span data-stu-id="ab3ad-144">Stay on the .NET Framework until portability issues are resolved</span></span>

<span data-ttu-id="ab3ad-145">如果更希望拥有在整个过程期间编译的代码，此方法可能是最佳选择。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-145">This approach might be the best if you prefer to have code that compiles during the entire process.</span></span> <span data-ttu-id="ab3ad-146">该方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-146">The approach is as follows:</span></span>

01. <span data-ttu-id="ab3ad-147">在项目上运行 ApiPort。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-147">Run ApiPort on a project.</span></span>
01. <span data-ttu-id="ab3ad-148">通过使用可移植的不同 API 解决问题。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-148">Address issues by using different APIs that are portable.</span></span>
01. <span data-ttu-id="ab3ad-149">记录阻止你使用直接替代方案的所有区域。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-149">Take note of any areas where you're prevented from using a direct alternative.</span></span>
01. <span data-ttu-id="ab3ad-150">对所有要移植的项目重复前面的步骤，直到确信每个项目都做好被复制到新的 .NET 项目中的准备。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-150">Repeat the prior steps for all projects you're porting until you're confident each is ready to be copied over into a new .NET project.</span></span>
01. <span data-ttu-id="ab3ad-151">将代码复制到新的 .NET 项目中。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-151">Copy the code into a new .NET project.</span></span>
01. <span data-ttu-id="ab3ad-152">解决所有已记录的不存在直接替代方案的问题。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-152">Work out any issues where you noted that a direct alternative doesn't exist.</span></span>

<span data-ttu-id="ab3ad-153">这种谨慎的方法比单纯解决编译器错误更有条理，但相对而言，它仍以代码为中心，且优点是始终拥有编译的代码。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-153">This careful approach is more structured than simply working out compiler errors, but it's still relatively code-focused and has the benefit of always having code that compiles.</span></span> <span data-ttu-id="ab3ad-154">解决不能通过只使用另一个 API 解决的某些问题的方法大不相同。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-154">The way you resolve certain issues that couldn't be addressed by just using another API varies greatly.</span></span> <span data-ttu-id="ab3ad-155">你可能会发现对于某些项目，需要制定更全面的计划，这将在下一种方法中介绍。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-155">You may find that you need to develop a more comprehensive plan for certain projects, which is covered in the next approach.</span></span>

### <a name="develop-a-comprehensive-plan-of-attack"></a><span data-ttu-id="ab3ad-156">制定全面的攻击计划</span><span class="sxs-lookup"><span data-stu-id="ab3ad-156">Develop a comprehensive plan of attack</span></span>

<span data-ttu-id="ab3ad-157">此方法可能最适合大型或更复杂的项目，在这种情况下，为支持 .NET，可能必需重构代码或将某些代码区域完全重写。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-157">This approach might be best for larger and more complex projects, where restructuring code or completely rewriting certain areas of code might be necessary to support .NET.</span></span> <span data-ttu-id="ab3ad-158">该方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-158">The approach is as follows:</span></span>

01. <span data-ttu-id="ab3ad-159">在项目上运行 ApiPort。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-159">Run ApiPort on a project.</span></span>
01. <span data-ttu-id="ab3ad-160">了解每个非可移植类型使用的位置以及位置对整体可移植性的影响。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-160">Understand where each non-portable type is used and how that affects overall portability.</span></span>

    - <span data-ttu-id="ab3ad-161">了解这些类型的特性。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-161">Understand the nature of those types.</span></span> <span data-ttu-id="ab3ad-162">它们是否数量少，但使用频繁？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-162">Are they small in number but used frequently?</span></span> <span data-ttu-id="ab3ad-163">它们是否数量大，但使用不频繁？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-163">Are they large in number but used infrequently?</span></span> <span data-ttu-id="ab3ad-164">它们是串联使用，还是在整个代码中传播？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-164">Is their use concentrated, or is it spread throughout your code?</span></span>
    - <span data-ttu-id="ab3ad-165">是否可以轻松隔离不可移植的代码，以便可以更有效地处理它？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-165">Is it easy to isolate code that isn't portable so that you can deal with it more effectively?</span></span>
    - <span data-ttu-id="ab3ad-166">是否需要重构代码？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-166">Do you need to refactor your code?</span></span>
    - <span data-ttu-id="ab3ad-167">对于这些不可移植的类型，是否存在可完成相同任务的备用 API？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-167">For those types that aren't portable, are there alternative APIs that accomplish the same task?</span></span> <span data-ttu-id="ab3ad-168">例如，如果使用的是 <xref:System.Net.WebClient> 类，则改用 <xref:System.Net.Http.HttpClient> 类。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-168">For example, if you're using the <xref:System.Net.WebClient> class, use the <xref:System.Net.Http.HttpClient> class instead.</span></span>
    - <span data-ttu-id="ab3ad-169">是否存在其他可用于完成任务的可移植 API，即使它不是直接替代 API？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-169">Are there different portable APIs available to accomplish a task, even if it's not a drop-in replacement?</span></span> <span data-ttu-id="ab3ad-170">例如，如果使用 <xref:System.Xml.Schema.XmlSchema> 来分析 XML，但是无需 XML 架构发现，则可使用 <xref:System.Xml.Linq> API 并自行实现分析，而不是依赖于某个 API。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-170">For example, if you're using <xref:System.Xml.Schema.XmlSchema> to parse XML but don't require XML schema discovery, you could use <xref:System.Xml.Linq> APIs and implement parsing yourself instead of relying on an API.</span></span>

01. <span data-ttu-id="ab3ad-171">如果具有难以移植的程序集，是否值得将其暂时留在 .NET Framework 上？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-171">If you have assemblies that are difficult to port, is it worth leaving them on .NET Framework for now?</span></span> <span data-ttu-id="ab3ad-172">以下是一些需要考虑的事项：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-172">Here are some things to consider:</span></span>

    - <span data-ttu-id="ab3ad-173">库中可能具有某些与 .NET 不兼容的功能，因为它过于依赖 .NET Framework 或 Windows 特定的功能。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-173">You may have some functionality in your library that's incompatible with .NET because it relies too heavily on .NET Framework or Windows-specific functionality.</span></span> <span data-ttu-id="ab3ad-174">是否值得暂时搁置该功能，并直到资源可用于移植这些功能，才发布具有较少功能的库的临时 .NET 版本？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-174">Is it worth leaving that functionality behind for now and releasing a temporary .NET version of your library with fewer features until resources are available to port the features?</span></span>
    - <span data-ttu-id="ab3ad-175">重构是否有用？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-175">Would a refactor help?</span></span>

01. <span data-ttu-id="ab3ad-176">编写自己对不可用 .NET Framework API 的实现是否合理？</span><span class="sxs-lookup"><span data-stu-id="ab3ad-176">Is it reasonable to write your own implementation of an unavailable .NET Framework API?</span></span>

    <span data-ttu-id="ab3ad-177">可以考虑复制、修改，并使用 [.NET Framework 参考源](https://github.com/Microsoft/referencesource)中的代码。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-177">You could consider copying, modifying, and using code from the [.NET Framework reference source](https://github.com/Microsoft/referencesource).</span></span> <span data-ttu-id="ab3ad-178">参考源代码已在 [MIT 许可证](https://github.com/Microsoft/referencesource/blob/master/LICENSE.txt)下获得许可，因此可以自由选择将此源作为自己代码的基础。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-178">The reference source code is licensed under the [MIT License](https://github.com/Microsoft/referencesource/blob/master/LICENSE.txt), so you have significant freedom to use the source as a basis for your own code.</span></span> <span data-ttu-id="ab3ad-179">只需确保在代码中正确设置 Microsoft。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-179">Just be sure to properly attribute Microsoft in your code.</span></span>

01. <span data-ttu-id="ab3ad-180">根据不同项目的需要，重复此过程。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-180">Repeat this process as needed for different projects.</span></span>

<span data-ttu-id="ab3ad-181">分析阶段可能需要一些时间，具体取决于代码库的大小。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-181">The analysis phase could take some time depending on the size of your codebase.</span></span> <span data-ttu-id="ab3ad-182">在此阶段花费时间（尤其是在具有复杂的代码库时），全面了解所需的更改范围并制定计划，最终通常可节省时间。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-182">Spending time in this phase to thoroughly understand the scope of changes needed and to develop a plan usually saves you time in the end, particularly if you have a complex codebase.</span></span>

<span data-ttu-id="ab3ad-183">计划可能包括对代码库做重大更改，同时仍面向 .NET Framework 4.7.2。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-183">Your plan could involve making significant changes to your codebase while still targeting .NET Framework 4.7.2.</span></span> <span data-ttu-id="ab3ad-184">这是前一种方法更有条理的版本。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-184">This is a more structured version of the previous approach.</span></span> <span data-ttu-id="ab3ad-185">着手执行计划的方式具体取决于代码库。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-185">How you go about executing your plan is dependent on your codebase.</span></span>

### <a name="mixed-approach"></a><span data-ttu-id="ab3ad-186">混合方法</span><span class="sxs-lookup"><span data-stu-id="ab3ad-186">Mixed approach</span></span>

<span data-ttu-id="ab3ad-187">在每个项目的基础上，可能会将上述方法进行混合。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-187">It's likely that you'll mix the above approaches on a per-project basis.</span></span> <span data-ttu-id="ab3ad-188">执行哪些操作对你和代码库最有意义。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-188">Do what makes the most sense to you and for your codebase.</span></span>

## <a name="port-your-tests"></a><span data-ttu-id="ab3ad-189">移植测试</span><span class="sxs-lookup"><span data-stu-id="ab3ad-189">Port your tests</span></span>

<span data-ttu-id="ab3ad-190">要确保移植代码后一切正常的最佳方式是在将代码移植到 .NET 时进行测试。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-190">The best way to make sure everything works when you've ported your code is to test your code as you port it to .NET.</span></span> <span data-ttu-id="ab3ad-191">为此，需要使用将针对 .NET 生成和运行测试的测试框架。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-191">To do this, you'll need to use a testing framework that builds and runs tests for .NET.</span></span> <span data-ttu-id="ab3ad-192">当前，有三个选择：</span><span class="sxs-lookup"><span data-stu-id="ab3ad-192">Currently, you have three options:</span></span>

- [<span data-ttu-id="ab3ad-193">xUnit</span><span class="sxs-lookup"><span data-stu-id="ab3ad-193">xUnit</span></span>](https://xunit.net/)
  - [<span data-ttu-id="ab3ad-194">入门</span><span class="sxs-lookup"><span data-stu-id="ab3ad-194">Getting Started</span></span>](https://xunit.net/docs/getting-started/netcore/cmdline)
  - [<span data-ttu-id="ab3ad-195">将 MSTest 项目转换为 xUnit 的工具</span><span class="sxs-lookup"><span data-stu-id="ab3ad-195">Tool to convert an MSTest project to xUnit</span></span>](https://github.com/dotnet/codeformatter/tree/main/src/XUnitConverter)
- [<span data-ttu-id="ab3ad-196">NUnit</span><span class="sxs-lookup"><span data-stu-id="ab3ad-196">NUnit</span></span>](https://nunit.org/)
  - [<span data-ttu-id="ab3ad-197">入门</span><span class="sxs-lookup"><span data-stu-id="ab3ad-197">Getting Started</span></span>](https://github.com/nunit/docs/wiki/Installation)
  - [<span data-ttu-id="ab3ad-198">关于从 MSTest 迁移到 NUnit 的博客文章</span><span class="sxs-lookup"><span data-stu-id="ab3ad-198">Blog post about migrating from MSTest to NUnit</span></span>](https://www.florian-rappl.de/News/Page/275/convert-mstest-to-nunit)
- [<span data-ttu-id="ab3ad-199">MSTest</span><span class="sxs-lookup"><span data-stu-id="ab3ad-199">MSTest</span></span>](/visualstudio/test/unit-test-basics)

## <a name="recommended-approach"></a><span data-ttu-id="ab3ad-200">推荐的方法</span><span class="sxs-lookup"><span data-stu-id="ab3ad-200">Recommended approach</span></span>

<span data-ttu-id="ab3ad-201">从根本上讲，移植工作在很大程度上取决于生成 .NET Framework 代码的方式。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-201">Ultimately, the porting effort depends heavily on how your .NET Framework code is structured.</span></span> <span data-ttu-id="ab3ad-202">移植代码的一个好方法是从库的基项开始，这是代码的基础组件。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-202">A good way to port your code is to begin with the *base* of your library, which is the foundational components of your code.</span></span> <span data-ttu-id="ab3ad-203">这可能是数据模型或某些其他内容直接或间接使用的基本类和方法。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-203">This might be data models or some other foundational classes and methods that everything else uses directly or indirectly.</span></span>

01. <span data-ttu-id="ab3ad-204">移植测试项目，该项目测试当前正在移植的库层。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-204">Port the test project that tests the layer of your library that you're currently porting.</span></span>
01. <span data-ttu-id="ab3ad-205">将库中的基项复制到新的 .NET 项目中，然后选择想要支持的 .NET Standard 版本。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-205">Copy over the base of your library into a new .NET project and select the version of .NET Standard you wish to support.</span></span>
01. <span data-ttu-id="ab3ad-206">进行任何所需的更改，使代码进行编译。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-206">Make any changes needed to get the code to compile.</span></span> <span data-ttu-id="ab3ad-207">大部分内容可能会要求将 NuGet 包依赖项添加到 csproj 文件。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-207">Much of this may require adding NuGet package dependencies to your *csproj* file.</span></span>
01. <span data-ttu-id="ab3ad-208">运行测试并进行任何所需调整。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-208">Run the tests and make any needed adjustments.</span></span>
01. <span data-ttu-id="ab3ad-209">选择下一层代码进行移植，并重复前面的步骤。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-209">Pick the next layer of code to port over and repeat the prior steps.</span></span>

<span data-ttu-id="ab3ad-210">如果从库的基项开始并从基项向外移动并根据需要测试每一层，移植将是一个系统化的过程，在这种情况下，问题可以一次隔离到一层代码中。</span><span class="sxs-lookup"><span data-stu-id="ab3ad-210">If you start with the base of your library and move outward from the base and test each layer as needed, porting is a systematic process where problems are isolated to one layer of code at a time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab3ad-211">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ab3ad-211">Next steps</span></span>

>[!div class="nextstepaction"]
>[<span data-ttu-id="ab3ad-212">整理 .NET 的项目</span><span class="sxs-lookup"><span data-stu-id="ab3ad-212">Organize projects for .NET</span></span>](project-structure.md)
