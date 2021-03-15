---
title: 从 .NET Framework 移植到 .NET 5
description: 了解移植过程，并发现可能会对将 .NET Framework 项目移植到 .NET 5（及 .NET Core 3.1）有用的工具。
author: adegeo
ms.date: 03/03/2020
no-loc:
- package.config
- PackageReference
ms.openlocfilehash: 8515689cf4a1be917f12bb8f3315cda89988d773
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605043"
---
# <a name="overview-of-porting-from-net-framework-to-net"></a><span data-ttu-id="ebb1e-103">有关从 .NET Framework 移植到 .NET 的概述</span><span class="sxs-lookup"><span data-stu-id="ebb1e-103">Overview of porting from .NET Framework to .NET</span></span>

<span data-ttu-id="ebb1e-104">本文概述了在将代码从 .NET Framework 移植到 .NET（旧称为 .NET Core）时应考虑的事项。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-104">This article provides an overview of what you should consider when porting your code from .NET Framework to .NET (formerly named .NET Core).</span></span> <span data-ttu-id="ebb1e-105">对于许多项目，从 .NET Framework 移植到 .NET 是相对简单的。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-105">Porting to .NET from .NET Framework for many projects is relatively straightforward.</span></span> <span data-ttu-id="ebb1e-106">项目的复杂性决定了在项目文件的初始迁移之后要做多少工作。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-106">The complexity of your projects dictates how much work you'll do after the initial migration of the project files.</span></span>

<span data-ttu-id="ebb1e-107">应用模型在 .NET 中可用的项目（如库、控制台应用和桌面应用）通常不需要太大的更改。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-107">Projects where the app-model is available in .NET (such as libraries, console apps, and desktop apps) usually require little change.</span></span> <span data-ttu-id="ebb1e-108">需要使用新应用模型的项目（如从 ASP.NET 迁移到 ASP.NET Core）需要的工作要多一点。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-108">Projects that require a new app model, such as moving to ASP.NET Core from ASP.NET, require more work.</span></span> <span data-ttu-id="ebb1e-109">旧应用模型中的很多模式都有可以在转换过程中使用的等效项。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-109">Many patterns from the old app model have equivalents that can be used during the conversion.</span></span>

## <a name="unavailable-technologies"></a><span data-ttu-id="ebb1e-110">不可用的技术</span><span class="sxs-lookup"><span data-stu-id="ebb1e-110">Unavailable technologies</span></span>

<span data-ttu-id="ebb1e-111">.NET Framework 中有一些技术在 .NET 中是不存在：</span><span class="sxs-lookup"><span data-stu-id="ebb1e-111">There are a few technologies in .NET Framework that don't exist in .NET:</span></span>

- [<span data-ttu-id="ebb1e-112">应用程序域</span><span class="sxs-lookup"><span data-stu-id="ebb1e-112">Application domains</span></span>](net-framework-tech-unavailable.md#application-domains)

  <span data-ttu-id="ebb1e-113">不支持创建额外应用程序域。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-113">Creating additional application domains isn't supported.</span></span> <span data-ttu-id="ebb1e-114">对于代码隔离，将流程或容器用作备用。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-114">For code isolation, use separate processes or containers as an alternative.</span></span>

- [<span data-ttu-id="ebb1e-115">远程处理</span><span class="sxs-lookup"><span data-stu-id="ebb1e-115">Remoting</span></span>](net-framework-tech-unavailable.md#remoting)

  <span data-ttu-id="ebb1e-116">远程处理用于跨不再受支持的应用程序域进行通信。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-116">Remoting is used for communicating across application domains, which are no longer supported.</span></span> <span data-ttu-id="ebb1e-117">对于跨进程通信，可将进程间通信 (IPC) 机制视为远程处理的备用方案，如 <xref:System.IO.Pipes> 类或 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 类。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-117">For communication across processes, consider inter-process communication (IPC) mechanisms as an alternative to remoting, such as the <xref:System.IO.Pipes> class or the <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> class.</span></span>

- [<span data-ttu-id="ebb1e-118">代码访问安全性 (CAS)</span><span class="sxs-lookup"><span data-stu-id="ebb1e-118">Code access security (CAS)</span></span>](net-framework-tech-unavailable.md#code-access-security-cas)

  <span data-ttu-id="ebb1e-119">CAS 是受 .NET Framework 支持、但在 .NET Framework 4.0 中已停用的沙盒技术。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-119">CAS was a sandboxing technique supported by .NET Framework but deprecated in .NET Framework 4.0.</span></span> <span data-ttu-id="ebb1e-120">它已被 Security Transparency 取代，并且在 .NET 中不受支持。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-120">It was replaced by Security Transparency and it's not supported in .NET.</span></span> <span data-ttu-id="ebb1e-121">请改用操作系统提供的安全边界，如虚拟化、容器或用户帐户。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-121">Instead, use security boundaries provided by the operating system, such as virtualization, containers, or user accounts.</span></span>

- [<span data-ttu-id="ebb1e-122">安全透明度</span><span class="sxs-lookup"><span data-stu-id="ebb1e-122">Security transparency</span></span>](net-framework-tech-unavailable.md#security-transparency)

  <span data-ttu-id="ebb1e-123">与 CAS 类似，这种沙盒技术不再被推荐用于 .NET Framework 应用程序，而且在 .NET 中也不受支持。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-123">Similar to CAS, this sandboxing technique is no longer recommended for .NET Framework applications and it's not supported in .NET.</span></span> <span data-ttu-id="ebb1e-124">请改用操作系统提供的安全边界，如虚拟化、容器或用户帐户。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-124">Instead, use security boundaries provided by the operating system, such as virtualization, containers, or user accounts.</span></span>
  
- <xref:System.EnterpriseServices?displayProperty=fullName>

  <span data-ttu-id="ebb1e-125">.NET 不支持 <xref:System.EnterpriseServices?displayProperty=fullName> (COM+)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-125"><xref:System.EnterpriseServices?displayProperty=fullName> (COM+) isn't supported in .NET.</span></span>

- <span data-ttu-id="ebb1e-126">Windows Workflow Foundation (WF) 和 Windows Communication Foundation (WCF)</span><span class="sxs-lookup"><span data-stu-id="ebb1e-126">Windows Workflow Foundation (WF) and Windows Communication Foundation (WCF)</span></span>

  <span data-ttu-id="ebb1e-127">.NET 5 及更高版本（包括 .NET Core）不支持 WF 和 WCF。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-127">WF and WCF aren't supported in .NET 5+ (including .NET Core).</span></span> <span data-ttu-id="ebb1e-128">有关替代方法，请参阅 [CoreWF](https://github.com/UiPath/corewf) 和 [CoreWCF](https://github.com/CoreWCF/CoreWCF)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-128">For alternatives, see [CoreWF](https://github.com/UiPath/corewf) and [CoreWCF](https://github.com/CoreWCF/CoreWCF).</span></span>

<span data-ttu-id="ebb1e-129">若要详细了解这些不受支持的技术，请参阅 [.NET Framework 技术在 .NET Core 和 .NET 5 及更高版本上不可用](net-framework-tech-unavailable.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-129">For more information about these unsupported technologies, see [.NET Framework technologies unavailable on .NET Core and .NET 5+](net-framework-tech-unavailable.md).</span></span>

## <a name="windows-desktop-technologies"></a><span data-ttu-id="ebb1e-130">Windows 桌面技术</span><span class="sxs-lookup"><span data-stu-id="ebb1e-130">Windows desktop technologies</span></span>

<span data-ttu-id="ebb1e-131">许多为 .NET Framework 创建的应用程序都使用桌面技术，如 Windows 窗体或 Windows Presentation Foundation (WPF)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-131">Many applications created for .NET Framework use a desktop technology such as Windows Forms or Windows Presentation Foundation (WPF).</span></span> <span data-ttu-id="ebb1e-132">虽然 Windows 窗体和 WPF 均已移植到 .NET 中，但这些仍是仅适用于 Windows 的技术。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-132">Both Windows Forms and WPF have been ported to .NET, but these remain Windows-only technologies.</span></span>

<span data-ttu-id="ebb1e-133">在迁移 Windows 窗体或 WPF 应用程序之前，请先考虑以下依赖项：</span><span class="sxs-lookup"><span data-stu-id="ebb1e-133">Consider the following dependencies before you migrate a Windows Forms or WPF application:</span></span>

01. <span data-ttu-id="ebb1e-134">适用于 .NET 的项目文件使用与 .NET Framework 不同的格式。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-134">Project files for .NET use a different format than .NET Framework.</span></span>
01. <span data-ttu-id="ebb1e-135">你的项目可能会使用在 .NET 中不可用的 API。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-135">Your project may use an API that isn't available in .NET.</span></span>
01. <span data-ttu-id="ebb1e-136">第三方控件和库可能还没有移植到 .NET 中，仍只对 .NET Framework 可用。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-136">3rd-party controls and libraries may not have been ported to .NET and remain only available to .NET Framework.</span></span>
01. <span data-ttu-id="ebb1e-137">你的项目使用在 .NET 中[不再可用的技术](net-framework-tech-unavailable.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-137">Your project uses a [technology that is no longer available](net-framework-tech-unavailable.md) in .NET.</span></span>

<span data-ttu-id="ebb1e-138">.NET 使用 Windows 窗体和 WPF 的开放源代码版本，并对 .NET Framework 进行了增强。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-138">.NET uses the open-source versions of Windows Forms and WPF and includes enhancements over .NET Framework.</span></span>

<span data-ttu-id="ebb1e-139">有关将桌面应用程序迁移到 .NET 5 的教程，请参阅以下文章之一：</span><span class="sxs-lookup"><span data-stu-id="ebb1e-139">For tutorials on migrating your desktop application to .NET 5, see one of the following articles:</span></span>

- [<span data-ttu-id="ebb1e-140">将 .NET Framework WPF 应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="ebb1e-140">Migrate .NET Framework WPF apps to .NET</span></span>](/dotnet/desktop/wpf/migration/convert-project-from-net-framework?view=netdesktop-5.0&preserve-view=true)
- [<span data-ttu-id="ebb1e-141">将 .NET Framework Windows 窗体应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="ebb1e-141">Migrate .NET Framework Windows Forms apps to .NET</span></span>](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)

## <a name="windows-specific-apis"></a><span data-ttu-id="ebb1e-142">特定于 Windows 的 API</span><span class="sxs-lookup"><span data-stu-id="ebb1e-142">Windows-specific APIs</span></span>

<span data-ttu-id="ebb1e-143">应用程序仍可以在 .NET 支持的平台上对本机库进行平台调用。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-143">Applications can still P/Invoke native libraries on platforms supported by .NET.</span></span> <span data-ttu-id="ebb1e-144">这项技术并不仅限于 Windows。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-144">This technology isn't limited to Windows.</span></span> <span data-ttu-id="ebb1e-145">但是，如果你引用的库是特定于 Windows 的（如 user32.dll 或 kernal32.dll），那么代码只能在 Windows 上正常运行。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-145">However, if the library you're referencing is Windows-specific, such as a _user32.dll_ or _kernal32.dll_, then the code only works on Windows.</span></span> <span data-ttu-id="ebb1e-146">对于想要在其上运行应用的每个平台，你都必须查找特定于平台的版本，或者让你的代码足够通用以在所有平台上运行。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-146">For each platform you want your app to run on, you'll have to either find platform-specific versions, or make your code generic enough to run on all platforms.</span></span>

<span data-ttu-id="ebb1e-147">当将应用程序从 .NET Framework 移植到 .NET 时，应用程序可能使用了随 .NET Framework 一起分发的库。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-147">When porting an application from .NET Framework to .NET, your application probably used a library provided distributed with the .NET Framework.</span></span> <span data-ttu-id="ebb1e-148">许多在 .NET Framework 中可用的 API 都没有移植到 .NET 中，因为它们依赖特定于 Windows 的技术，如 Windows Registry 或 GDI+ 绘图模型。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-148">Many APIs that were available in .NET Framework weren't ported to .NET because they relied on Windows-specific technology, such as the Windows Registry or the GDI+ drawing model.</span></span>

<span data-ttu-id="ebb1e-149">Windows Compatibility Pack 为 .NET 提供了大部分的 .NET Framework API 面，并通过 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)提供。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-149">The **Windows Compatibility Pack** provides a large portion of the .NET Framework API surface to .NET and is provided via the [Microsoft.Windows.Compatibility NuGet package](https://www.nuget.org/packages/Microsoft.Windows.Compatibility).</span></span>

<span data-ttu-id="ebb1e-150">有关详细信息，请参阅[使用 Windows Compatibility Pack 将代码移植到 .NET 中](windows-compat-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-150">For more information, see [Use the Windows Compatibility Pack to port code to .NET](windows-compat-pack.md).</span></span>

## <a name="net-framework-compatibility-mode"></a><span data-ttu-id="ebb1e-151">.NET Framework 兼容性模式</span><span class="sxs-lookup"><span data-stu-id="ebb1e-151">.NET Framework compatibility mode</span></span>

<span data-ttu-id="ebb1e-152">.NET Framework 兼容性模式是在 .NET Standard 2.0 中引入的。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-152">The .NET Framework compatibility mode was introduced in .NET Standard 2.0.</span></span> <span data-ttu-id="ebb1e-153">使用此兼容性模式，.NET Standard 和 .NET 5 及更高版本（以及 .NET Core 3.1）项目可以在仅适用于 Windows 的情况下引用 .NET Framework 库。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-153">This compatibility mode allows .NET Standard and .NET 5+ (and .NET Core 3.1) projects to reference .NET Framework libraries on Windows-only.</span></span> <span data-ttu-id="ebb1e-154">引用 .NET Framework 库不适用于所有项目（如库使用 Windows Presentation Foundation (WPF) API 时），但它的开启了很多移植方案。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-154">Referencing .NET Framework libraries doesn't work for all projects, such as if the library uses Windows Presentation Foundation (WPF) APIs, but it does unblock many porting scenarios.</span></span> <span data-ttu-id="ebb1e-155">有关详细信息，请参阅[分析依赖项以将代码从 .NET Framework 移植到 .NET 中](third-party-deps.md#net-framework-compatibility-mode)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-155">For more information, see the [Analyze your dependencies to port code from .NET Framework to .NET](third-party-deps.md#net-framework-compatibility-mode).</span></span>

## <a name="cross-platform"></a><span data-ttu-id="ebb1e-156">跨平台</span><span class="sxs-lookup"><span data-stu-id="ebb1e-156">Cross-platform</span></span>

<span data-ttu-id="ebb1e-157">.NET（旧称为 .NET Core）是为跨平台而设计的。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-157">.NET (formerly known as .NET Core) is designed to be cross-platform.</span></span> <span data-ttu-id="ebb1e-158">如果代码不依赖特定于 Windows 的技术，那么它可以在 macOS、Linux 和 Android 等其他平台上运行。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-158">If your code doesn't depend on Windows-specific technologies, it may run on other platforms such as macOS, Linux, and Android.</span></span> <span data-ttu-id="ebb1e-159">这包括如下项目类型：</span><span class="sxs-lookup"><span data-stu-id="ebb1e-159">This includes project types like:</span></span>

- <span data-ttu-id="ebb1e-160">库</span><span class="sxs-lookup"><span data-stu-id="ebb1e-160">Libraries</span></span>
- <span data-ttu-id="ebb1e-161">基于控制台的工具</span><span class="sxs-lookup"><span data-stu-id="ebb1e-161">Console-based tools</span></span>
- <span data-ttu-id="ebb1e-162">自动化</span><span class="sxs-lookup"><span data-stu-id="ebb1e-162">Automation</span></span>
- <span data-ttu-id="ebb1e-163">ASP.NET 站点</span><span class="sxs-lookup"><span data-stu-id="ebb1e-163">ASP.NET sites</span></span>

<span data-ttu-id="ebb1e-164">.NET Framework 是仅适用于 Windows 的组件。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-164">.NET Framework is a Windows-only component.</span></span> <span data-ttu-id="ebb1e-165">当代码使用特定于 Windows 的技术或 API（如 Windows 窗体和 Windows Presentation Foundation (WPF)）时，代码仍可以在 .NET 上运行，但不能在其他操作系统上运行。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-165">When your code uses Windows-specific technologies or APIs, such as Windows Forms and Windows Presentation Foundation (WPF), the code can still run on .NET but it won't run on other operating systems.</span></span>

<span data-ttu-id="ebb1e-166">库或基于控制台的应用程序不需要太多更改就可以跨平台使用。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-166">It's possible that your library or console-based application can be used cross-platform without changing much.</span></span> <span data-ttu-id="ebb1e-167">当移植到 .NET 时，可能需要考虑这一点，并在其他平台上测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-167">When porting to .NET, you may want to take this into consideration and test your application on other platforms.</span></span>

## <a name="the-future-of-net-standard"></a><span data-ttu-id="ebb1e-168">.NET Standard 的未来</span><span class="sxs-lookup"><span data-stu-id="ebb1e-168">The future of .NET Standard</span></span>

<span data-ttu-id="ebb1e-169">[.NET Standard](https://github.com/dotnet/standard) 是针对多个 .NET 实现推出的一套正式的 .NET API 规范。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-169">[.NET Standard](https://github.com/dotnet/standard) is a formal specification of .NET APIs that are available on multiple .NET implementations.</span></span> <span data-ttu-id="ebb1e-170">推出 .NET Standard 的背后动机是要提高 .NET 生态系统中的一致性。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-170">The motivation behind .NET Standard was to establish greater uniformity in the .NET ecosystem.</span></span> <span data-ttu-id="ebb1e-171">自 .NET 5 起，采用了一种不同的方法来建立一致性；使用这种新方法，在很多情况下，都不需要使用 .NET Standard。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-171">Starting with .NET 5, a different approach to establishing uniformity has been adopted, and this new approach eliminates the need for .NET Standard in many scenarios.</span></span> <span data-ttu-id="ebb1e-172">有关详细信息，请参阅 [.NET 5 和 .NET Standard](../../standard/net-standard.md#net-5-and-net-standard)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-172">For more information, see [.NET 5 and .NET Standard](../../standard/net-standard.md#net-5-and-net-standard).</span></span>

<span data-ttu-id="ebb1e-173">.NET Standard 2.0 是支持 .NET Framework 的最后一个版本。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-173">.NET Standard 2.0 was the last version to support .NET Framework.</span></span>

## <a name="tools-to-assist-porting"></a><span data-ttu-id="ebb1e-174">移植辅助工具</span><span class="sxs-lookup"><span data-stu-id="ebb1e-174">Tools to assist porting</span></span>

<span data-ttu-id="ebb1e-175">可以使用不同的工具来帮助自动执行迁移的某些方面，而不是将应用程序从 .NET Framework 手动移植到 .NET 中。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-175">Instead of manually porting an application from .NET Framework to .NET, you can use different tools to help automate some aspects of the migration.</span></span> <span data-ttu-id="ebb1e-176">移植复杂的项目本身就是一个复杂的过程。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-176">Porting a complex project is, in itself, a complex process.</span></span> <span data-ttu-id="ebb1e-177">这些工具可能在此过程中有所帮助。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-177">These tools may help in that journey.</span></span>

<span data-ttu-id="ebb1e-178">即使你使用工具来帮助移植应用程序，也应查阅本文中的[“移植时的注意事项”部分](#considerations-when-porting)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-178">Even if you use a tool to help port your application, you should review the [Considerations when porting section](#considerations-when-porting) in this article.</span></span>

### <a name="net-upgrade-assistant"></a><span data-ttu-id="ebb1e-179">.NET 升级助手</span><span class="sxs-lookup"><span data-stu-id="ebb1e-179">.NET Upgrade Assistant</span></span>

<span data-ttu-id="ebb1e-180">[.NET 升级助手](upgrade-assistant-overview.md)是一款可以在不同类型的 .NET Framework 应用上运行的命令行工具。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-180">The [.NET Upgrade Assistant](upgrade-assistant-overview.md) is a command-line tool that can be run on different kinds of .NET Framework apps.</span></span> <span data-ttu-id="ebb1e-181">它旨在帮助将 .NET Framework 应用升级到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-181">It's designed to assist with upgrading .NET Framework apps to .NET 5.</span></span> <span data-ttu-id="ebb1e-182">在运行此工具后，大多数情况下，应用将需要更多操作才能完成迁移。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-182">After running the tool, **in most cases the app will require more effort to complete the migration**.</span></span> <span data-ttu-id="ebb1e-183">此工具会安装可以帮助完成迁移的分析器。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-183">The tool includes the installation of analyzers that can assist with completing the migration.</span></span> <span data-ttu-id="ebb1e-184">此工具适用于以下类型的 .NET Framework 应用程序：</span><span class="sxs-lookup"><span data-stu-id="ebb1e-184">This tool works on the following types of .NET Framework applications:</span></span>

- <span data-ttu-id="ebb1e-185">Windows 窗体</span><span class="sxs-lookup"><span data-stu-id="ebb1e-185">Windows Forms</span></span>
- <span data-ttu-id="ebb1e-186">WPF</span><span class="sxs-lookup"><span data-stu-id="ebb1e-186">WPF</span></span>
- <span data-ttu-id="ebb1e-187">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="ebb1e-187">ASP.NET MVC</span></span>
- <span data-ttu-id="ebb1e-188">控制台</span><span class="sxs-lookup"><span data-stu-id="ebb1e-188">Console</span></span>
- <span data-ttu-id="ebb1e-189">类库</span><span class="sxs-lookup"><span data-stu-id="ebb1e-189">Class libraries</span></span>

<span data-ttu-id="ebb1e-190">此工具使用本文中列出的其他工具，并指导迁移过程。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-190">This tool uses the other tools listed in this article and guides the migration process.</span></span> <span data-ttu-id="ebb1e-191">若要详细了解此工具，请参阅 [.NET 升级助手概述](upgrade-assistant-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-191">For more information about the tool, see [Overview of the .NET Upgrade Assistant](upgrade-assistant-overview.md).</span></span>

### <a name="try-convert"></a><span data-ttu-id="ebb1e-192">try-convert</span><span class="sxs-lookup"><span data-stu-id="ebb1e-192">try-convert</span></span>

<span data-ttu-id="ebb1e-193">try-convert 工具是一款 .NET 全局工具，可用于将项目或整个解决方案转换为 .NET SDK，包括将桌面应用迁移到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-193">The try-convert tool is a .NET global tool that can convert a project or entire solution to the .NET SDK, including moving desktop apps to .NET 5.</span></span> <span data-ttu-id="ebb1e-194">但是，如果你的项目有复杂的生成进程（如自定义任务、目标或导入），则不建议使用此工具。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-194">However, this tool isn't recommended if your project has a complicated build process such as custom tasks, targets, or imports.</span></span>

<span data-ttu-id="ebb1e-195">有关详细信息，请参阅 [try-convert GitHub 存储库](https://github.com/dotnet/try-convert)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-195">For more information, see the [try-convert GitHub repository](https://github.com/dotnet/try-convert).</span></span>

### <a name="net-portability-analyzer"></a><span data-ttu-id="ebb1e-196">.NET 可移植性分析器</span><span class="sxs-lookup"><span data-stu-id="ebb1e-196">.NET Portability Analyzer</span></span>

<span data-ttu-id="ebb1e-197">.NET 可移植性分析器是一种工具，可分析程序集并为应用程序或库提供有关缺失的 .NET API 的详细报告，以便在指定的目标 .NET 平台上实现可移植性。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-197">The .NET Portability Analyzer is a tool that analyzes assemblies and provides a detailed report on .NET APIs that are missing for the applications or libraries to be portable on your specified targeted .NET platforms.</span></span>

<span data-ttu-id="ebb1e-198">若要使用 Visual Studio 中的 .NET 可移植性分析器，请[从市场中安装此扩展](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-198">To use the .NET Portability Analyzer in Visual Studio, install the [extension from the marketplace](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span></span>

<span data-ttu-id="ebb1e-199">有关详细信息，请参阅 [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-199">For more information, see [The .NET Portability Analyzer](../../standard/analyzers/portability-analyzer.md).</span></span>

### <a name="net-api-analyzer"></a><span data-ttu-id="ebb1e-200">.NET API 分析器</span><span class="sxs-lookup"><span data-stu-id="ebb1e-200">.NET API Analyzer</span></span>

<span data-ttu-id="ebb1e-201">[.NET API 分析器](../../standard/analyzers/api-analyzer.md)分析你是否在使用将会在运行时抛出 <xref:System.PlatformNotSupportedException> 的 API。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-201">The [.NET API Analyzer](../../standard/analyzers/api-analyzer.md) analyzes whether or not you're using an API that will throw a <xref:System.PlatformNotSupportedException> at run-time.</span></span> <span data-ttu-id="ebb1e-202">尽管这并不常见，但如果从 .NET Framework 4.7.2 或更高版本进行移动，最好进行检查。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-202">Although this isn't common if you're moving from .NET Framework 4.7.2 or higher, it's good to check.</span></span> <span data-ttu-id="ebb1e-203">若要详细了解会在 .NET 上抛出异常的 API，请参阅[始终在 .NET Core 上抛出异常的 API](../compatibility/unsupported-apis.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-203">For more information about APIs that throw exceptions on .NET, see [APIs that always throw exceptions on .NET Core](../compatibility/unsupported-apis.md).</span></span>

<span data-ttu-id="ebb1e-204">API 分析器作为 NuGet 包 [Microsoft.DotNet.Analyzers.Compatibility](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/) 添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-204">API Analyzer comes as a NuGet package [Microsoft.DotNet.Analyzers.Compatibility](https://www.nuget.org/packages/Microsoft.DotNet.Analyzers.Compatibility/) you add to your project.</span></span>

<span data-ttu-id="ebb1e-205">有关详细信息，请参阅 [.NET API 分析器](../../standard/analyzers/api-analyzer.md)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-205">For more information, see [.NET API analyzer](../../standard/analyzers/api-analyzer.md).</span></span>

## <a name="considerations-when-porting"></a><span data-ttu-id="ebb1e-206">移植时的注意事项</span><span class="sxs-lookup"><span data-stu-id="ebb1e-206">Considerations when porting</span></span>

<span data-ttu-id="ebb1e-207">将应用程序移植到 .NET 时，请按顺序考虑以下建议。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-207">When porting your application to .NET, consider the following suggestions in order.</span></span>

<span data-ttu-id="ebb1e-208">✔️ 考虑使用 [.NET 升级助手](upgrade-assistant-overview.md)来迁移项目。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-208">✔️ CONSIDER using the [.NET Upgrade Assistant](upgrade-assistant-overview.md) to migrate your projects.</span></span> <span data-ttu-id="ebb1e-209">尽管此工具处于预览阶段，但它自动执行本文中详细介绍的大部分手动步骤，并为你继续迁移路径提供了一个很好的起点。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-209">Even though this tool is in preview, it automates most of the manual steps detailed in this article and gives you a great starting point for continuing your migration path.</span></span>

<span data-ttu-id="ebb1e-210">✔️ 考虑先检查依赖项。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-210">✔️ CONSIDER examining your dependencies first.</span></span> <span data-ttu-id="ebb1e-211">依赖项必须定目标到 .NET 5、.NET Standard 或 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-211">Your dependencies must target .NET 5, .NET Standard, or .NET Core.</span></span>

<span data-ttu-id="ebb1e-212">✔️ 务必从 NuGet packages.config 文件迁移到项目文件中的 [PackageReference](/nuget/consume-packages/package-references-in-project-files) 设置。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-212">✔️ DO migrate from a NuGet _packages.config_ file to [PackageReference](/nuget/consume-packages/package-references-in-project-files) settings in the project file.</span></span> <span data-ttu-id="ebb1e-213">使用 Visual Studio [转换 package.config 文件](/nuget/consume-packages/migrate-packages-config-to-package-reference#migration-steps)。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-213">Use Visual Studio to [convert the _package.config_ file](/nuget/consume-packages/migrate-packages-config-to-package-reference#migration-steps).</span></span>

<span data-ttu-id="ebb1e-214">✔️ 考虑升级到最新的项目文件格式，即使你还不能移植应用，也不例外。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-214">✔️ CONSIDER upgrading to the latest project file format even if you can't yet port your app.</span></span> <span data-ttu-id="ebb1e-215">.NET Framework 项目使用过时的项目格式。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-215">.NET Framework projects use an outdated project format.</span></span> <span data-ttu-id="ebb1e-216">尽管最新的项目格式（称为“SDK 样式项目”）是为 .NET Core 及更高版本创建的，它们也适用于 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-216">Even though the latest project format, known as SDK-style projects, was created for .NET Core and beyond, they work with .NET Framework.</span></span> <span data-ttu-id="ebb1e-217">拥有最新格式的项目文件可以为将来移植应用打下良好的基础。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-217">Having your project file in the latest format gives you a good basis for porting your app in the future.</span></span>

<span data-ttu-id="ebb1e-218">✔️ 务必将 .NET Framework 项目重新定目标到 .NET Framework 4.7.2 及更高版本。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-218">✔️ DO retarget your .NET Framework project to at least .NET Framework 4.7.2.</span></span> <span data-ttu-id="ebb1e-219">在 .NET Standard 不支持现有 API 情况下，这可确保最新备用 API 的可用性。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-219">This ensures the availability of the latest API alternatives for cases where .NET Standard doesn't support existing APIs.</span></span>

<span data-ttu-id="ebb1e-220">✔️ 考虑定目标到 .NET 5（而不是 .NET Core 3.1）。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-220">✔️ CONSIDER targeting .NET 5 instead of .NET Core 3.1.</span></span> <span data-ttu-id="ebb1e-221">虽然 .NET Core 3.1 是长期支持 (LTS) 版本，但 .NET 5 是最新的，并且 .NET 6 也将在发布后成为 LTS。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-221">While .NET Core 3.1 is under long-term support (LTS), .NET 5 is the latest and .NET 6 will be LTS when released.</span></span>

<span data-ttu-id="ebb1e-222">✔️ 务必为 Windows 窗体和 WPF 项目定目标到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-222">✔️ DO target .NET 5 for **Windows Forms and WPF** projects.</span></span> <span data-ttu-id="ebb1e-223">.NET 5 包含许多对桌面应用的改进。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-223">.NET 5 contains many improvements for Desktop apps.</span></span>

<span data-ttu-id="ebb1e-224">✔️ 若要迁移也可以用于 .NET Framework 项目的库，请考虑定目标到 .NET Standard 2.0。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-224">✔️ CONSIDER targeting .NET Standard 2.0 if you're migrating a library that may also be used with .NET Framework projects.</span></span> <span data-ttu-id="ebb1e-225">也可以为库设定多个目标，同时定目标到 .NET Framework 和 .NET Standard。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-225">You can also multitarget your library, targeting both .NET Framework and .NET Standard.</span></span>

<span data-ttu-id="ebb1e-226">✔️ 如果迁移之后出现缺少 API 的错误，请务必添加对 [Microsoft.Windows.Compatibility NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)的引用。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-226">✔️ DO add reference to the [Microsoft.Windows.Compatibility NuGet package](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) if, after migrating, you get errors of missing APIs.</span></span> <span data-ttu-id="ebb1e-227">大部分 .NET Framework API 面是通过 NuGet 包提供给 .NET 的。</span><span class="sxs-lookup"><span data-stu-id="ebb1e-227">A large portion of the .NET Framework API surface is available to .NET via the NuGet package.</span></span>

## <a name="see-also"></a><span data-ttu-id="ebb1e-228">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ebb1e-228">See also</span></span>

- [<span data-ttu-id="ebb1e-229">.NET 升级助手概述</span><span class="sxs-lookup"><span data-stu-id="ebb1e-229">Overview of the .NET Upgrade Assistant</span></span>](upgrade-assistant-overview.md)
- [<span data-ttu-id="ebb1e-230">ASP.NET 到 ASP.NET Core 迁移</span><span class="sxs-lookup"><span data-stu-id="ebb1e-230">ASP.NET to ASP.NET Core migration</span></span>](/aspnet/core/migration/proper-to-2x)
- [<span data-ttu-id="ebb1e-231">将 .NET Framework WPF 应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="ebb1e-231">Migrate .NET Framework WPF apps to .NET</span></span>](/dotnet/desktop/wpf/migration/convert-project-from-net-framework?view=netdesktop-5.0&preserve-view=true)
- [<span data-ttu-id="ebb1e-232">将 .NET Framework Windows 窗体应用迁移到 .NET</span><span class="sxs-lookup"><span data-stu-id="ebb1e-232">Migrate .NET Framework Windows Forms apps to .NET</span></span>](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)
- [<span data-ttu-id="ebb1e-233">将 .NET Framework 库移植到 .NET 中</span><span class="sxs-lookup"><span data-stu-id="ebb1e-233">Port .NET Framework libraries to .NET</span></span>](libraries.md)
- [<span data-ttu-id="ebb1e-234">适用于服务器应用的 .NET 5 与 .NET Framework</span><span class="sxs-lookup"><span data-stu-id="ebb1e-234">.NET 5 vs. .NET Framework for server apps</span></span>](../../standard/choosing-core-framework-server.md)
