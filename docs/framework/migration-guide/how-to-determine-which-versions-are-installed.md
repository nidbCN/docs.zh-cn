---
title: 确定已安装的 .NET Framework 版本
description: 使用代码、regedit.exe 或 PowerShell，通过查询 Windows 注册表来检测在计算机上安装了哪些版本的 .NET Framework。
ms.date: 12/04/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- versions, determining for .NET Framework
- .NET Framework, determining installed versions
ms.assetid: 40a67826-e4df-4f59-a651-d9eb0fdc755d
ms.openlocfilehash: 817e7ad7b10e11675fb055b1243a8584196cc2b5
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102258619"
---
# <a name="how-to-determine-which-net-framework-versions-are-installed"></a><span data-ttu-id="83945-103">如何：确定已安装的 .NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="83945-103">How to: Determine which .NET Framework versions are installed</span></span>

<span data-ttu-id="83945-104">用户可在他们的计算机上[安装](../install/index.md)和运行 .NET Framework 的多个版本。</span><span class="sxs-lookup"><span data-stu-id="83945-104">Users can [install](../install/index.md) and run multiple versions of .NET Framework on their computers.</span></span> <span data-ttu-id="83945-105">当你开发或部署应用时，你可能需要知道用户的计算机上安装了哪些 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="83945-105">When you develop or deploy your app, you might need to know which .NET Framework versions are installed on the user's computer.</span></span> <span data-ttu-id="83945-106">注册表包含计算机上安装的 .NET Framework 版本列表。</span><span class="sxs-lookup"><span data-stu-id="83945-106">The registry contains a list of the versions of .NET Framework installed on the computer.</span></span>

> [!NOTE]
> <span data-ttu-id="83945-107">本文特定于 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="83945-107">This article is specific to .NET Framework.</span></span> <span data-ttu-id="83945-108">若要确定安装了哪些 .NET Core、.NET 5+ SDK 和运行时，请参阅[如何检查是否已安装 .NET](../../core/install/how-to-detect-installed-versions.md)。</span><span class="sxs-lookup"><span data-stu-id="83945-108">To determine which .NET Core and .NET 5+ SDKs and runtimes are installed, see [How to check that .NET is already installed](../../core/install/how-to-detect-installed-versions.md).</span></span>

<span data-ttu-id="83945-109">.NET Framework 由两个采用不同版本的主要组件构成：</span><span class="sxs-lookup"><span data-stu-id="83945-109">.NET Framework consists of two main components, which are versioned separately:</span></span>

- <span data-ttu-id="83945-110">一组程序集，它们是为应用提供功能的类型与资源的集合。</span><span class="sxs-lookup"><span data-stu-id="83945-110">A set of assemblies, which are collections of types and resources that provide the functionality for your apps.</span></span> <span data-ttu-id="83945-111">.NET Framework 和程序集使用相同的版本号。</span><span class="sxs-lookup"><span data-stu-id="83945-111">.NET Framework and the assemblies share the same version number.</span></span> <span data-ttu-id="83945-112">例如，.NET Framework 版本包括 4.5、4.6.1 和 4.7.2。</span><span class="sxs-lookup"><span data-stu-id="83945-112">For example, .NET Framework versions include 4.5, 4.6.1, and 4.7.2.</span></span>

- <span data-ttu-id="83945-113">公共语言运行时 (CLR)，可管理并执行应用代码。</span><span class="sxs-lookup"><span data-stu-id="83945-113">The common language runtime (CLR), which manages and executes your app's code.</span></span> <span data-ttu-id="83945-114">单个 CLR 版本通常可支持多个 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="83945-114">A single CLR version typically supports multiple .NET Framework versions.</span></span> <span data-ttu-id="83945-115">例如，CLR 版本4.0.30319.xxxxx（其中 xxxxx 小于42000）支持 .NET Framework 版本 4 到 4.5.2。 </span><span class="sxs-lookup"><span data-stu-id="83945-115">For example, CLR version 4.0.30319.*xxxxx* where *xxxxx* is less than 42000, supports .NET Framework versions 4 through 4.5.2.</span></span> <span data-ttu-id="83945-116">大于或等于4.0.30319.42000 的 CLR 版本支持从 .NET Framework 4.6 开始的 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="83945-116">CLR version greater than or equal to 4.0.30319.42000 supports .NET Framework versions starting with .NET Framework 4.6.</span></span>

<span data-ttu-id="83945-117">可使用社区维护的工具帮助检测安装了哪些 .NET Framework 版本：</span><span class="sxs-lookup"><span data-stu-id="83945-117">Community-maintained tools are available to help detect which .NET Framework versions are installed:</span></span>

- [https://github.com/jmalarcon/DotNetVersions](https://github.com/jmalarcon/DotNetVersions)

  <span data-ttu-id="83945-118">.NET Framework 2.0 命令行工具。</span><span class="sxs-lookup"><span data-stu-id="83945-118">A .NET Framework 2.0 command-line tool.</span></span>

- [https://github.com/EliteLoser/DotNetVersionLister](https://github.com/EliteLoser/DotNetVersionLister)

  <span data-ttu-id="83945-119">PowerShell 2.0 模块。</span><span class="sxs-lookup"><span data-stu-id="83945-119">A PowerShell 2.0 module.</span></span>

<span data-ttu-id="83945-120">要了解如何检测已安装的每个 .NET Framework 版本的更新，请参阅[操作说明：确定已安装的 .NET Framework 更新](how-to-determine-which-net-framework-updates-are-installed.md)。</span><span class="sxs-lookup"><span data-stu-id="83945-120">For information about detecting the installed updates for each version of .NET Framework, see [How to: Determine which .NET Framework updates are installed](how-to-determine-which-net-framework-updates-are-installed.md).</span></span>

## <a name="determine-which-net-implementation-and-version-an-app-is-running-on"></a><span data-ttu-id="83945-121">确定应用运行于的 .NET 实现和版本</span><span class="sxs-lookup"><span data-stu-id="83945-121">Determine which .NET implementation and version an app is running on</span></span>

<span data-ttu-id="83945-122">可以使用 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 属性来查询应用运行于的 .NET 实现和版本。</span><span class="sxs-lookup"><span data-stu-id="83945-122">You can use the <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> property to query for which .NET implementation and version your app is running on.</span></span> <span data-ttu-id="83945-123">如果应用在 .NET Framework 上运行，则输出将类似于：</span><span class="sxs-lookup"><span data-stu-id="83945-123">If the app is running on .NET Framework, the output will be similar to:</span></span>

```output
.NET Framework 4.8.4250.0
```

<span data-ttu-id="83945-124">相比之下，如果应用在 .NET Core 或 .NET 5+ 上运行，则输出将类似于：</span><span class="sxs-lookup"><span data-stu-id="83945-124">By comparison, if the app is running on .NET Core or .NET 5+, the output will be similar to:</span></span>

```output
.NET Core 3.1.9
.NET 5.0.0
```

## <a name="detect-net-framework-45-and-later-versions"></a><span data-ttu-id="83945-125">检测 .NET Framework 4.5 及更高版本</span><span class="sxs-lookup"><span data-stu-id="83945-125">Detect .NET Framework 4.5 and later versions</span></span>

<span data-ttu-id="83945-126">计算机上安装的 .NET Framework 版本（4.5 及更高版本）列出在注册表中，位于 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。</span><span class="sxs-lookup"><span data-stu-id="83945-126">The version of .NET Framework (4.5 and later) installed on a machine is listed in the registry at **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**.</span></span> <span data-ttu-id="83945-127">如果缺少 Full 子项，则未安装 .NET Framework 4.5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="83945-127">If the **Full** subkey is missing, then .NET Framework 4.5 or above isn't installed.</span></span>

> [!NOTE]
> <span data-ttu-id="83945-128">注册表路径中的 .NET Framework Setup 子项不以句点开头。</span><span class="sxs-lookup"><span data-stu-id="83945-128">The **NET Framework Setup** subkey in the registry path does *not* begin with a period.</span></span>

<span data-ttu-id="83945-129">注册表中的 **Release** REG_DWORD 值代表已安装的 .NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="83945-129">The **Release** REG_DWORD value in the registry represents the version of .NET Framework installed.</span></span>

<a name="version_table"></a>

| <span data-ttu-id="83945-130">.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="83945-130">.NET Framework version</span></span> | <span data-ttu-id="83945-131">Release 的值</span><span class="sxs-lookup"><span data-stu-id="83945-131">Value of **Release**</span></span> |
| ---------------------- | -------------------------- |
| <span data-ttu-id="83945-132">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="83945-132">.NET Framework 4.5</span></span>     | <span data-ttu-id="83945-133">所有 Windows 操作系统：378389</span><span class="sxs-lookup"><span data-stu-id="83945-133">All Windows operating systems: 378389</span></span> |
| <span data-ttu-id="83945-134">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="83945-134">.NET Framework 4.5.1</span></span>   | <span data-ttu-id="83945-135">在 Windows 8.1 和 Windows Server 2012 R2 上：378675</span><span class="sxs-lookup"><span data-stu-id="83945-135">On Windows 8.1 and Windows Server 2012 R2: 378675</span></span><br /><span data-ttu-id="83945-136">在所有其他 Windows 操作系统上：378758</span><span class="sxs-lookup"><span data-stu-id="83945-136">On all other Windows operating systems: 378758</span></span> |
| <span data-ttu-id="83945-137">.NET Framework 4.5.2</span><span class="sxs-lookup"><span data-stu-id="83945-137">.NET Framework 4.5.2</span></span>   | <span data-ttu-id="83945-138">所有 Windows 操作系统：379893</span><span class="sxs-lookup"><span data-stu-id="83945-138">All Windows operating systems: 379893</span></span> |
| <span data-ttu-id="83945-139">.NET Framework 4.6</span><span class="sxs-lookup"><span data-stu-id="83945-139">.NET Framework 4.6</span></span>     | <span data-ttu-id="83945-140">在 Windows 10 上：393295</span><span class="sxs-lookup"><span data-stu-id="83945-140">On Windows 10: 393295</span></span><br /><span data-ttu-id="83945-141">在所有其他 Windows 操作系统上：393297</span><span class="sxs-lookup"><span data-stu-id="83945-141">On all other Windows operating systems: 393297</span></span> |
| <span data-ttu-id="83945-142">.NET Framework 4.6.1</span><span class="sxs-lookup"><span data-stu-id="83945-142">.NET Framework 4.6.1</span></span>   | <span data-ttu-id="83945-143">在 Windows 10 11 月更新系统上：394254</span><span class="sxs-lookup"><span data-stu-id="83945-143">On Windows 10 November Update systems: 394254</span></span><br /><span data-ttu-id="83945-144">在所有其他 Windows 操作系统（包括 Windows 10）上：394271</span><span class="sxs-lookup"><span data-stu-id="83945-144">On all other Windows operating systems (including Windows 10): 394271</span></span> |
| <span data-ttu-id="83945-145">.NET Framework 4.6.2</span><span class="sxs-lookup"><span data-stu-id="83945-145">.NET Framework 4.6.2</span></span>   | <span data-ttu-id="83945-146">在 Windows 10 周年更新和 Windows Server 2016 上：394802</span><span class="sxs-lookup"><span data-stu-id="83945-146">On Windows 10 Anniversary Update and Windows Server 2016: 394802</span></span><br /><span data-ttu-id="83945-147">在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：394806</span><span class="sxs-lookup"><span data-stu-id="83945-147">On all other Windows operating systems (including other Windows 10 operating systems): 394806</span></span> |
| <span data-ttu-id="83945-148">.NET Framework 4.7</span><span class="sxs-lookup"><span data-stu-id="83945-148">.NET Framework 4.7</span></span>     | <span data-ttu-id="83945-149">在 Windows 10 创意者更新上：460798</span><span class="sxs-lookup"><span data-stu-id="83945-149">On Windows 10 Creators Update: 460798</span></span><br /><span data-ttu-id="83945-150">在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：460805</span><span class="sxs-lookup"><span data-stu-id="83945-150">On all other Windows operating systems (including other Windows 10 operating systems): 460805</span></span> |
| <span data-ttu-id="83945-151">.NET Framework 4.7.1</span><span class="sxs-lookup"><span data-stu-id="83945-151">.NET Framework 4.7.1</span></span>   | <span data-ttu-id="83945-152">在 Windows 10 Fall Creators Update 和 Windows Server 版本 1709 上：461308</span><span class="sxs-lookup"><span data-stu-id="83945-152">On Windows 10 Fall Creators Update and Windows Server, version 1709: 461308</span></span><br/><span data-ttu-id="83945-153">在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：461310</span><span class="sxs-lookup"><span data-stu-id="83945-153">On all other Windows operating systems (including other Windows 10 operating systems): 461310</span></span> |
| <span data-ttu-id="83945-154">.NET Framework 4.7.2</span><span class="sxs-lookup"><span data-stu-id="83945-154">.NET Framework 4.7.2</span></span>   | <span data-ttu-id="83945-155">在 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 上：461808</span><span class="sxs-lookup"><span data-stu-id="83945-155">On Windows 10 April 2018 Update and Windows Server, version 1803: 461808</span></span><br/><span data-ttu-id="83945-156">在除 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 之外的所有 Windows 操作系统上：461814</span><span class="sxs-lookup"><span data-stu-id="83945-156">On all Windows operating systems other than Windows 10 April 2018 Update and Windows Server, version 1803: 461814</span></span> |
| <span data-ttu-id="83945-157">.NET Framework 4.8</span><span class="sxs-lookup"><span data-stu-id="83945-157">.NET Framework 4.8</span></span>     | <span data-ttu-id="83945-158">在 Windows 10 2019 年 5 月更新和 Windows 10 2019 年 11 月更新上：528040</span><span class="sxs-lookup"><span data-stu-id="83945-158">On Windows 10 May 2019 Update and Windows 10 November 2019 Update: 528040</span></span><br/><span data-ttu-id="83945-159">在 Windows 10 2020 年 5 月更新和 Windows 10 2020 年 10 月更新中：528372</span><span class="sxs-lookup"><span data-stu-id="83945-159">On Windows 10 May 2020 Update and Windows 10 October 2020 Update: 528372</span></span><br/><span data-ttu-id="83945-160">在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：528049</span><span class="sxs-lookup"><span data-stu-id="83945-160">On all other Windows operating systems (including other Windows 10 operating systems): 528049</span></span> |

### <a name="minimum-version"></a><span data-ttu-id="83945-161">最低版本</span><span class="sxs-lookup"><span data-stu-id="83945-161">Minimum version</span></span>

<span data-ttu-id="83945-162">若要确定是否存在 .NET Framework 的最低版本，请查看大于或等于下列表中列出的相应值的“Release”REG_DWORD 值。</span><span class="sxs-lookup"><span data-stu-id="83945-162">To determine whether a *minimum* version of .NET Framework is present, check for a **Release** REG_DWORD value that's greater than or equal to the corresponding value listed in the following table.</span></span> <span data-ttu-id="83945-163">例如，如果应用程序在 .NET Framework 4.8 或更高版本下运行，请测试 Release REG_DWORD 值是否大于或等于 528040。</span><span class="sxs-lookup"><span data-stu-id="83945-163">For example, if your application runs under .NET Framework 4.8 or a later version, test for a **Release** REG_DWORD value that's *greater than or equal to* 528040.</span></span>

| <span data-ttu-id="83945-164">.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="83945-164">.NET Framework version</span></span> | <span data-ttu-id="83945-165">最小值</span><span class="sxs-lookup"><span data-stu-id="83945-165">Minimum value</span></span> |
| ---------------------- | ------------- |
| <span data-ttu-id="83945-166">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="83945-166">.NET Framework 4.5</span></span>     | <span data-ttu-id="83945-167">378389</span><span class="sxs-lookup"><span data-stu-id="83945-167">378389</span></span> |
| <span data-ttu-id="83945-168">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="83945-168">.NET Framework 4.5.1</span></span>   | <span data-ttu-id="83945-169">378675</span><span class="sxs-lookup"><span data-stu-id="83945-169">378675</span></span> |
| <span data-ttu-id="83945-170">.NET Framework 4.5.2</span><span class="sxs-lookup"><span data-stu-id="83945-170">.NET Framework 4.5.2</span></span>   | <span data-ttu-id="83945-171">379893</span><span class="sxs-lookup"><span data-stu-id="83945-171">379893</span></span> |
| <span data-ttu-id="83945-172">.NET Framework 4.6</span><span class="sxs-lookup"><span data-stu-id="83945-172">.NET Framework 4.6</span></span>     | <span data-ttu-id="83945-173">393295</span><span class="sxs-lookup"><span data-stu-id="83945-173">393295</span></span> |
| <span data-ttu-id="83945-174">.NET Framework 4.6.1</span><span class="sxs-lookup"><span data-stu-id="83945-174">.NET Framework 4.6.1</span></span>   | <span data-ttu-id="83945-175">394254</span><span class="sxs-lookup"><span data-stu-id="83945-175">394254</span></span> |
| <span data-ttu-id="83945-176">.NET Framework 4.6.2</span><span class="sxs-lookup"><span data-stu-id="83945-176">.NET Framework 4.6.2</span></span>   | <span data-ttu-id="83945-177">394802</span><span class="sxs-lookup"><span data-stu-id="83945-177">394802</span></span> |
| <span data-ttu-id="83945-178">.NET Framework 4.7</span><span class="sxs-lookup"><span data-stu-id="83945-178">.NET Framework 4.7</span></span>     | <span data-ttu-id="83945-179">460798</span><span class="sxs-lookup"><span data-stu-id="83945-179">460798</span></span> |
| <span data-ttu-id="83945-180">.NET Framework 4.7.1</span><span class="sxs-lookup"><span data-stu-id="83945-180">.NET Framework 4.7.1</span></span>   | <span data-ttu-id="83945-181">461308</span><span class="sxs-lookup"><span data-stu-id="83945-181">461308</span></span> |
| <span data-ttu-id="83945-182">.NET Framework 4.7.2</span><span class="sxs-lookup"><span data-stu-id="83945-182">.NET Framework 4.7.2</span></span>   | <span data-ttu-id="83945-183">461808</span><span class="sxs-lookup"><span data-stu-id="83945-183">461808</span></span> |
| <span data-ttu-id="83945-184">.NET Framework 4.8</span><span class="sxs-lookup"><span data-stu-id="83945-184">.NET Framework 4.8</span></span>     | <span data-ttu-id="83945-185">528040</span><span class="sxs-lookup"><span data-stu-id="83945-185">528040</span></span> |

### <a name="use-registry-editor"></a><span data-ttu-id="83945-186">使用注册表编辑器</span><span class="sxs-lookup"><span data-stu-id="83945-186">Use Registry Editor</span></span>

1. <span data-ttu-id="83945-187">在“开始”菜单中，选择“运行”，输入“regedit”，然后选择“确定” 。</span><span class="sxs-lookup"><span data-stu-id="83945-187">From the **Start** menu, choose **Run**, enter *regedit*, and then select **OK**.</span></span>

   <span data-ttu-id="83945-188">（必须具有管理凭据才能运行 regedit。）</span><span class="sxs-lookup"><span data-stu-id="83945-188">(You must have administrative credentials to run regedit.)</span></span>

1. <span data-ttu-id="83945-189">在注册表编辑器中，打开以下子项：HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。</span><span class="sxs-lookup"><span data-stu-id="83945-189">In the Registry Editor, open the following subkey: **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**.</span></span> <span data-ttu-id="83945-190">如果“Full”子项不存在，则表示尚未安装 .NET Framework 4.5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="83945-190">If the **Full** subkey isn't present, then you don't have .NET Framework 4.5 or later installed.</span></span>

1. <span data-ttu-id="83945-191">请检查名为“Release”的 REG_DWORD 条目。</span><span class="sxs-lookup"><span data-stu-id="83945-191">Check for a REG_DWORD entry named **Release**.</span></span> <span data-ttu-id="83945-192">如果存在，则已安装 .NET Framework 4.5 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="83945-192">If it exists, then you have .NET Framework 4.5 or later installed.</span></span> <span data-ttu-id="83945-193">其值对应于 .NET Framework 的特定版本。</span><span class="sxs-lookup"><span data-stu-id="83945-193">Its value corresponds to a particular version of .NET Framework.</span></span> <span data-ttu-id="83945-194">以下图为例，“Release”条目的值为 528040，这是 .NET Framework 4.8 的版本密钥。</span><span class="sxs-lookup"><span data-stu-id="83945-194">In the following figure, for example, the value of the **Release** entry is 528040, which is the release key for .NET Framework 4.8.</span></span>

   ![.NET Framework 4.5 的注册表项](./media/clr-installdir.png )

### <a name="use-powershell-to-check-for-a-minimum-version"></a><span data-ttu-id="83945-196">使用 PowerShell 检查最低版本</span><span class="sxs-lookup"><span data-stu-id="83945-196">Use PowerShell to check for a minimum version</span></span>

<span data-ttu-id="83945-197">使用 PowerShell 命令检查 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full 子项“Release”条目的值\\。</span><span class="sxs-lookup"><span data-stu-id="83945-197">Use PowerShell commands to check the value of the **Release** entry of the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full** subkey.</span></span>

<span data-ttu-id="83945-198">以下示例检查“Release”条目的值，以确定是否已安装 .NET Framework 4.6.2 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="83945-198">The following examples check the value of the **Release** entry to determine whether .NET Framework 4.6.2 or later is installed.</span></span> <span data-ttu-id="83945-199">如果安装了此代码，则返回 `True`，否则返回 `False`。</span><span class="sxs-lookup"><span data-stu-id="83945-199">This code returns `True` if it's installed and `False` otherwise.</span></span>

```powershell
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release -ge 394802
```

### <a name="query-the-registry-using-code"></a><span data-ttu-id="83945-200">使用代码查询注册表</span><span class="sxs-lookup"><span data-stu-id="83945-200">Query the registry using code</span></span>

01. <span data-ttu-id="83945-201">使用 <xref:Microsoft.Win32.RegistryKey.OpenBaseKey%2A?displayProperty=nameWithType> 和 <xref:Microsoft.Win32.RegistryKey.OpenSubKey%2A?displayProperty=nameWithType> 方法访问 Windows 注册表中的 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full 子项。</span><span class="sxs-lookup"><span data-stu-id="83945-201">Use the <xref:Microsoft.Win32.RegistryKey.OpenBaseKey%2A?displayProperty=nameWithType> and <xref:Microsoft.Win32.RegistryKey.OpenSubKey%2A?displayProperty=nameWithType> methods to access the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full** subkey in the Windows registry.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="83945-202">如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。</span><span class="sxs-lookup"><span data-stu-id="83945-202">If the app you're running is 32-bit and running in 64-bit Windows, the registry paths will be different than previously listed.</span></span> <span data-ttu-id="83945-203">可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。</span><span class="sxs-lookup"><span data-stu-id="83945-203">The 64-bit registry is available in the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\** subkey.</span></span> <span data-ttu-id="83945-204">例如，.NET Framework 4.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。</span><span class="sxs-lookup"><span data-stu-id="83945-204">For example, the registry subkey for .NET Framework 4.5 is **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**.</span></span>

01. <span data-ttu-id="83945-205">检查 Release REG_DWORD 值以确定已安装的版本。</span><span class="sxs-lookup"><span data-stu-id="83945-205">Check the **Release** REG_DWORD value to determine the installed version.</span></span> <span data-ttu-id="83945-206">为了向前兼容，可检查是否有一个值大于或等于 [.NET Framework 版本表](#version_table)中所列的值。</span><span class="sxs-lookup"><span data-stu-id="83945-206">To be forward-compatible, check for a value greater than or equal to the value listed in the [.NET Framework version table](#version_table).</span></span>

<span data-ttu-id="83945-207">以下示例检查注册表中“Release”条目的值，以查找已安装的 .NET Framework 4.5-4.8：</span><span class="sxs-lookup"><span data-stu-id="83945-207">The following example checks the value of the **Release** entry in the registry to find the versions of .NET Framework 4.5-4.8 that are installed:</span></span>

:::code language="csharp" source="snippets/csharp/versions-installed.cs" id="2":::

:::code language="vb" source="snippets/visual-basic/versions-installed.vb" id="2":::

<span data-ttu-id="83945-208">此示例显示类似以下内容的输出：</span><span class="sxs-lookup"><span data-stu-id="83945-208">The example displays output like the following:</span></span>

```output
.NET Framework Version: 4.6.1
```

<span data-ttu-id="83945-209">此示例遵循版本检查的建议做法：</span><span class="sxs-lookup"><span data-stu-id="83945-209">This example follows the recommended practice for version checking:</span></span>

- <span data-ttu-id="83945-210">检查“Release”条目的值是否大于或等于已知版本密钥的值。</span><span class="sxs-lookup"><span data-stu-id="83945-210">It checks whether the value of the **Release** entry is *greater than or equal to* the value of the known release keys.</span></span>
- <span data-ttu-id="83945-211">按从最新版本到最早版本的顺序检查。</span><span class="sxs-lookup"><span data-stu-id="83945-211">It checks in order from most recent version to earliest version.</span></span>

## <a name="detect-net-framework-10-through-40"></a><span data-ttu-id="83945-212">检测 .NET Framework 1.0 到 4.0</span><span class="sxs-lookup"><span data-stu-id="83945-212">Detect .NET Framework 1.0 through 4.0</span></span>

<span data-ttu-id="83945-213">.NET Framework 1.1 到 4.0 的每个版本都作为子项列出在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP。</span><span class="sxs-lookup"><span data-stu-id="83945-213">Each version of .NET Framework from 1.1 to 4.0 is listed as a subkey at **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP**.</span></span> <span data-ttu-id="83945-214">下表列出了每个 .NET Framework 版本的路径。</span><span class="sxs-lookup"><span data-stu-id="83945-214">The following table lists the path to each .NET Framework version.</span></span> <span data-ttu-id="83945-215">对于大多数版本，都有 Install REG_DWORD 值 `1`，指示已安装此版本。</span><span class="sxs-lookup"><span data-stu-id="83945-215">For most versions, there's an **Install** REG_DWORD value of `1` to indicate this version is installed.</span></span> <span data-ttu-id="83945-216">在这些子项中，还有一个包含版本字符串的 Version REG_SZ 值。</span><span class="sxs-lookup"><span data-stu-id="83945-216">In these subkeys, there's also a **Version** REG_SZ value that contains a version string.</span></span>

> [!NOTE]
> <span data-ttu-id="83945-217">注册表路径中的 .NET Framework Setup 子项不以句点开头。</span><span class="sxs-lookup"><span data-stu-id="83945-217">The **NET Framework Setup** subkey in the registry path does *not* begin with a period.</span></span>

| <span data-ttu-id="83945-218">Framework 版本</span><span class="sxs-lookup"><span data-stu-id="83945-218">Framework Version</span></span>  | <span data-ttu-id="83945-219">注册表子项</span><span class="sxs-lookup"><span data-stu-id="83945-219">Registry Subkey</span></span> | <span data-ttu-id="83945-220">“值”</span><span class="sxs-lookup"><span data-stu-id="83945-220">Value</span></span> |
| ------------------ | --------------- | ----- |
| <span data-ttu-id="83945-221">1.0</span><span class="sxs-lookup"><span data-stu-id="83945-221">1.0</span></span>                | <span data-ttu-id="83945-222">**HKLM\\Software\\Microsoft\\.NETFramework\\Policy\\v1.0\\3705**</span><span class="sxs-lookup"><span data-stu-id="83945-222">**HKLM\\Software\\Microsoft\\.NETFramework\\Policy\\v1.0\\3705**</span></span>     | <span data-ttu-id="83945-223">**Install** REG_SZ 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-223">**Install** REG_SZ equals `1`</span></span> |
| <span data-ttu-id="83945-224">1.1</span><span class="sxs-lookup"><span data-stu-id="83945-224">1.1</span></span>                | <span data-ttu-id="83945-225">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v1.1.4322**</span><span class="sxs-lookup"><span data-stu-id="83945-225">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v1.1.4322**</span></span>   | <span data-ttu-id="83945-226">**Install** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-226">**Install** REG_DWORD equals `1`</span></span> |
| <span data-ttu-id="83945-227">2.0</span><span class="sxs-lookup"><span data-stu-id="83945-227">2.0</span></span>                | <span data-ttu-id="83945-228">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v2.0.50727**</span><span class="sxs-lookup"><span data-stu-id="83945-228">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v2.0.50727**</span></span>  | <span data-ttu-id="83945-229">**Install** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-229">**Install** REG_DWORD equals `1`</span></span> |
| <span data-ttu-id="83945-230">3.0</span><span class="sxs-lookup"><span data-stu-id="83945-230">3.0</span></span>                | <span data-ttu-id="83945-231">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.0\\Setup**</span><span class="sxs-lookup"><span data-stu-id="83945-231">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.0\\Setup**</span></span> | <span data-ttu-id="83945-232">**InstallSuccess** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-232">**InstallSuccess** REG_DWORD equals `1`</span></span> |
| <span data-ttu-id="83945-233">3.5</span><span class="sxs-lookup"><span data-stu-id="83945-233">3.5</span></span>                | <span data-ttu-id="83945-234">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.5**</span><span class="sxs-lookup"><span data-stu-id="83945-234">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.5**</span></span>        | <span data-ttu-id="83945-235">**Install** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-235">**Install** REG_DWORD equals `1`</span></span> |
| <span data-ttu-id="83945-236">4.0 客户端配置文件</span><span class="sxs-lookup"><span data-stu-id="83945-236">4.0 Client Profile</span></span> | <span data-ttu-id="83945-237">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Client**</span><span class="sxs-lookup"><span data-stu-id="83945-237">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Client**</span></span>  | <span data-ttu-id="83945-238">**Install** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-238">**Install** REG_DWORD equals `1`</span></span> |
| <span data-ttu-id="83945-239">4.0 完整配置文件</span><span class="sxs-lookup"><span data-stu-id="83945-239">4.0 Full Profile</span></span>   | <span data-ttu-id="83945-240">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**</span><span class="sxs-lookup"><span data-stu-id="83945-240">**HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**</span></span>    | <span data-ttu-id="83945-241">**Install** REG_DWORD 等于 `1`</span><span class="sxs-lookup"><span data-stu-id="83945-241">**Install** REG_DWORD equals `1`</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="83945-242">如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。</span><span class="sxs-lookup"><span data-stu-id="83945-242">If the app you're running is 32-bit and running in 64-bit Windows, the registry paths will be different than previously listed.</span></span> <span data-ttu-id="83945-243">可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。</span><span class="sxs-lookup"><span data-stu-id="83945-243">The 64-bit registry is available in the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\** subkey.</span></span> <span data-ttu-id="83945-244">例如，.NET Framework 3.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5。</span><span class="sxs-lookup"><span data-stu-id="83945-244">For example, the registry subkey for .NET Framework 3.5 is **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5**.</span></span>

<span data-ttu-id="83945-245">请注意，.NET Framework 1.0 子项的注册表路径与其他子项不同。</span><span class="sxs-lookup"><span data-stu-id="83945-245">Notice that the registry path to the .NET Framework 1.0 subkey is different from the others.</span></span>

### <a name="use-registry-editor-older-framework-versions"></a><span data-ttu-id="83945-246">使用注册表编辑器（较旧的框架版本）</span><span class="sxs-lookup"><span data-stu-id="83945-246">Use Registry Editor (older framework versions)</span></span>

01. <span data-ttu-id="83945-247">在“开始”菜单中，选择“运行”，输入“regedit”，然后选择“确定” 。</span><span class="sxs-lookup"><span data-stu-id="83945-247">From the **Start** menu, choose **Run**, enter *regedit*, and then select **OK**.</span></span>

    <span data-ttu-id="83945-248">必须具有管理凭据才能运行 regedit。</span><span class="sxs-lookup"><span data-stu-id="83945-248">You must have administrative credentials to run regedit.</span></span>

01. <span data-ttu-id="83945-249">打开与要检查的版本匹配的子项。</span><span class="sxs-lookup"><span data-stu-id="83945-249">Open the subkey that matches the version you want to check.</span></span> <span data-ttu-id="83945-250">使用[检测 .NET Framework 1.0 到 4.0](#detect-net-framework-10-through-40)部分列出的表。</span><span class="sxs-lookup"><span data-stu-id="83945-250">Use the table in the [Detect .NET Framework 1.0 through 4.0](#detect-net-framework-10-through-40) section.</span></span>

    <span data-ttu-id="83945-251">下图显示了 .NET Framework 3.5 的子项及其 Version 值。</span><span class="sxs-lookup"><span data-stu-id="83945-251">The following figure shows the subkey and its **Version** value for .NET Framework 3.5.</span></span>

    <span data-ttu-id="83945-252">![.NET Framework 3.5 的注册表项。](./media/net-4-and-earlier.png ".NET Framework 3.5 及早期版本")</span><span class="sxs-lookup"><span data-stu-id="83945-252">![The registry entry for .NET Framework 3.5.](./media/net-4-and-earlier.png ".NET Framework 3.5 and earlier versions")</span></span>

### <a name="query-the-registry-using-code-older-framework-versions"></a><span data-ttu-id="83945-253">使用代码查询注册表（较旧的框架版本）</span><span class="sxs-lookup"><span data-stu-id="83945-253">Query the registry using code (older framework versions)</span></span>

<span data-ttu-id="83945-254">使用 <xref:Microsoft.Win32.RegistryKey?displayProperty=nameWithType> 类访问 Windows 注册表中的 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP 子项。</span><span class="sxs-lookup"><span data-stu-id="83945-254">Use the <xref:Microsoft.Win32.RegistryKey?displayProperty=nameWithType> class to access the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP** subkey in the Windows registry.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83945-255">如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。</span><span class="sxs-lookup"><span data-stu-id="83945-255">If the app you're running is 32-bit and running in 64-bit Windows, the registry paths will be different than previously listed.</span></span> <span data-ttu-id="83945-256">可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。</span><span class="sxs-lookup"><span data-stu-id="83945-256">The 64-bit registry is available in the **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\** subkey.</span></span> <span data-ttu-id="83945-257">例如，.NET Framework 3.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5。</span><span class="sxs-lookup"><span data-stu-id="83945-257">For example, the registry subkey for .NET Framework 3.5 is **HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5**.</span></span>

<span data-ttu-id="83945-258">以下示例查找已安装的 .NET Framework 1-4：</span><span class="sxs-lookup"><span data-stu-id="83945-258">The following example finds the versions of .NET Framework 1-4 that are installed:</span></span>

:::code language="csharp" source="snippets/csharp/versions-installed.cs" id="1":::

:::code language="vb" source="snippets/visual-basic/versions-installed.vb" id="1":::

<span data-ttu-id="83945-259">该示例显示类似于以下内容的输出：</span><span class="sxs-lookup"><span data-stu-id="83945-259">The example displays output similar to the following:</span></span>

```output
v2.0.50727  2.0.50727.4927  SP2
v3.0  3.0.30729.4926  SP2
v3.5  3.5.30729.4926  SP1
v4.0
  Client  4.0.0.0
```

## <a name="find-clr-versions"></a><span data-ttu-id="83945-260">查找 CLR 版本</span><span class="sxs-lookup"><span data-stu-id="83945-260">Find CLR versions</span></span>

<span data-ttu-id="83945-261">随 .NET Framework 一起安装 .NET Framework CLR 单独进行版本控制。</span><span class="sxs-lookup"><span data-stu-id="83945-261">The .NET Framework CLR installed with .NET Framework is versioned separately.</span></span> <span data-ttu-id="83945-262">可通过两种方式检测 .NET Framework CLR 的版本：</span><span class="sxs-lookup"><span data-stu-id="83945-262">There are two ways to detect the version of the .NET Framework CLR:</span></span>

- <span data-ttu-id="83945-263">**Clrver 工具**</span><span class="sxs-lookup"><span data-stu-id="83945-263">**The Clrver.exe tool**</span></span>

  <span data-ttu-id="83945-264">使用 [CLR 版本工具 (Clrver.exe)](../tools/clrver-exe-clr-version-tool.md) 确定计算机上安装了哪些版本的 CLR。</span><span class="sxs-lookup"><span data-stu-id="83945-264">Use the [CLR Version tool (Clrver.exe)](../tools/clrver-exe-clr-version-tool.md) to determine which versions of the CLR are installed on a computer.</span></span> <span data-ttu-id="83945-265">打开[开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)，然后输入 `clrver`。</span><span class="sxs-lookup"><span data-stu-id="83945-265">Open a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell) and enter `clrver`.</span></span>

  <span data-ttu-id="83945-266">示例输出：</span><span class="sxs-lookup"><span data-stu-id="83945-266">Sample output:</span></span>

  ```console
  Versions installed on the machine:
  v2.0.50727
  v4.0.30319
  ```

- <span data-ttu-id="83945-267">**`Environment` 类**</span><span class="sxs-lookup"><span data-stu-id="83945-267">**The `Environment` class**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="83945-268">对于 .NET Framework 4.5 及更高版本，请勿使用 <xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性来检测 CLR 的版本。</span><span class="sxs-lookup"><span data-stu-id="83945-268">For .NET Framework 4.5 and later versions, don't use the <xref:System.Environment.Version%2A?displayProperty=nameWithType> property to detect the version of the CLR.</span></span> <span data-ttu-id="83945-269">而应按照[检测 .NET Framework 4.5 及更高版本](#detect-net-framework-45-and-later-versions)中所述查询注册表。</span><span class="sxs-lookup"><span data-stu-id="83945-269">Instead, query the registry as described in [Detect .NET Framework 4.5 and later versions](#detect-net-framework-45-and-later-versions).</span></span>

  1. <span data-ttu-id="83945-270">查询 <xref:System.Environment.Version?displayProperty=nameWithType> 属性以检索 <xref:System.Version> 对象。</span><span class="sxs-lookup"><span data-stu-id="83945-270">Query the <xref:System.Environment.Version?displayProperty=nameWithType> property to retrieve a <xref:System.Version> object.</span></span>

     <span data-ttu-id="83945-271">返回的 `System.Version` 对象标识当前正在执行代码的运行时版本。</span><span class="sxs-lookup"><span data-stu-id="83945-271">The returned `System.Version` object identifies the version of the runtime that's currently executing the code.</span></span> <span data-ttu-id="83945-272">它不返回可能已安装在计算机上的程序集版本或其他运行时版本。</span><span class="sxs-lookup"><span data-stu-id="83945-272">It doesn't return assembly versions or other versions of the runtime that may have been installed on the computer.</span></span>

     <span data-ttu-id="83945-273">对于 .NET Framework 版本 4、4.5、4.5.1 和 4.5.2，返回的 <xref:System.Version>  对象的字符串表示形式为 4.0.30319.xxxxx，其中 xxxxx 小于 42000。</span><span class="sxs-lookup"><span data-stu-id="83945-273">For .NET Framework versions 4, 4.5, 4.5.1, and 4.5.2, the string representation of the returned <xref:System.Version> object has the form 4.0.30319.*xxxxx*, where *xxxxx* is less than 42000.</span></span> <span data-ttu-id="83945-274">对于 .NET Framework 4.6 及更高版本，它的格式为 4.0.30319.42000。</span><span class="sxs-lookup"><span data-stu-id="83945-274">For .NET Framework 4.6 and later versions, it has the form 4.0.30319.42000.</span></span>

  1. <span data-ttu-id="83945-275">获得 Version 对象后，按如下方式查询：</span><span class="sxs-lookup"><span data-stu-id="83945-275">After you have the **Version** object, query it as follows:</span></span>

     - <span data-ttu-id="83945-276">对于主要版本标识符（例如，4 表示版本 4.0），请使用 <xref:System.Version.Major%2A?displayProperty=nameWithType> 属性。</span><span class="sxs-lookup"><span data-stu-id="83945-276">For the major release identifier (for example, *4* for version 4.0), use the <xref:System.Version.Major%2A?displayProperty=nameWithType> property.</span></span>

     - <span data-ttu-id="83945-277">对于次要版本标识符（例如，0 表示版本 4.0），请使用 <xref:System.Version.Minor%2A?displayProperty=nameWithType> 属性</span><span class="sxs-lookup"><span data-stu-id="83945-277">For the minor release identifier (for example, *0* for version 4.0), use the <xref:System.Version.Minor%2A?displayProperty=nameWithType> property.</span></span>

     - <span data-ttu-id="83945-278">对于整个版本字符串（例如，4.0.30319.18010），请使用 <xref:System.Version.ToString%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="83945-278">For the entire version string (for example, *4.0.30319.18010*), use the <xref:System.Version.ToString%2A?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="83945-279">此方法返回一个值，该值反映正在执行代码的运行时的版本。</span><span class="sxs-lookup"><span data-stu-id="83945-279">This method returns a single value that reflects the version of the runtime that's executing the code.</span></span> <span data-ttu-id="83945-280">它不返回可能安装在计算机上的程序集版本或其他运行时版本。</span><span class="sxs-lookup"><span data-stu-id="83945-280">It doesn't return assembly versions or other runtime versions that may be installed on the computer.</span></span>

  <span data-ttu-id="83945-281">以下示例使用 <xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性检索 CLR 版本信息：</span><span class="sxs-lookup"><span data-stu-id="83945-281">The following example uses the <xref:System.Environment.Version%2A?displayProperty=nameWithType> property to retrieve CLR version information:</span></span>

  ```csharp
  Console.WriteLine($"Version: {Environment.Version}");
  ```

  ```vb
  Console.WriteLine($"Version: {Environment.Version}")
  ```

  <span data-ttu-id="83945-282">该示例显示类似于以下内容的输出：</span><span class="sxs-lookup"><span data-stu-id="83945-282">The example displays output similar to the following:</span></span>

  ```output
  Version: 4.0.30319.18010
  ```

## <a name="see-also"></a><span data-ttu-id="83945-283">请参阅</span><span class="sxs-lookup"><span data-stu-id="83945-283">See also</span></span>

- [<span data-ttu-id="83945-284">如何：确定安装了哪些 .NET Framework 更新</span><span class="sxs-lookup"><span data-stu-id="83945-284">How to: Determine which .NET Framework updates are installed</span></span>](how-to-determine-which-net-framework-updates-are-installed.md)
- [<span data-ttu-id="83945-285">安装面向开发人员的 .NET Framework</span><span class="sxs-lookup"><span data-stu-id="83945-285">Install .NET Framework for developers</span></span>](../install/guide-for-developers.md)
- [<span data-ttu-id="83945-286">.NET Framework 版本和依赖关系</span><span class="sxs-lookup"><span data-stu-id="83945-286">.NET Framework versions and dependencies</span></span>](versions-and-dependencies.md)
